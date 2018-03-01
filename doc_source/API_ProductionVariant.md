# ProductionVariant<a name="API_ProductionVariant"></a>

Identifies a model that you want to host and the resources to deploy for hosting it\. If you are deploying multiple models, tell Amazon SageMaker how to distribute traffic among the models by specifying variant weights\. 

## Contents<a name="API_ProductionVariant_Contents"></a>

 **InitialInstanceCount**   <a name="SageMaker-Type-ProductionVariant-InitialInstanceCount"></a>
Number of instances to launch initially\.  
Type: Integer  
Valid Range: Minimum value of 1\.  
Required: Yes

 **InitialVariantWeight**   <a name="SageMaker-Type-ProductionVariant-InitialVariantWeight"></a>
Determines initial traffic distribution among all of the models that you specify in the endpoint configuration\. The traffic to a production variant is determined by the ratio of the `VariantWeight` to the sum of all `VariantWeight` values across all ProductionVariants\. If unspecified, it defaults to 1\.0\.   
Type: Float  
Valid Range: Minimum value of 0\.  
Required: No

 **InstanceType**   <a name="SageMaker-Type-ProductionVariant-InstanceType"></a>
The ML compute instance type\.  
Type: String  
Valid Values:` ml.c4.2xlarge | ml.c4.8xlarge | ml.c4.xlarge | ml.c5.2xlarge | ml.c5.9xlarge | ml.c5.xlarge | ml.m4.xlarge | ml.p2.xlarge | ml.p3.2xlarge | ml.t2.medium`   
Required: Yes

 **ModelName**   <a name="SageMaker-Type-ProductionVariant-ModelName"></a>
The name of the model that you want to host\. This is the name that you specified when creating the model\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

 **VariantName**   <a name="SageMaker-Type-ProductionVariant-VariantName"></a>
The name of the production variant\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

## See Also<a name="API_ProductionVariant_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:

+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ProductionVariant) 

+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ProductionVariant) 

+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ProductionVariant) 

+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ProductionVariant) 