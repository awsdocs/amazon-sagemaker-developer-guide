# Delete a scaling policy<a name="endpoint-auto-scaling-delete"></a>

You can delete a scaling policy with the AWS Management Console, the AWS CLI, or the Application Auto Scaling API\. You must delete a scaling policy if you wish to update a model's endpoint\.

## Delete a scaling policy \(Console\)<a name="endpoint-auto-scaling-delete-console"></a>

**To delete an automatic scaling policy \(console\)**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. In the navigation pane, choose **Endpoints**\. 

1. Choose the endpoint for which you want to delete automatic scaling\.

1. For **Endpoint runtime settings**, choose the variant that you want to configure\.

1. Choose **Configure auto scaling**\.

1. Choose **Deregister auto scaling**\.

## Delete a scaling policy \(AWS CLI or Application Auto Scaling API\)<a name="endpoint-auto-scaling-delete-code"></a>

You can use the AWS CLI or the Application Auto Scaling API to delete a scaling policy from a variant\.

### Delete a scaling policy \(AWS CLI\)<a name="endpoint-auto-scaling-delete-code-cli"></a>

To delete a scaling policy from a variant, use the [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/delete-scaling-policy.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/delete-scaling-policy.html) AWS CLI command with the following parameters:
+ `--policy-name`—The name of the scaling policy\.
+ `--resource-id`—The resource identifier for the variant\. For this parameter, the resource type is `endpoint` and the unique identifier is the name of the variant\. For example, `endpoint/MyEndpoint/variant/MyVariant`\.
+ `--service-namespace`—Set this value to `sagemaker`\.
+ `--scalable-dimension`—Set this value to `sagemaker:variant:DesiredInstanceCount`\.

**Example**  
The following example deletes a target\-tracking scaling policy named `myscalablepolicy` from a variant named `myscalablevariant`\.  

```
aws application-autoscaling delete-scaling-policy \
    --policy-name myscalablepolicy \
    --resource-id endpoint/MyEndpoint/variant/MyVariant \
    --service-namespace sagemaker \
    --scalable-dimension sagemaker:variant:DesiredInstanceCount
```

### Delete a scaling policy \(Application Auto Scaling API\)<a name="endpoint-auto-scaling-delete-code-api"></a>

To delete a scaling policy from your variant, use the [https://docs.aws.amazon.com/autoscaling/application/APIReference/API_DeleteScalingPolicy.html](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_DeleteScalingPolicy.html) Application Auto Scaling API action with the following parameters:
+ `PolicyName`—The name of the scaling policy\.
+ `ServiceNamespace`—Set this value to `sagemaker`\.
+ `ResourceID`—The resource identifier for the variant\. For this parameter, the resource type is `endpoint` and the unique identifier is the name of the variant,\. For example, `endpoint/MyEndpoint/variant/MyVariant`\.
+ `ScalableDimension`—Set this value to `sagemaker:variant:DesiredInstanceCount`\.

**Example**  
The following example uses the Application Auto Scaling API to delete a target\-tracking scaling policy named `myscalablepolicy` from a variant named `myscalablevariant`\.  

```
POST / HTTP/1.1
Host: autoscaling.us-east-2.amazonaws.com
Accept-Encoding: identity
X-Amz-Target: AnyScaleFrontendService.DeleteScalingPolicy
X-Amz-Date: 20160506T182145Z
User-Agent: aws-cli/1.10.23 Python/2.7.11 Darwin/15.4.0 botocore/1.4.8
Content-Type: application/x-amz-json-1.1
Authorization: AUTHPARAMS

{
    "PolicyName": "myscalablepolicy",
    "ServiceNamespace": "sagemaker",
    "ResourceId": "endpoint/MyEndpoint/variant/MyVariant",
    "ScalableDimension": "sagemaker:variant:DesiredInstanceCount"
}
```