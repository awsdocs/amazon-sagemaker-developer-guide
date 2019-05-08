# OutputConfig<a name="API_OutputConfig"></a>

Contains information about the output location for the compiled model and the device \(target\) that the model runs on\.

## Contents<a name="API_OutputConfig_Contents"></a>

 **S3OutputLocation**   <a name="SageMaker-Type-OutputConfig-S3OutputLocation"></a>
Identifies the S3 path where you want Amazon SageMaker to store the model artifacts\. For example, s3://bucket\-name/key\-name\-prefix\.  
Type: String  
Length Constraints: Maximum length of 1024\.  
Pattern: `^(https|s3)://([^/]+)/?(.*)$`   
Required: Yes

 **TargetDevice**   <a name="SageMaker-Type-OutputConfig-TargetDevice"></a>
Identifies the device that you want to run your model on after it has been compiled\. For example: ml\_c5\.  
Type: String  
Valid Values:` lambda | ml_m4 | ml_m5 | ml_c4 | ml_c5 | ml_p2 | ml_p3 | jetson_tx1 | jetson_tx2 | jetson_nano | rasp3b | deeplens | rk3399 | rk3288`   
Required: Yes

## See Also<a name="API_OutputConfig_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/OutputConfig) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/OutputConfig) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/OutputConfig) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/OutputConfig) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/OutputConfig) 