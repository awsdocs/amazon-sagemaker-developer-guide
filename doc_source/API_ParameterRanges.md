# ParameterRanges<a name="API_ParameterRanges"></a>

Specifies ranges of integer, continuous, and categorical hyperparameters that a hyperparameter tuning job searches\.

## Contents<a name="API_ParameterRanges_Contents"></a>

 **CategoricalParameterRanges**   <a name="SageMaker-Type-ParameterRanges-CategoricalParameterRanges"></a>
The array of [CategoricalParameterRange](API_CategoricalParameterRange.md) objects that specify ranges of categorical hyperparameters that a hyperparameter tuning job searches\.  
Type: Array of [CategoricalParameterRange](API_CategoricalParameterRange.md) objects  
Array Members: Minimum number of 0 items\. Maximum number of 20 items\.  
Required: No

 **ContinuousParameterRanges**   <a name="SageMaker-Type-ParameterRanges-ContinuousParameterRanges"></a>
The array of [ContinuousParameterRange](API_ContinuousParameterRange.md) objects that specify ranges of continuous hyperparameters that a hyperparameter tuning job searches\.  
Type: Array of [ContinuousParameterRange](API_ContinuousParameterRange.md) objects  
Array Members: Minimum number of 0 items\. Maximum number of 20 items\.  
Required: No

 **IntegerParameterRanges**   <a name="SageMaker-Type-ParameterRanges-IntegerParameterRanges"></a>
The array of [IntegerParameterRange](API_IntegerParameterRange.md) objects that specify ranges of integer hyperparameters that a hyperparameter tuning job searches\.  
Type: Array of [IntegerParameterRange](API_IntegerParameterRange.md) objects  
Array Members: Minimum number of 0 items\. Maximum number of 20 items\.  
Required: No

## See Also<a name="API_ParameterRanges_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ParameterRanges) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ParameterRanges) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ParameterRanges) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ParameterRanges) 