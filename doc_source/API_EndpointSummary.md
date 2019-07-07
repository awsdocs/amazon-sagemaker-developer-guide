# EndpointSummary<a name="API_EndpointSummary"></a>

Provides summary information for an endpoint\.

## Contents<a name="API_EndpointSummary_Contents"></a>

 **CreationTime**   <a name="SageMaker-Type-EndpointSummary-CreationTime"></a>
A timestamp that shows when the endpoint was created\.  
Type: Timestamp  
Required: Yes

 **EndpointArn**   <a name="SageMaker-Type-EndpointSummary-EndpointArn"></a>
The Amazon Resource Name \(ARN\) of the endpoint\.  
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:endpoint/.*`   
Required: Yes

 **EndpointName**   <a name="SageMaker-Type-EndpointSummary-EndpointName"></a>
The name of the endpoint\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

 **EndpointStatus**   <a name="SageMaker-Type-EndpointSummary-EndpointStatus"></a>
The status of the endpoint\.  
+  `OutOfService`: Endpoint is not available to take incoming requests\.
+  `Creating`: [CreateEndpoint](API_CreateEndpoint.md) is executing\.
+  `Updating`: [UpdateEndpoint](API_UpdateEndpoint.md) or [UpdateEndpointWeightsAndCapacities](API_UpdateEndpointWeightsAndCapacities.md) is executing\.
+  `SystemUpdating`: Endpoint is undergoing maintenance and cannot be updated or deleted or re\-scaled until it has completed\. This maintenance operation does not change any customer\-specified values such as VPC config, KMS encryption, model, instance type, or instance count\.
+  `RollingBack`: Endpoint fails to scale up or down or change its variant weight and is in the process of rolling back to its previous configuration\. Once the rollback completes, endpoint returns to an `InService` status\. This transitional status only applies to an endpoint that has autoscaling enabled and is undergoing variant weight or capacity changes as part of an [UpdateEndpointWeightsAndCapacities](API_UpdateEndpointWeightsAndCapacities.md) call or when the [UpdateEndpointWeightsAndCapacities](API_UpdateEndpointWeightsAndCapacities.md) operation is called explicitly\.
+  `InService`: Endpoint is available to process incoming requests\.
+  `Deleting`: [DeleteEndpoint](API_DeleteEndpoint.md) is executing\.
+  `Failed`: Endpoint could not be created, updated, or re\-scaled\. Use [DescribeEndpoint:FailureReason](API_DescribeEndpoint.md#SageMaker-DescribeEndpoint-response-FailureReason) for information about the failure\. [DeleteEndpoint](API_DeleteEndpoint.md) is the only operation that can be performed on a failed endpoint\.
To get a list of endpoints with a specified status, use the [ListEndpoints:StatusEquals](API_ListEndpoints.md#SageMaker-ListEndpoints-request-StatusEquals) filter\.  
Type: String  
Valid Values:` OutOfService | Creating | Updating | SystemUpdating | RollingBack | InService | Deleting | Failed`   
Required: Yes

 **LastModifiedTime**   <a name="SageMaker-Type-EndpointSummary-LastModifiedTime"></a>
A timestamp that shows when the endpoint was last modified\.  
Type: Timestamp  
Required: Yes

## See Also<a name="API_EndpointSummary_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/EndpointSummary) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/EndpointSummary) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/EndpointSummary) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/EndpointSummary) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/EndpointSummary) 