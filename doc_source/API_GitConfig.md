# GitConfig<a name="API_GitConfig"></a>

Specifies configuration details for a Git repository in your AWS account\.

## Contents<a name="API_GitConfig_Contents"></a>

 **Branch**   <a name="SageMaker-Type-GitConfig-Branch"></a>
The default branch for the Git repository\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Required: No

 **RepositoryUrl**   <a name="SageMaker-Type-GitConfig-RepositoryUrl"></a>
The URL where the Git repository is located\.  
Type: String  
Pattern: `^https://([^/]+)/?(.*)$`   
Required: Yes

 **SecretArn**   <a name="SageMaker-Type-GitConfig-SecretArn"></a>
The Amazon Resource Name \(ARN\) of the AWS Secrets Manager secret that contains the credentials used to access the git repository\. The secret must have a staging label of `AWSCURRENT` and must be in the following format:  
 `{"username": UserName, "password": Password}`   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 2048\.  
Pattern: `arn:aws[a-z\-]*:secretsmanager:[a-z0-9\-]*:[0-9]{12}:secret:.*`   
Required: No

## See Also<a name="API_GitConfig_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/GitConfig) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/GitConfig) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/GitConfig) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/GitConfig) 