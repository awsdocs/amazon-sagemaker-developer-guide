# ChannelSpecification<a name="API_ChannelSpecification"></a>

Defines a named input source, called a channel, to be used by an algorithm\.

## Contents<a name="API_ChannelSpecification_Contents"></a>

 **Description**   <a name="SageMaker-Type-ChannelSpecification-Description"></a>
A brief description of the channel\.  
Type: String  
Length Constraints: Maximum length of 1024\.  
Pattern: `[\p{L}\p{M}\p{Z}\p{S}\p{N}\p{P}]*`   
Required: No

 **IsRequired**   <a name="SageMaker-Type-ChannelSpecification-IsRequired"></a>
Indicates whether the channel is required by the algorithm\.  
Type: Boolean  
Required: No

 **Name**   <a name="SageMaker-Type-ChannelSpecification-Name"></a>
The name of the channel\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 64\.  
Pattern: `[A-Za-z0-9\.\-_]+`   
Required: Yes

 **SupportedCompressionTypes**   <a name="SageMaker-Type-ChannelSpecification-SupportedCompressionTypes"></a>
The allowed compression types, if data compression is used\.  
Type: Array of strings  
Valid Values:` None | Gzip`   
Required: No

 **SupportedContentTypes**   <a name="SageMaker-Type-ChannelSpecification-SupportedContentTypes"></a>
The supported MIME types for the data\.  
Type: Array of strings  
Length Constraints: Maximum length of 256\.  
Pattern: `.*`   
Required: Yes

 **SupportedInputModes**   <a name="SageMaker-Type-ChannelSpecification-SupportedInputModes"></a>
The allowed input mode, either FILE or PIPE\.  
In FILE mode, Amazon SageMaker copies the data from the input source onto the local Amazon Elastic Block Store \(Amazon EBS\) volumes before starting your training algorithm\. This is the most commonly used input mode\.  
In PIPE mode, Amazon SageMaker streams input data from the source directly to your algorithm without using the EBS volume\.  
Type: Array of strings  
Array Members: Minimum number of 1 item\.  
Valid Values:` Pipe | File`   
Required: Yes

## See Also<a name="API_ChannelSpecification_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ChannelSpecification) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ChannelSpecification) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/ChannelSpecification) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ChannelSpecification) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ChannelSpecification) 