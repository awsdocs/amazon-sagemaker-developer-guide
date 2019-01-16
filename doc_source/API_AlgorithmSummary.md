# AlgorithmSummary<a name="API_AlgorithmSummary"></a>

Provides summary information about an algorithm\.

## Contents<a name="API_AlgorithmSummary_Contents"></a>

 **AlgorithmArn**   <a name="SageMaker-Type-AlgorithmSummary-AlgorithmArn"></a>
The Amazon Resource Name \(ARN\) of the algorithm\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 2048\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:algorithm/.*`   
Required: Yes

 **AlgorithmDescription**   <a name="SageMaker-Type-AlgorithmSummary-AlgorithmDescription"></a>
A brief description of the algorithm\.  
Type: String  
Length Constraints: Maximum length of 1024\.  
Pattern: `[\p{L}\p{M}\p{Z}\p{S}\p{N}\p{P}]*`   
Required: No

 **AlgorithmName**   <a name="SageMaker-Type-AlgorithmSummary-AlgorithmName"></a>
The name of the algorithm that is described by the summary\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*$`   
Required: Yes

 **AlgorithmStatus**   <a name="SageMaker-Type-AlgorithmSummary-AlgorithmStatus"></a>
The overall status of the algorithm\.  
Type: String  
Valid Values:` Pending | InProgress | Completed | Failed | Deleting`   
Required: Yes

 **CreationTime**   <a name="SageMaker-Type-AlgorithmSummary-CreationTime"></a>
A timestamp that shows when the algorithm was created\.  
Type: Timestamp  
Required: Yes

## See Also<a name="API_AlgorithmSummary_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/AlgorithmSummary) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/AlgorithmSummary) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/AlgorithmSummary) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/AlgorithmSummary) 