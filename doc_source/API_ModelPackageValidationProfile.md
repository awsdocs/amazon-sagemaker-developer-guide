# ModelPackageValidationProfile<a name="API_ModelPackageValidationProfile"></a>

Contains data, such as the inputs and targeted instance types that are used in the process of validating the model package\.

The data provided in the validation profile is made available to your buyers on AWS Marketplace\.

## Contents<a name="API_ModelPackageValidationProfile_Contents"></a>

 **ProfileName**   <a name="SageMaker-Type-ModelPackageValidationProfile-ProfileName"></a>
The name of the profile for the model package\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*$`   
Required: Yes

 **TransformJobDefinition**   <a name="SageMaker-Type-ModelPackageValidationProfile-TransformJobDefinition"></a>
The `TransformJobDefinition` object that describes the transform job used for the validation of the model package\.  
Type: [TransformJobDefinition](API_TransformJobDefinition.md) object  
Required: Yes

## See Also<a name="API_ModelPackageValidationProfile_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ModelPackageValidationProfile) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ModelPackageValidationProfile) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/ModelPackageValidationProfile) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ModelPackageValidationProfile) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ModelPackageValidationProfile) 