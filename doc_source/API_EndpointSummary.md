# EndpointSummary<a name="API_EndpointSummary"></a>

Provides summary information for an endpoint\.

## Contents<a name="API_EndpointSummary_Contents"></a>

 **CreationTime**   <a name="SageMaker-Type-EndpointSummary-CreationTime"></a>
A timestamp that shows when the endpoint was created\.  
Type: Timestamp  
Required: Yes

 **EndpointArn**   <a name="SageMaker-Type-EndpointSummary-EndpointArn"></a>
The Amazon Resource Name \(ARN\) of the endpoint\.  
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.  
Required: Yes

 **EndpointName**   <a name="SageMaker-Type-EndpointSummary-EndpointName"></a>
The name of the endpoint\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

 **EndpointStatus**   <a name="SageMaker-Type-EndpointSummary-EndpointStatus"></a>
The status of the endpoint\.  
Type: String  
Valid Values:` OutOfService | Creating | Updating | RollingBack | InService | Deleting | Failed`   
Required: Yes

 **LastModifiedTime**   <a name="SageMaker-Type-EndpointSummary-LastModifiedTime"></a>
A timestamp that shows when the endpoint was last modified\.  
Type: Timestamp  
Required: Yes

## See Also<a name="API_EndpointSummary_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:

+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/EndpointSummary) 

+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/EndpointSummary) 

+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/EndpointSummary) 

+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/EndpointSummary) 