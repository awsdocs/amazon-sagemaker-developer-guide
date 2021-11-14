# All At Once Traffic Shifting<a name="deployment-guardrails-blue-green-all-at-once"></a>

With all at once traffic shifting, you can quickly roll out an endpoint update using the safety guardrails of a blue/green deployment\. You can use this traffic shifting option to minimize the update duration while still taking advantage of the availability guarantees of blue/green deployments\. The baking period feature helps you to monitor the performance and functionality of your new instances before terminating your old instances, ensuring that your new fleet is fully operational\.

The following diagram shows how all at once traffic shifting manages the old and new fleets\.

![\[A successful 100% traffic shift from the old fleet to the new fleet.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/deployment-guardrails-blue-green-all-at-once.png)

When you use all at once traffic shifting, SageMaker routes 100% of the traffic to the new fleet \(green fleet\)\. Once the green fleet starts receiving traffic, the baking period begins\. The baking period is a set amount of time in which pre\-specified Amazon CloudWatch alarms monitor the performance of the green fleet\. If no alarms trip during the baking period, SageMaker terminates the old fleet \(blue fleet\)\. If any alarms trip during the baking period, then an auto\-rollback initiates and 100% of the traffic shifts back to the blue fleet\.

## Prerequisites<a name="deployment-guardrails-blue-green-all-at-once-prereqs"></a>

Before setting up a deployment with all at once traffic shifting, you must create Amazon CloudWatch alarms to watch metrics from your endpoint\. If any of the alarms trip during the baking period, then the traffic rolls back to your blue fleet\. To learn how to set up CloudWatch alarms on an endpoint, see the prerequisite page [Auto\-Rollback Configuration and Monitoring](deployment-guardrails-configuration.md)\. To learn more about CloudWatch alarms, see [Using Amazon CloudWatch alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) in the *Amazon CloudWatch User Guide*\.

## Configure All At Once Traffic Shifting<a name="deployment-guardrails-blue-green-all-at-once-configure"></a>

Once you are ready for your deployment and have set up CloudWatch alarms for your endpoint, you can use either the SageMaker [UpdateEndpoint](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateEndpoint.html) API or the [update\-endpoint](https://docs.aws.amazon.com/cli/latest/reference/sagemaker/update-endpoint.html) command in the AWS Command Line Interface to initiate the deployment\.

**Topics**
+ [How to update an endpoint \(API\)](#deployment-guardrails-blue-green-all-at-once-configure-api-update)
+ [How to update an endpoint with an existing blue/green update policy \(API\)](#deployment-guardrails-blue-green-all-at-once-configure-api-existing)
+ [How to update an endpoint \(CLI\)](#deployment-guardrails-blue-green-all-at-once-configure-cli-update)

### How to update an endpoint \(API\)<a name="deployment-guardrails-blue-green-all-at-once-configure-api-update"></a>

The following example shows how you can update your endpoint with all at once traffic shifting using [UpdateEndpoint](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateEndpoint.html) in the Amazon SageMaker API\.

```
import boto3
client = boto3.client("sagemaker")

response = client.update_endpoint(
    EndpointName="<your-endpoint-name>",
    EndpointConfigName="<your-config-name>",
    DeploymentConfig={
        "BlueGreenUpdatePolicy": {
            "TrafficRoutingConfiguration": {
                "Type": "ALL_AT_ONCE"
            },
            "TerminationWaitInSeconds": 600,
            "MaximumExecutionTimeoutInSeconds": 1800
        },
        "AutoRollbackConfiguration": {
            "Alarms": [
                {
                    "AlarmName": "<your-cw-alarm>"
                },
            ]
        }
    }
)
```

To configure the all at once traffic shifting option, do the following:
+ For `EndpointName`, use the name of the existing endpoint you want to update\.
+ For `EndpointConfigName`, use the name of the endpoint configuration you want to use\.
+ Under `DeploymentConfig` and `BlueGreenUpdatePolicy`, in `TrafficRoutingConfiguration`, set the `Type` parameter to `ALL_AT_ONCE`\. This specifies that the deployment uses the all at once traffic shifting mode\.
+ For `TerminationWaitInSeconds`, use `600`\. This parameter tells SageMaker to wait for the specified amount of time \(in seconds\) after your green fleet is fully active before terminating the instances in the blue fleet\. In this example, SageMaker waits for 10 minutes after the final baking period before terminating the blue fleet\.
+ For `MaximumExecutionTimeoutInSeconds`, use `1800`\. This parameter sets the maximum amount of time that the deployment can run before it times out\. In the preceding example, your deployment has a limit of 30 minutes to finish\.
+ In `AutoRollbackConfiguration`, within the `Alarms` field, you can add your CloudWatch alarms by name\. Create one `AlarmName: <your-cw-alarm>` entry for each alarm you want to use\.

### How to update an endpoint with an existing blue/green update policy \(API\)<a name="deployment-guardrails-blue-green-all-at-once-configure-api-existing"></a>

When you use the [CreateEndpoint](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html) API to create an endpoint, you can optionally specify a deployment configuration to reuse for future endpoint updates\. You can use the same `DeploymentConfig` options as the previous UpdateEndpoint API example\. There are no changes to the CreateEndpoint API behavior\. Specifying the deployment configuration does not automatically perform a blue/green update on your endpoint\.

The option to use a previous deployment configuration happens when using the [UpdateEndpoint](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateEndpoint.html) API to update your endpoint\. When updating your endpoint, you can use the `RetainDeploymentConfig` option to keep the deployment configuration you specified when you created the endpoint\.

When calling the [UpdateEndpoint](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateEndpoint.html) API, set `RetainDeploymentConfig` to `True` to keep the `DeploymentConfig` options from your original endpoint configuration\.

```
response = client.update_endpoint(
    EndpointName="<your-endpoint-name>",
    EndpointConfigName="<your-config-name>",
    RetainDeploymentConfig=True
)
```

### How to update an endpoint \(CLI\)<a name="deployment-guardrails-blue-green-all-at-once-configure-cli-update"></a>

If you are using the AWS CLI, the following example shows how to start a blue/green all at once deployment using the [update\-endpoint](https://docs.aws.amazon.com/cli/latest/reference/sagemaker/update-endpoint.html) command\.

```
update-endpoint
--endpoint-name <your-endpoint-name> 
--endpoint-config-name <your-config-name> 
--deployment-config '"BlueGreenUpdatePolicy": {"TrafficRoutingConfiguration": {"Type": "ALL_AT_ONCE"},
    "TerminationWaitInSeconds": 600, "MaximumExecutionTimeoutInSeconds": 1800},
    "AutoRollbackConfiguration": {"Alarms": [{"AlarmName": "<your-alarm>"}]}'
```

To configure the all at once traffic shifting option, do the following:
+ For `endpoint-name`, use the name of the endpoint you want to update\.
+ For `endpoint-config-name`, use the name of the endpoint configuration you want to use\.
+ For `deployment-config`, use a [BlueGreenUpdatePolicy](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_BlueGreenUpdatePolicy.html) JSON object\.

**Note**  
If you would rather save your JSON object in a file, see [Generating AWS CLI skeleton and input parameters](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-skeleton.html) in the *AWS CLI User Guide*\.