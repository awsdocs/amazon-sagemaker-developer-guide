# ModelPackageValidationSpecification<a name="API_ModelPackageValidationSpecification"></a>

Specifies batch transform jobs that Amazon SageMaker runs to validate your model package\.

## Contents<a name="API_ModelPackageValidationSpecification_Contents"></a>

 **ValidationProfiles**   <a name="SageMaker-Type-ModelPackageValidationSpecification-ValidationProfiles"></a>
An array of `ModelPackageValidationProfile` objects, each of which specifies a batch transform job that Amazon SageMaker runs to validate your model package\.  
Type: Array of [ModelPackageValidationProfile](API_ModelPackageValidationProfile.md) objects  
Array Members: Fixed number of 1 item\.  
Required: Yes

 **ValidationRole**   <a name="SageMaker-Type-ModelPackageValidationSpecification-ValidationRole"></a>
The IAM roles to be used for the validation of the model package\.  
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.  
Pattern: `^arn:aws[a-z\-]*:iam::\d{12}:role/?[a-zA-Z_0-9+=,.@\-_/]+$`   
Required: Yes

## See Also<a name="API_ModelPackageValidationSpecification_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ModelPackageValidationSpecification) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ModelPackageValidationSpecification) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/ModelPackageValidationSpecification) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ModelPackageValidationSpecification) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ModelPackageValidationSpecification) 