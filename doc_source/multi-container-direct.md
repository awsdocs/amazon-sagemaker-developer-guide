# Use a multi\-container endpoint with direct invocation<a name="multi-container-direct"></a>

SageMaker multi\-container endpoints enable customers to deploy multiple containers to deploy different models on a SageMaker endpoint\. You can host up to 15 different inference containers on a single endpoint\. By using direct invocation, you can send a request to a specific inference container hosted on a multi\-container endpoint\.

**Topics**
+ [Invoke a multi\-container endpoint with direct invocation](#multi-container-invoke)
+ [Security with multi\-container endpoints with direct invocation](#multi-container-security)
+ [Metrics for multi\-container endpoints with direct invocation](#multi-container-metrics)
+ [Autoscale multi\-container endpoints](#multi-container-auto-scaling)
+ [Troubleshoot multi\-container endpoints](#multi-container-troubleshooting)

## Invoke a multi\-container endpoint with direct invocation<a name="multi-container-invoke"></a>

 To invoke a multi\-container endpoint with direct invocation, call [invoke\_endpoint](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker-runtime.html#SageMakerRuntime.Client.invoke_endpoint) as you would invoke any other endpoint, and specify which container you want to invoke by using the `TargetContainerHostname` parameter\.

 

 The following example directly invokes the `secondContainer` of a multi\-container endpoint to get a prediction\.

```
import boto3
runtime_sm_client = boto3.Session().client('sagemaker-runtime')

response = runtime_sm_client.invoke_endpoint(
   EndpointName ='my-endpoint',
   ContentType = 'text/csv',
   TargetContainerHostname='secondContainer', 
   Body = body)
```

 For each direct invocation request to a multi\-container endpoint, only the container with the `TargetContainerHostname` processes the invocation request\. You will get validation errors if you do any of the following:
+ Specify a `TargetContainerHostname` that does not exist in the endpoint
+ Do not specify a value for `TargetContainerHostname` in a request to an endpoint configured for direct invocation
+ Specify a value for `TargetContainerHostname` in a request to an endpoint that is not configured for direct invocation\.

## Security with multi\-container endpoints with direct invocation<a name="multi-container-security"></a>

 For multi\-container endpoints with direct invocation, there are multiple containers hosted in a single instance by sharing memory and a storage volume\. It's your responsibility to use secure containers, maintain the correct mapping of requests to target containers, and provide users with the correct access to target containers\. SageMaker uses IAM roles to provide IAM identity\-based policies that you use to specify whether access to a resource is allowed or denied to that role, and under what conditions\. For information about IAM roles, see [IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) in the *AWS Identity and Access Management User Guide*\. For information about identity\-based policies, see [Identity\-based policies and resource\-based policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_identity-vs-resource.html)\.

By default, an IAM principal with `InvokeEndpoint` permissions on a multi\-container endpoint with direct invocation can invoke any container inside the endpoint with the endpoint name that you specify when you call `invoke_endpoint`\. If you need to restrict `invoke_endpoint` access to a limited set of containers inside a multi\-container endpoint, use the `sagemaker:TargetContainerHostname` IAM condition key\. The following policies show how to limit calls to specific containers within an endpoint\.

The following policy allows `invoke_endpoint` requests only when the value of the `TargetContainerHostname` field matches one of the specified regular expressions\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "sagemaker:InvokeEndpoint"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:sagemaker:region:account-id:endpoint/endpoint_name",
            "Condition": {
                "StringLike": {
                    "sagemaker:TargetContainerHostname": ["customIps*", "common*"]
                }
            }
        }
    ]
}
```

The following policy denies `invoke_endpoint` requests when the value of the `TargetContainerHostname` field matches one of the specified regular expressions in the `Deny` statement\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "sagemaker:InvokeEndpoint"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:sagemaker:region:account-id:endpoint/endpoint_name",
            "Condition": {
                "StringLike": {
                    "sagemaker:TargetContainerHostname": ["*"]
                }
            }
        },
        {
            "Action": [
                "sagemaker:InvokeEndpoint"
            ],
            "Effect": "Deny",
            "Resource": "arn:aws:sagemaker:region:account-id:endpoint/endpoint_name",
            "Condition": {
                "StringLike": {
                    "sagemaker:TargetContainerHostname": ["special*"]
                }
            }
        }
    ]
}
```

 For information about SageMaker condition keys, see [Condition Keys for SageMaker](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_amazonsagemaker.html#amazonsagemaker-policy-keys) in the *AWS Identity and Access Management User Guide*\.

## Metrics for multi\-container endpoints with direct invocation<a name="multi-container-metrics"></a>

In addition to the endpoint metrics that are listed in [Monitor Amazon SageMaker with Amazon CloudWatch](monitoring-cloudwatch.md), SageMaker also provides per\-container metrics\.

Per\-container metrics for multi\-container endpoints with direct invocation are located in CloudWatch and categorized into two namespaces: `AWS/SageMaker` and `aws/sagemaker/Endpoints`\. The `AWS/SageMaker` namespace includes invocation\-related metrics, and the `aws/sagemaker/Endpoints` namespace includes memory and CPU utilization metrics\.

The following table lists the per\-container metrics for multi\-container endpoints with direct invocation\. All the metrics use the \[`EndpointName, VariantName, ContainerName`\] dimension, which filters metrics at a specific endpoint, for a specific variant and corresponding to a specific container\. These metrics share the same metric names as in those for inference pipelines, but at a per\-container level \[`EndpointName, VariantName, ContainerName`\]\.

 


|  |  |  |  | 
| --- |--- |--- |--- |
|  Metric Name  |  Description  |  Dimension  |  NameSpace  | 
|  Invocations  |  The number of InvokeEndpoint requests sent to a container inside an endpoint\. To get the total number of requests sent to that container, use the Sum statistic\. Units: None Valid statistics: Sum, Sample Count |  EndpointName, VariantName, ContainerName  | AWS/SageMaker | 
|  Invocation4XX Errors  |  The number of InvokeEndpoint requests that the model returned a 4xx HTTP response code for on a specific container\. For each 4xx response, SageMaker sends a 1\. Units: None Valid statistics: Average, Sum  |  EndpointName, VariantName, ContainerName  | AWS/SageMaker | 
|  Invocation5XX Errors  |  The number of InvokeEndpoint requests that the model returned a 5xx HTTP response code for on a specific container\. For each 5xx response, SageMaker sends a 1\. Units: None Valid statistics: Average, Sum  |  EndpointName, VariantName, ContainerName  | AWS/SageMaker | 
|  ContainerLatency  |  The time it took for the target container to respond as viewed from SageMaker\. ContainerLatency includes the time it took to send the request, to fetch the response from the model's container, and to complete inference in the container\. Units: Microseconds Valid statistics: Average, Sum, Min, Max, Sample Count |  EndpointName, VariantName, ContainerName  | AWS/SageMaker | 
|  OverheadLatency  |  The time added to the time taken to respond to a client request by SageMaker for overhead\. OverheadLatency is measured from the time that SageMaker receives the request until it returns a response to the client, minus theModelLatency\. Overhead latency can vary depending on request and response payload sizes, request frequency, and authentication or authorization of the request, among other factors\. Units: Microseconds Valid statistics: Average, Sum, Min, Max, `Sample Count `  |  EndpointName, VariantName, ContainerName  | AWS/SageMaker | 
|  CPUUtilization  | The percentage of CPU units that are used by each container running on an instance\. The value ranges from 0% to 100%, and is multiplied by the number of CPUs\. For example, if there are four CPUs, CPUUtilization can range from 0% to 400%\. For endpoints with direct invocation, the number of CPUUtilization metrics equals the number of containers in that endpoint\. Units: Percent  |  EndpointName, VariantName, ContainerName  | aws/sagemaker/Endpoints | 
|  MemoryUtilizaton  |  The percentage of memory that is used by each container running on an instance\. This value ranges from 0% to 100%\. Similar as CPUUtilization, in endpoints with direct invocation, the number of MemoryUtilization metrics equals the number of containers in that endpoint\. Units: Percent  |  EndpointName, VariantName, ContainerName  | aws/sagemaker/Endpoints | 

All the metrics in the previous table are specific to multi\-container endpoints with direct invocation\. Besides these special per\-container metrics, there are also metrics at the variant level with dimension `[EndpointName, VariantName]` for all the metrics in the table expect `ContainerLatency`\.

## Autoscale multi\-container endpoints<a name="multi-container-auto-scaling"></a>

If you want to configure automatic scaling for a multi\-container endpoint using the `InvocationsPerInstance` metric, we recommend that the model in each container exhibits similar CPU utilization and latency on each inference request\. This is recommended because if traffic to the multi\-container endpoint shifts from a low CPU utilization model to a high CPU utilization model, but the overall call volume remains the same, the endpoint does not scale out and there may not be enough instances to handle all the requests to the high CPU utilization model\. For information about automatically scaling endpoints, see [Automatically Scale Amazon SageMaker Models](endpoint-auto-scaling.md)\.

## Troubleshoot multi\-container endpoints<a name="multi-container-troubleshooting"></a>

The following sections can help you troubleshoot errors with multi\-container endpoints\.

### Ping Health Check Errors<a name="multi-container-ping-errors"></a>

 With multiple containers, endpoint memory and CPU are under higher pressure during endpoint creation\. Specifically, the `MemoryUtilization` and `CPUUtilization` metrics are higher than for single\-container endpoints, because utilization pressure is proportional to the number of containers\. Because of this, we recommend that you choose instance types with enough memory and CPU to ensure that there is enough memory on the instance to have all the models loaded \(the same guidance applies to deploying an inference pipeline\)\. Otherwise, your endpoint creation might fail with an error such as `XXX did not pass the ping health check`\.

### Missing accept\-bind\-to\-port=true Docker label<a name="multi-container-missing-accept"></a>

The containers in a multi\-container endpoints listen on the port specified in the `SAGEMAKER_BIND_TO_PORT` environment variable instead of port 8080\. When a container runs in a multi\-container endpoint, SageMaker automatically provides this environment variable to the container\. If this environment variable isn't present, containers default to using port 8080\. To indicate that your container complies with this requirement, use the following command to add a label to your Dockerfile: 

```
LABEL com.amazonaws.sagemaker.capabilities.accept-bind-to-port=true
```

 Otherwise, You will see an error message such as `Your Ecr Image XXX does not contain required com.amazonaws.sagemaker.capabilities.accept-bind-to-port=true Docker label(s).`

 If your container needs to listen on a second port, choose a port in the range specified by the `SAGEMAKER_SAFE_PORT_RANGE` environment variable\. Specify the value as an inclusive range in the format *XXXX*\-*YYYY*, where XXXX and YYYY are multi\-digit integers\. SageMaker provides this value automatically when you run the container in a multi\-container endpoint\. 