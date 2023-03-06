# Autoscale an asynchronous endpoint<a name="async-inference-autoscale"></a>

Amazon SageMaker supports automatic scaling \(autoscaling\) your asynchronous endpoint\. Autoscaling dynamically adjusts the number of instances provisioned for a model in response to changes in your workload\. Unlike other hosted models Amazon SageMaker supports, with Asynchronous Inference you can also scale down your asynchronous endpoints instances to zero\. Requests that are received when there are zero instances are queued for processing once the endpoint scales up\.

To autoscale your asynchronous endpoint you must at a minimum:
+ Register a deployed model \(production variant\)\.
+ Define a scaling policy\.
+ Apply the autoscaling policy\.

Before you can use autoscaling, you must have already deployed a model to a SageMaker endpoint\. Deployed models are referred to as a [production variant](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ProductionVariant.html)\. See [Deploy the Model to SageMaker Hosting Services](https://docs.aws.amazon.com/sagemaker/latest/dg/ex1-model-deployment.html#ex1-deploy-model) for more information about deploying a model to an endpoint\. To specify the metrics and target values for a scaling policy, you configure a scaling policy\. For information on how to define a scaling policy, see [Define a scaling policy](https://docs.aws.amazon.com/sagemaker/latest/dg/endpoint-auto-scaling-add-code-define.html)\. After registering your model and defining a scaling policy, apply the scaling policy to the registered model\. For information on how to apply the scaling policy, see [Apply a scaling policy](https://docs.aws.amazon.com/sagemaker/latest/dg/endpoint-auto-scaling-add-code-apply.html)\.

For more information on how to define an optional additional scaling policy that scales up your endpoint upon receiving a request after your endpoint has been scaled down to zero, see [Optional: Define a scaling policy that scales up from zero for new requests](#async-inference-autoscale-scale-up)\. If you don’t specify this optional policy, then your endpoint only initiates scaling up from zero after the number of backlog requests exceeds the target tracking value\.

 For details on other prerequisites and components used with autoscaling, see the [Prerequisites](https://docs.aws.amazon.com/sagemaker/latest/dg/endpoint-auto-scaling-prerequisites.html) section in the SageMaker autoscaling documentation\.

**Note**  
If you attach multiple scaling policies to the same autoscaling group, you might have scaling conflicts\. When a conflict occurs, Amazon EC2 Auto Scaling chooses the policy that provisions the largest capacity for both scale out and scale in\. For more information about this behavior, see [Multiple dynamic scaling policies](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-scale-based-on-demand.html#multiple-scaling-policy-resolution) in the *Amazon EC2 Auto Scaling documentation*\.

## Define a scaling policy<a name="async-inference-autoscale-define-async"></a>

To specify the metrics and target values for a scaling policy, you configure a target\-tracking scaling policy\. Define the scaling policy as a JSON block in a text file\. You use that text file when invoking the AWS CLI or the Application Auto Scaling API\. For more information about policy configuration syntax, see [https://docs.aws.amazon.com/autoscaling/application/APIReference/API_TargetTrackingScalingPolicyConfiguration.html](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_TargetTrackingScalingPolicyConfiguration.html) in the Application Auto Scaling API Reference\.

For asynchronous endpoints SageMaker strongly recommends that you create a policy configuration for target\-tracking scaling for a variant\. In this configuration example, we use a custom metric, `CustomizedMetricSpecification`, called `ApproximateBacklogSizePerInstance`\.

```
TargetTrackingScalingPolicyConfiguration={
        'TargetValue': 5.0, # The target value for the metric. Here the metric is: ApproximateBacklogSizePerInstance
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

## Define a scaling policy that scales to zero<a name="async-inference-autoscale-define-async-zero"></a>

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

## Optional: Define a scaling policy that scales up from zero for new requests<a name="async-inference-autoscale-scale-up"></a>

You might have a use case where you have sporadic requests or periods with low numbers of requests\. If your endpoint has been scaled down to zero instances during these periods, then your endpoint won’t scale up again until the number of requests in the queue exceeds the target specified in your scaling policy\. This can result in long waiting times for requests in the queue\. The following section shows you how to create an additional scaling policy that scales your endpoint up from zero instances after receiving any new request in the queue\. Your endpoint will be able to respond to new requests more quickly instead of waiting for the queue size to exceed the target\.

To create a scaling policy for your endpoint that scales up from zero instances, do the following:

1. Create a scaling policy that defines the desired behavior, which is to scale up your endpoint when it’s at zero instances but has requests in the queue\. The following shows you how to define a scaling policy called `HasBacklogWithoutCapacity-ScalingPolicy` using the AWS SDK for Python \(Boto3\)\. When the queue is greater than zero and the current instance count for your endpoint is also zero, the policy scales your endpoint up\. In all other cases, the policy does not affect scaling for your endpoint\.

   ```
   response = client.put_scaling_policy(
       PolicyName="HasBacklogWithoutCapacity-ScalingPolicy",
       ServiceNamespace="sagemaker",  # The namespace of the service that provides the resource.
       ResourceId=resource_id,  # Endpoint name
       ScalableDimension="sagemaker:variant:DesiredInstanceCount",  # SageMaker supports only Instance Count
       PolicyType="StepScaling",  # 'StepScaling' or 'TargetTrackingScaling'
       StepScalingPolicyConfiguration={
           "AdjustmentType": "ChangeInCapacity", # Specifies whether the ScalingAdjustment value in the StepAdjustment property is an absolute number or a percentage of the current capacity. 
           "MetricAggregationType": "Average", # The aggregation type for the CloudWatch metrics.
           "Cooldown": 300, # The amount of time, in seconds, to wait for a previous scaling activity to take effect. 
           "StepAdjustments": # A set of adjustments that enable you to scale based on the size of the alarm breach.
           [ 
               {
                 "MetricIntervalLowerBound": 0,
                 "ScalingAdjustment": 1
               }
             ]
       },    
   )
   ```

1. Create a CloudWatch alarm with the custom metric `HasBacklogWithoutCapacity`\. When triggered, the alarm initiates the previously defined scaling policy\. For more information about the `HasBacklogWithoutCapacity` metric, see [Asynchronous Inference Endpoint Metrics](async-inference-monitor.md#async-inference-monitor-cloudwatch-async)\.

   ```
   response = cw_client.put_metric_alarm(
       AlarmName=step_scaling_policy_alarm_name,
       MetricName='HasBacklogWithoutCapacity',
       Namespace='AWS/SageMaker',
       Statistic='Average',
       EvaluationPeriods= 2,
       DatapointsToAlarm= 2,
       Threshold= 1,
       ComparisonOperator='GreaterThanOrEqualToThreshold',
       TreatMissingData='missing',
       Dimensions=[
           { 'Name':'EndpointName', 'Value':endpoint_name },
       ],
       Period= 60,
       AlarmActions=[step_scaling_policy_arn]
   )
   ```

You should now have a scaling policy and CloudWatch alarm that scale up your endpoint from zero instances whenever your queue has pending requests\.