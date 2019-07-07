# HyperParameterSpecification<a name="API_HyperParameterSpecification"></a>

Defines a hyperparameter to be used by an algorithm\.

## Contents<a name="API_HyperParameterSpecification_Contents"></a>

 **DefaultValue**   <a name="SageMaker-Type-HyperParameterSpecification-DefaultValue"></a>
The default value for this hyperparameter\. If a default value is specified, a hyperparameter cannot be required\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `.*`   
Required: No

 **Description**   <a name="SageMaker-Type-HyperParameterSpecification-Description"></a>
A brief description of the hyperparameter\.  
Type: String  
Length Constraints: Maximum length of 1024\.  
Pattern: `[\p{L}\p{M}\p{Z}\p{S}\p{N}\p{P}]*`   
Required: No

 **IsRequired**   <a name="SageMaker-Type-HyperParameterSpecification-IsRequired"></a>
Indicates whether this hyperparameter is required\.  
Type: Boolean  
Required: No

 **IsTunable**   <a name="SageMaker-Type-HyperParameterSpecification-IsTunable"></a>
Indicates whether this hyperparameter is tunable in a hyperparameter tuning job\.  
Type: Boolean  
Required: No

 **Name**   <a name="SageMaker-Type-HyperParameterSpecification-Name"></a>
The name of this hyperparameter\. The name must be unique\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `[\p{L}\p{M}\p{Z}\p{S}\p{N}\p{P}]*`   
Required: Yes

 **Range**   <a name="SageMaker-Type-HyperParameterSpecification-Range"></a>
The allowed range for this hyperparameter\.  
Type: [ParameterRange](API_ParameterRange.md) object  
Required: No

 **Type**   <a name="SageMaker-Type-HyperParameterSpecification-Type"></a>
The type of this hyperparameter\. The valid types are `Integer`, `Continuous`, `Categorical`, and `FreeText`\.  
Type: String  
Valid Values:` Integer | Continuous | Categorical | FreeText`   
Required: Yes

## See Also<a name="API_HyperParameterSpecification_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/HyperParameterSpecification) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/HyperParameterSpecification) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/HyperParameterSpecification) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/HyperParameterSpecification) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/HyperParameterSpecification) 