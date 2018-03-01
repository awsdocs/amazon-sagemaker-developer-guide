# TrainingJobDefinition<a name="API_hpo_TrainingJobDefinition"></a>

## Contents<a name="API_hpo_TrainingJobDefinition_Contents"></a>

 **AlgorithmSpecification**   
Type: [AlgorithmSpecification](API_hpo_AlgorithmSpecification.md) object  
Required: Yes

 **InputDataConfig**   
Type: Array of [Channel](API_hpo_Channel.md) objects  
Array Members: Minimum number of 1 item\. Maximum number of 8 items\.  
Required: Yes

 **OutputDataConfig**   
Type: [OutputDataConfig](API_hpo_OutputDataConfig.md) object  
Required: Yes

 **ResourceConfig**   
Type: [ResourceConfig](API_hpo_ResourceConfig.md) object  
Required: Yes

 **RoleArn**   
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.  
Required: Yes

 **StaticHyperParameters**   
Type: String to string map  
Key Length Constraints: Maximum length of 256\.  
Value Length Constraints: Maximum length of 256\.  
Required: No

 **StoppingCondition**   
Type: [StoppingCondition](API_hpo_StoppingCondition.md) object  
Required: Yes

 **Tags**   
Type: Array of [Tag](API_hpo_Tag.md) objects  
Array Members: Minimum number of 0 items\. Maximum number of 50 items\.  
Required: No

## See Also<a name="API_hpo_TrainingJobDefinition_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:

+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemakerhpo-2017-11-08/TrainingJobDefinition) 

+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemakerhpo-2017-11-08/TrainingJobDefinition) 

+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemakerhpo-2017-11-08/TrainingJobDefinition) 

+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemakerhpo-2017-11-08/TrainingJobDefinition) 