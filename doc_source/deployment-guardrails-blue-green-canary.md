# Canary Traffic Shifting<a name="deployment-guardrails-blue-green-canary"></a>

With canary traffic shifting, you can test a portion of your endpoint traffic on the new fleet while the old fleet serves the remainder of the traffic\. This testing step is a safety guardrail that validates the new fleet’s functionality before shifting all of your traffic to the new fleet\. You still have the benefits of a blue/green deployment, and the added canary feature lets you ensure that your new \(green\) fleet can serve inference before letting it handle 100% of the traffic\.

The portion of your green fleet that turns on to receive traffic is called the canary, and you can choose the size of this canary\. Note that the canary size should be less than or equal to 50% of the new fleet's capacity\. Once the baking period finishes and no pre\-specified Amazon CloudWatch alarms trip, the rest of the traffic shifts from the old \(blue\) fleet to the green fleet\. Canary traffic shifting provides you with more safety during your deployment since any issues with the updated model only impact the canary\.

The following diagram shows how canary traffic shifting manages the distribution of traffic between the blue and green fleets\.

![\[A successful two step canary traffic shift from the old fleet to the new fleet.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/deployment-guardrails-blue-green-canary.png)

Once SageMaker provisions the green fleet, SageMaker routes a portion of the incoming traffic \(for example, 25%\) to the canary\. Then the baking period begins, during which your CloudWatch alarms monitor the performance of the green fleet\. During this time, both the blue fleet and green fleet are partially active and receiving traffic\. If any of the alarms trip during the baking period, then SageMaker initiates a rollback and all traffic returns to the blue fleet\. If none of the alarms trip, then all of the traffic shifts to the green fleet and there is a final baking period\. If the final baking period finishes without tripping any alarms, then the green fleet serves all traffic and SageMaker terminates the blue fleet\.

## Prerequisites<a name="deployment-guardrails-blue-green-canary-prereqs"></a>

Before setting up a deployment with canary traffic shifting, you must create Amazon CloudWatch alarms to monitor metrics from your endpoint\. The alarms are active during the baking period, and if any alarms trip, then all endpoint traffic rolls back to the blue fleet\. To learn how to set up CloudWatch alarms on an endpoint, see the prerequisite page [Auto\-Rollback Configuration and Monitoring](deployment-guardrails-configuration.md)\. To learn more about CloudWatch alarms, see [Using Amazon CloudWatch alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) in the *Amazon CloudWatch User Guide*\.

## Configure Canary Traffic Shifting<a name="deployment-guardrails-blue-green-canary-configure"></a>

Once you are ready for your deployment and have set up Amazon CloudWatch alarms for your endpoint, you can use either the Amazon SageMaker [UpdateEndpoint](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateEndpoint.html) API or the [update\-endpoint](https://docs.aws.amazon.com/cli/latest/reference/sagemaker/update-endpoint.html) command in the AWS CLI to initiate the deployment\.

**Topics**
+ [How to update an endpoint \(API\)](#deployment-guardrails-blue-green-canary-configure-api-update)
+ [How to update an endpoint with an existing blue/green update policy \(API\)](#deployment-guardrails-blue-green-canary-configure-api-existing)
+ [How to update an endpoint \(CLI\)](#deployment-guardrails-blue-green-canary-configure-cli-update)

### How to update an endpoint \(API\)<a name="deployment-guardrails-blue-green-canary-configure-api-update"></a>

The following example of the [UpdateEndpoint](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateEndpoint.html) API shows how you can update an endpoint with canary traffic shifting\.

```
import boto3
client = boto3.client("sagemaker")

response = client.update_endpoint(
    EndpointName="<your-endpoint-name>",
    EndpointConfigName="<your-config-name>",
    DeploymentConfig={
        "BlueGreenUpdatePolicy": {
            "TrafficRoutingConfiguration": {
                "Type": "CANARY",
                "CanarySize": {
                    "Type": "CAPACITY_PERCENT",
                    "Value": 30
                },
                "WaitIntervalInSeconds": 600
            },
            "TerminationWaitInSeconds": 600,
            "MaximumExecutionTimeoutInSeconds": 1800
        },
        "AutoRollbackConfiguration": {
            "Alarms": [
                {
                    "AlarmName": "<your-cw-alarm>"
                }
            ]
        }
    }
)
```

To configure the canary traffic shifting option, do the following:
+ For `EndpointName`, use the name of the existing endpoint you want to update\.
+ For `EndpointConfigName`, use the name of the endpoint configuration you want to use\.
+ Under `DeploymentConfig` and `BlueGreenUpdatePolicy`, in `TrafficRoutingConfiguration`, set the `Type` parameter to `CANARY`\. This specifies that the deployment uses canary traffic shifting\.
+ In the `CanarySize` field, you can change the size of the canary by modifying the `Type` and `Value` parameters\. For `Type`, use `CAPACITY_PERCENT`, meaning the percentage of your green fleet you want to use as the canary, and then set `Value` to `30`\. In this example, you use 30% of the green fleet’s capacity as the canary\. Note that the canary size should be equal to or less than 50% of the green fleet's capacity\.
+ For `WaitIntervalInSeconds`, use `600`\. The parameter tells SageMaker to wait for the specified amount of time \(in seconds\) between each interval shift\. This interval is the duration of the canary baking period\. In the preceding example, SageMaker waits for 10 minutes after the canary shift and then completes the second and final traffic shift\.
+ For `TerminationWaitInSeconds`, use `600`\. This parameter tells SageMaker to wait for the specified amount of time \(in seconds\) after your green fleet is fully active before terminating the instances in the blue fleet\. In this example, SageMaker waits for 10 minutes after the final baking period before terminating the blue fleet\.
+ For `MaximumExecutionTimeoutInSeconds`, use `1800`\. This parameter sets the maximum amount of time that the deployment can run before it times out\. In the preceding example, your deployment has a limit of 30 minutes to finish\.
+ In `AutoRollbackConfiguration`, within the `Alarms` field, you can add your CloudWatch alarms by name\. Create one `AlarmName: <your-cw-alarm>` entry for each alarm you want to use\.

### How to update an endpoint with an existing blue/green update policy \(API\)<a name="deployment-guardrails-blue-green-canary-configure-api-existing"></a>

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

### How to update an endpoint \(CLI\)<a name="deployment-guardrails-blue-green-canary-configure-cli-update"></a>

If you are using the AWS CLI, the following example shows how to start a blue/green canary deployment using the [update\-endpoint](https://docs.aws.amazon.com/cli/latest/reference/sagemaker/update-endpoint.html) command\.

```
update-endpoint
--endpoint-name <your-endpoint-name>
--endpoint-config-name <your-config-name> 
--deployment-config '"BlueGreenUpdatePolicy": {"TrafficRoutingConfiguration": {"Type": "CANARY",
    "CanarySize": {"Type": "CAPACITY_PERCENT", "Value": 30}, "WaitIntervalInSeconds": 600},
    "TerminationWaitInSeconds": 600, "MaximumExecutionTimeoutInSeconds": 1800},
    "AutoRollbackConfiguration": {"Alarms": [{"AlarmName": "<your-alarm>"}]}'
```

To configure the canary traffic shifting option, do the following:
+ For `endpoint-name`, use the name of the endpoint you want to update\.
+ For `endpoint-config-name`, use the name of the endpoint configuration you want to use\.
+ For `deployment-config`, use a [BlueGreenUpdatePolicy](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_BlueGreenUpdatePolicy.html) JSON object\.

**Note**  
If you would rather save your JSON object in a file, see [Generating AWS CLI skeleton and input parameters](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-skeleton.html) in the *AWS CLI User Guide*\.