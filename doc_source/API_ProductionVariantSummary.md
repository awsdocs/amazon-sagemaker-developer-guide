# ProductionVariantSummary<a name="API_ProductionVariantSummary"></a>

Describes weight and capacities for a production variant associated with an endpoint\. If you sent a request to the `UpdateEndpointWeightsAndCapacities` API and the endpoint status is `Updating`, you get different desired and current values\. 

## Contents<a name="API_ProductionVariantSummary_Contents"></a>

 **CurrentInstanceCount**   <a name="SageMaker-Type-ProductionVariantSummary-CurrentInstanceCount"></a>
The number of instances associated with the variant\.  
Type: Integer  
Valid Range: Minimum value of 1\.  
Required: No

 **CurrentWeight**   <a name="SageMaker-Type-ProductionVariantSummary-CurrentWeight"></a>
The weight associated with the variant\.  
Type: Float  
Valid Range: Minimum value of 0\.  
Required: No

 **DeployedImages**   <a name="SageMaker-Type-ProductionVariantSummary-DeployedImages"></a>
An array of `DeployedImage` objects that specify the Amazon EC2 Container Registry paths of the inference images deployed on instances of this `ProductionVariant`\.  
Type: Array of [DeployedImage](API_DeployedImage.md) objects  
Required: No

 **DesiredInstanceCount**   <a name="SageMaker-Type-ProductionVariantSummary-DesiredInstanceCount"></a>
The number of instances requested in the `UpdateEndpointWeightsAndCapacities` request\.   
Type: Integer  
Valid Range: Minimum value of 1\.  
Required: No

 **DesiredWeight**   <a name="SageMaker-Type-ProductionVariantSummary-DesiredWeight"></a>
The requested weight, as specified in the `UpdateEndpointWeightsAndCapacities` request\.   
Type: Float  
Valid Range: Minimum value of 0\.  
Required: No

 **VariantName**   <a name="SageMaker-Type-ProductionVariantSummary-VariantName"></a>
The name of the variant\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

## See Also<a name="API_ProductionVariantSummary_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ProductionVariantSummary) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ProductionVariantSummary) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/ProductionVariantSummary) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ProductionVariantSummary) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ProductionVariantSummary) 