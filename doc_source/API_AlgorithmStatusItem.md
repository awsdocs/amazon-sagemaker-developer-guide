# AlgorithmStatusItem<a name="API_AlgorithmStatusItem"></a>

Represents the overall status of an algorithm\.

## Contents<a name="API_AlgorithmStatusItem_Contents"></a>

 **FailureReason**   <a name="SageMaker-Type-AlgorithmStatusItem-FailureReason"></a>
if the overall status is `Failed`, the reason for the failure\.  
Type: String  
Required: No

 **Name**   <a name="SageMaker-Type-AlgorithmStatusItem-Name"></a>
The name of the algorithm for which the overall status is being reported\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*$`   
Required: Yes

 **Status**   <a name="SageMaker-Type-AlgorithmStatusItem-Status"></a>
The current status\.  
Type: String  
Valid Values:` NotStarted | InProgress | Completed | Failed`   
Required: Yes

## See Also<a name="API_AlgorithmStatusItem_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/AlgorithmStatusItem) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/AlgorithmStatusItem) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/AlgorithmStatusItem) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/AlgorithmStatusItem) 