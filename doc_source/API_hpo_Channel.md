# Channel<a name="API_hpo_Channel"></a>

## Contents<a name="API_hpo_Channel_Contents"></a>

 **ChannelName**   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 64\.  
Pattern: `[A-Za-z0-9\.\-_]+`   
Required: Yes

 **CompressionType**   
Type: String  
Valid Values:` None | Gzip`   
Required: No

 **ContentType**   
Type: String  
Length Constraints: Maximum length of 256\.  
Required: No

 **DataSource**   
Type: [DataSource](API_hpo_DataSource.md) object  
Required: Yes

 **RecordWrapperType**   
Type: String  
Valid Values:` None | RecordIO`   
Required: No

## See Also<a name="API_hpo_Channel_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:

+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemakerhpo-2017-11-08/Channel) 

+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemakerhpo-2017-11-08/Channel) 

+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemakerhpo-2017-11-08/Channel) 

+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemakerhpo-2017-11-08/Channel) 