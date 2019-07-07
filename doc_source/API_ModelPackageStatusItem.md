# ModelPackageStatusItem<a name="API_ModelPackageStatusItem"></a>

Represents the overall status of a model package\.

## Contents<a name="API_ModelPackageStatusItem_Contents"></a>

 **FailureReason**   <a name="SageMaker-Type-ModelPackageStatusItem-FailureReason"></a>
if the overall status is `Failed`, the reason for the failure\.  
Type: String  
Required: No

 **Name**   <a name="SageMaker-Type-ModelPackageStatusItem-Name"></a>
The name of the model package for which the overall status is being reported\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*$`   
Required: Yes

 **Status**   <a name="SageMaker-Type-ModelPackageStatusItem-Status"></a>
The current status\.  
Type: String  
Valid Values:` NotStarted | InProgress | Completed | Failed`   
Required: Yes

## See Also<a name="API_ModelPackageStatusItem_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ModelPackageStatusItem) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ModelPackageStatusItem) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/ModelPackageStatusItem) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ModelPackageStatusItem) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ModelPackageStatusItem) 