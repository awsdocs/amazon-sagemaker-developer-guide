# Troubleshoot Inference Pipelines<a name="inference-pipeline-troubleshoot"></a>

Amazon SageMaker provides logs and error message to troubleshoot issues that arise with Inference Pipelines\.

## Troubleshoot ECR Permissions for Inference Pipelines<a name="inference-pipeline-troubleshoot-permissions"></a>

When you use your own custom Docker images in a pipeline that includes [Amazon SageMaker built\-in algorithms](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-algo-docker-registry-paths.html), you need an [Amazon ECR policy](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html)\. In particular, your repo needs to grant permission for Amazon SageMaker to pull the image\. The ECR policy must add the following permissions:

```
{
    "Version": "2008-10-17",
    "Statement": [
        {
            "Sid": "allowSageMakerToPull",
            "Effect": "Allow",
            "Principal": {
                "Service": "sagemaker.amazonaws.com"
            },
            "Action": [
                "ecr:GetDownloadUrlForLayer",
                "ecr:BatchGetImage",
                "ecr:BatchCheckLayerAvailability"
            ]
        }
    ]
}
```

## Troubleshoot with Inference Pipelines Logs<a name="inference-pipeline-troubleshoot-logs"></a>

We publish the customer container logs for endpoints deploying an inference pipeline to CloudWatch under the following path for each container\.

```
/aws/sagemaker/Endpoints/{EndpointName}/{Variant}/{InstanceId}/{ContainerHostname}
```

For example, if you want logs for the following endpoint:

```
EndpointName: MyInferencePipelinesEndpoint
Variant: MyInferencePipelinesVariant
InstanceId: i-0179208609ff7e488
ContainerHostname: MyContainerName1 and MyContainerName2
```

The logs will be in:

```
logGroup: /aws/sagemaker/Endpoints/MyInferencePipelinesEndpoint
logStream: MyInferencePipelinesVariant/i-0179208609ff7e488/MyContainerName1
logStream: MyInferencePipelinesVariant/i-0179208609ff7e488/MyContainerName2
```

To see the logs, filter on **MyInferencePipelinesEndpoint** in the CloudWatch **Log Groups** console\.

![\[An image of the CloudWatch log groups.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pipeline-log-group-filter.png)

To see the log streams, choose **MyInferencePipelinesEndpoint** in the CloudWatch **Log Groups** console and then **Search Log Group**\.

![\[An image of the CloudWatch log stream.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pipeline-log-streams-2.png)

For a listing on all of the logs provided by Amazon SageMaker, see [Logs and Metrics](inference-pipeline-logs-metrics.md)\.

## Troubleshoot with Inference Pipelines Error Messages<a name="inference-pipeline-troubleshoot-errors"></a>

We have updated the error messages to indicate which containers failed\.
+ During `InvokeEndpoint`, if the error is a ModelError \(error code 424\), we indicate which container failed instead of generically indicating the error is from \*model\*\.
+ During `InvokeEndpoint`, if the request payload \(the response from the previous container\) exceeds the limit of 5 MB, we throw as `ModelError` with a detailed error message such as: 

  Received response from MyContainerName1 with status code 200\. However, the request payload from MyContainerName1 to MyContainerName2 is 6000000 bytes, which has exceeded the maximum limit of 5 MB\. See https://us\-west\-2\.console\.aws\.amazon\.com/cloudwatch/home?region=us\-west\-2\#logEventViewer:group=/aws/sagemaker/Endpoints/MyInferencePipelinesEndpoint in account 123456789012 for more information\.
+ While running the [CreateEndpoint](API_CreateEndpoint.md), if a container fails the ping health check, Amazon SageMaker returns a `ClientError` and indicates all the containers that failed the ping check in the last health check attempt instead of just reporting on the `PrimaryContainer`\.