# ModelSummary<a name="API_ModelSummary"></a>

Provides summary information about a model\.

## Contents<a name="API_ModelSummary_Contents"></a>

 **CreationTime**   <a name="SageMaker-Type-ModelSummary-CreationTime"></a>
A timestamp that indicates when the model was created\.  
Type: Timestamp  
Required: Yes

 **ModelArn**   <a name="SageMaker-Type-ModelSummary-ModelArn"></a>
The Amazon Resource Name \(ARN\) of the model\.  
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.  
Required: Yes

 **ModelName**   <a name="SageMaker-Type-ModelSummary-ModelName"></a>
The name of the model that you want a summary for\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

## See Also<a name="API_ModelSummary_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ModelSummary) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ModelSummary) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ModelSummary) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ModelSummary) 