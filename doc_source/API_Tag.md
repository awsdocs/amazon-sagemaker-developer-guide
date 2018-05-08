# Tag<a name="API_Tag"></a>

Describes a tag\. 

## Contents<a name="API_Tag_Contents"></a>

 **Key**   <a name="SageMaker-Type-Tag-Key"></a>
The tag key\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 128\.  
Pattern: `^((?!aws:)[\p{L}\p{Z}\p{N}_.:/=+\-@]*)$`   
Required: Yes

 **Value**   <a name="SageMaker-Type-Tag-Value"></a>
The tag value\.  
Type: String  
Length Constraints: Minimum length of 0\. Maximum length of 256\.  
Pattern: `^([\p{L}\p{Z}\p{N}_.:/=+\-@]*)$`   
Required: Yes

## See Also<a name="API_Tag_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/Tag) 
+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/Tag) 
+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/Tag) 
+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/Tag) 