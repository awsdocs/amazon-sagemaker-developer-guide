# ProductionVariant<a name="API_ProductionVariant"></a>

Identifies a model that you want to host and the resources to deploy for hosting it\. If you are deploying multiple models, tell Amazon SageMaker how to distribute traffic among the models by specifying variant weights\. 

## Contents<a name="API_ProductionVariant_Contents"></a>

 **AcceleratorType**   <a name="SageMaker-Type-ProductionVariant-AcceleratorType"></a>
The size of the Elastic Inference \(EI\) instance to use for the production variant\. EI instances provide on\-demand GPU computing for inference\. For more information, see [Using Elastic Inference in Amazon SageMaker](http://docs.aws.amazon.com/sagemaker/latest/dg/ei.html)\. For more information, see [Using Elastic Inference in Amazon SageMaker](http://docs.aws.amazon.com/sagemaker/latest/dg/ei.html)\.  
Type: String  
Valid Values:` ml.eia1.medium | ml.eia1.large | ml.eia1.xlarge`   
Required: No

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
Valid Values:` ml.t2.medium | ml.t2.large | ml.t2.xlarge | ml.t2.2xlarge | ml.m4.xlarge | ml.m4.2xlarge | ml.m4.4xlarge | ml.m4.10xlarge | ml.m4.16xlarge | ml.m5.large | ml.m5.xlarge | ml.m5.2xlarge | ml.m5.4xlarge | ml.m5.12xlarge | ml.m5.24xlarge | ml.c4.large | ml.c4.xlarge | ml.c4.2xlarge | ml.c4.4xlarge | ml.c4.8xlarge | ml.p2.xlarge | ml.p2.8xlarge | ml.p2.16xlarge | ml.p3.2xlarge | ml.p3.8xlarge | ml.p3.16xlarge | ml.c5.large | ml.c5.xlarge | ml.c5.2xlarge | ml.c5.4xlarge | ml.c5.9xlarge | ml.c5.18xlarge`   
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
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ProductionVariant) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ProductionVariant) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/ProductionVariant) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ProductionVariant) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ProductionVariant) 