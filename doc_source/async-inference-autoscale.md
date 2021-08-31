# Autoscale an Asynchronous Endpoint<a name="async-inference-autoscale"></a>

Amazon SageMaker supports automatic scaling \(autoscaling\) your asynchronous endpoint\. Autoscaling dynamically adjusts the number of instances provisioned for a model in response to changes in your workload\. Unlike other hosted models Amazon SageMaker supports, with Asynchronous Inference you can also scale down your asynchronous endpoints instances to zero\. Requests that are received when there are zero instances are queued for processing once the endpoint scales up\.

To autoscale your asynchronous endpoint you must at a minimum:
+ Register a deployed model \(production variant\)\.
+ Define a scaling policy\.
+ Apply the autoscaling policy\.

Before you can use autoscaling, you must have already created a SageMaker model deployment\. Deployed models are referred to as a [production variant](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ProductionVariant.html)\. See [Deploy the Model to SageMaker Hosting Services](https://docs.aws.amazon.com/sagemaker/latest/dg/ex1-model-deployment.html#ex1-deploy-model) for more information about deploying a model endpoint\. To specify the metrics and target values for a scaling policy, you configure a target\-tracking scaling policy\. For information on how to define a scaling policy, see [Define a scaling policy](https://docs.aws.amazon.com/sagemaker/latest/dg/endpoint-auto-scaling-add-code-define.html)\. After registering your model and defining a scaling policy, apply the scaling policy to the registered model\. For information on how to apply the scaling policy, see [Apply a scaling policy](https://docs.aws.amazon.com/sagemaker/latest/dg/endpoint-auto-scaling-add-code-apply.html)\.

 For details on other prerequisites and components used with autoscaling, see the [Prerequisites](https://docs.aws.amazon.com/sagemaker/latest/dg/endpoint-auto-scaling-prerequisites.html) section in the SageMaker autoscaling documentation\.

## Define a Scaling Policy<a name="async-inference-autoscale-define-async"></a>

To specify the metrics and target values for a scaling policy, you configure a target\-tracking scaling policy\. Define the scaling policy as a JSON block in a text file\. You use that text file when invoking the AWS CLI or the Application Auto Scaling API\. For more information about policy configuration syntax, see [https://docs.aws.amazon.com/autoscaling/application/APIReference/API_TargetTrackingScalingPolicyConfiguration.html](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_TargetTrackingScalingPolicyConfiguration.html) in the Application Auto Scaling API Reference\.

For asynchronous endpoints SageMaker strongly recommends that you create a policy configuration for target\-tracking scaling for a variant\. In this configuration example, we use a custom metric, `CustomizedMetricSpecification`, called `ApproximateBacklogSizePerInstance`\.

```
TargetTrackingScalingPolicyConfiguration={
        'TargetValue': 5.0, # The target value for the metric. Here the metric is: SageMakerVariantInvocationsPerInstance
        'CustomizedMetricSpecification': {
            'MetricName': 'ApproximateBacklogSizePerInstance',
            'Namespace': 'AWS/SageMaker',
            'Dimensions': [
                {'Name': 'EndpointName', 'Value': <endpoint_name> }
            ],
            'Statistic': 'Average',
        }
    }
```

## Define a Scaling Policy that Scales to 0<a name="async-inference-autoscale-define-async-zero"></a>

The following shows you how to both define and register your endpoint variant with application autoscaling using the AWS SDK for Python \(Boto3\)\. After defining a low\-level client object representing application autoscaling with Boto3, we use the [https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/application-autoscaling.html#ApplicationAutoScaling.Client.register_scalable_target](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/application-autoscaling.html#ApplicationAutoScaling.Client.register_scalable_target) method to register the production variant\. We set `MinCapacity` to 0 because Asynchronous Inference enables you to autoscale to 0 when there are no requests to process\.

```
# Common class representing application autoscaling for SageMaker 
client = boto3.client('application-autoscaling') 

# This is the format in which application autoscaling references the endpoint
resource_id='endpoint/' + <endpoint_name> + '/variant/' + <'variant1'> 

# Define and register your endpoint variant
response = client.register_scalable_target(
    ServiceNamespace='sagemaker', 
    ResourceId=resource_id,
    ScalableDimension='sagemaker:variant:DesiredInstanceCount', # The number of EC2 instances for your Amazon SageMaker model endpoint variant.
    MinCapacity=0,
    MaxCapacity=5
)
```

For detailed description about the Application Autoscaling API, see the [Application Scaling Boto3](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/application-autoscaling.html#ApplicationAutoScaling.Client.register_scalable_target) documentation\.