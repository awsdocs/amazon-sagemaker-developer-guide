# ContinuousParameterRange<a name="API_ContinuousParameterRange"></a>

A list of continuous hyperparameters to tune\.

## Contents<a name="API_ContinuousParameterRange_Contents"></a>

 **MaxValue**   <a name="SageMaker-Type-ContinuousParameterRange-MaxValue"></a>
The maximum value for the hyperparameter\. The tuning job uses floating\-point values between `MinValue` value and this value for tuning\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Required: Yes

 **MinValue**   <a name="SageMaker-Type-ContinuousParameterRange-MinValue"></a>
The minimum value for the hyperparameter\. The tuning job uses floating\-point values between this value and `MaxValue`for tuning\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Required: Yes

 **Name**   <a name="SageMaker-Type-ContinuousParameterRange-Name"></a>
The name of the continuous hyperparameter to tune\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Required: Yes

## See Also<a name="API_ContinuousParameterRange_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ContinuousParameterRange) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ContinuousParameterRange) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ContinuousParameterRange) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ContinuousParameterRange) 