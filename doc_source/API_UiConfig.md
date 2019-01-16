# UiConfig<a name="API_UiConfig"></a>

Provided configuration information for the worker UI for a labeling job\. 

## Contents<a name="API_UiConfig_Contents"></a>

 **UiTemplateS3Uri**   <a name="SageMaker-Type-UiConfig-UiTemplateS3Uri"></a>
The Amazon S3 bucket location of the UI template\. For more information about the contents of a UI template, see [ Creating Your Custom Labeling Task Template](http://docs.aws.amazon.com/sagemaker/latest/dg/sms-custom-templates-step2.html)\.  
Type: String  
Length Constraints: Maximum length of 1024\.  
Pattern: `^(https|s3)://([^/]+)/?(.*)$`   
Required: Yes

## See Also<a name="API_UiConfig_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/UiConfig) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/UiConfig) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/UiConfig) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/UiConfig) 