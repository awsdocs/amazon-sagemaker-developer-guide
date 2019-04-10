# ContinuousParameterRange<a name="API_ContinuousParameterRange"></a>

A list of continuous hyperparameters to tune\.

## Contents<a name="API_ContinuousParameterRange_Contents"></a>

 **MaxValue**   <a name="SageMaker-Type-ContinuousParameterRange-MaxValue"></a>
The maximum value for the hyperparameter\. The tuning job uses floating\-point values between `MinValue` value and this value for tuning\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `.*`   
Required: Yes

 **MinValue**   <a name="SageMaker-Type-ContinuousParameterRange-MinValue"></a>
The minimum value for the hyperparameter\. The tuning job uses floating\-point values between this value and `MaxValue`for tuning\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `.*`   
Required: Yes

 **Name**   <a name="SageMaker-Type-ContinuousParameterRange-Name"></a>
The name of the continuous hyperparameter to tune\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `.*`   
Required: Yes

 **ScalingType**   <a name="SageMaker-Type-ContinuousParameterRange-ScalingType"></a>
The scale that hyperparameter tuning uses to search the hyperparameter range\. For information about choosing a hyperparameter scale, see [Hyperparameter Scaling](http://docs.aws.amazon.com//sagemaker/latest/dg/automatic-model-tuning-define-ranges.html#scaling-type)\. One of the following values:    
Auto  
Amazon SageMaker hyperparameter tuning chooses the best scale for the hyperparameter\.  
Linear  
Hyperparameter tuning searches the values in the hyperparameter range by using a linear scale\.  
Logarithmic  
Hyperparemeter tuning searches the values in the hyperparameter range by using a logarithmic scale\.  
Logarithmic scaling works only for ranges that have only values greater than 0\.  
ReverseLogarithmic  
Hyperparemeter tuning searches the values in the hyperparameter range by using a reverse logarithmic scale\.  
Reverse logarithmic scaling works only for ranges that are entirely within the range 0<=x<1\.0\.
Type: String  
Valid Values:` Auto | Linear | Logarithmic | ReverseLogarithmic`   
Required: No

## See Also<a name="API_ContinuousParameterRange_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ContinuousParameterRange) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ContinuousParameterRange) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/ContinuousParameterRange) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ContinuousParameterRange) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ContinuousParameterRange) 