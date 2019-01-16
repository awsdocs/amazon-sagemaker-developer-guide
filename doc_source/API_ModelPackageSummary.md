# ModelPackageSummary<a name="API_ModelPackageSummary"></a>

Provides summary information about a model package\.

## Contents<a name="API_ModelPackageSummary_Contents"></a>

 **CreationTime**   <a name="SageMaker-Type-ModelPackageSummary-CreationTime"></a>
A timestamp that shows when the model package was created\.  
Type: Timestamp  
Required: Yes

 **ModelPackageArn**   <a name="SageMaker-Type-ModelPackageSummary-ModelPackageArn"></a>
The Amazon Resource Name \(ARN\) of the model package\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 2048\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:model-package/.*`   
Required: Yes

 **ModelPackageDescription**   <a name="SageMaker-Type-ModelPackageSummary-ModelPackageDescription"></a>
A brief description of the model package\.  
Type: String  
Length Constraints: Maximum length of 1024\.  
Pattern: `[\p{L}\p{M}\p{Z}\p{S}\p{N}\p{P}]*`   
Required: No

 **ModelPackageName**   <a name="SageMaker-Type-ModelPackageSummary-ModelPackageName"></a>
The name of the model package\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*$`   
Required: Yes

 **ModelPackageStatus**   <a name="SageMaker-Type-ModelPackageSummary-ModelPackageStatus"></a>
The overall status of the model package\.  
Type: String  
Valid Values:` Pending | InProgress | Completed | Failed | Deleting`   
Required: Yes

## See Also<a name="API_ModelPackageSummary_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ModelPackageSummary) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ModelPackageSummary) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ModelPackageSummary) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ModelPackageSummary) 