# AlgorithmValidationProfile<a name="API_AlgorithmValidationProfile"></a>

Defines a training job and a batch transform job that Amazon SageMaker runs to validate your algorithm\.

The data provided in the validation profile is made available to your buyers on AWS Marketplace\.

## Contents<a name="API_AlgorithmValidationProfile_Contents"></a>

 **ProfileName**   <a name="SageMaker-Type-AlgorithmValidationProfile-ProfileName"></a>
The name of the profile for the algorithm\. The name must have 1 to 63 characters\. Valid characters are a\-z, A\-Z, 0\-9, and \- \(hyphen\)\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*$`   
Required: Yes

 **TrainingJobDefinition**   <a name="SageMaker-Type-AlgorithmValidationProfile-TrainingJobDefinition"></a>
The `TrainingJobDefinition` object that describes the training job that Amazon SageMaker runs to validate your algorithm\.  
Type: [TrainingJobDefinition](API_TrainingJobDefinition.md) object  
Required: Yes

 **TransformJobDefinition**   <a name="SageMaker-Type-AlgorithmValidationProfile-TransformJobDefinition"></a>
The `TransformJobDefinition` object that describes the transform job that Amazon SageMaker runs to validate your algorithm\.  
Type: [TransformJobDefinition](API_TransformJobDefinition.md) object  
Required: No

## See Also<a name="API_AlgorithmValidationProfile_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/AlgorithmValidationProfile) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/AlgorithmValidationProfile) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/AlgorithmValidationProfile) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/AlgorithmValidationProfile) 