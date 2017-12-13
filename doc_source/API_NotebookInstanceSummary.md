# NotebookInstanceSummary<a name="API_NotebookInstanceSummary"></a>

Provides summary information for an Amazon SageMaker notebook instance\.

## Contents<a name="API_NotebookInstanceSummary_Contents"></a>

 **CreationTime**   
A timestamp that shows when the notebook instance was created\.  
Type: Timestamp  
Required: No

 **InstanceType**   
The type of ML compute instance that the notebook instance is running on\.  
Type: String  
Valid Values:` ml.t2.medium | ml.m4.xlarge | ml.p2.xlarge`   
Required: No

 **LastModifiedTime**   
A timestamp that shows when the notebook instance was last modified\.  
Type: Timestamp  
Required: No

 **NotebookInstanceArn**   
The Amazon Resource Name \(ARN\) of the notebook instance\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Required: Yes

 **NotebookInstanceName**   
The name of the notebook instance that you want a summary for\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

 **NotebookInstanceStatus**   
The status of the notebook instance\.  
Type: String  
Valid Values:` Pending | InService | Stopping | Stopped | Failed | Deleting`   
Required: No

 **Url**   
The URL that you use to connect to the Jupyter instance running in your notebook instance\.   
Type: String  
Required: No

## See Also<a name="API_NotebookInstanceSummary_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:

+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/NotebookInstanceSummary) 

+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/NotebookInstanceSummary) 

+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/NotebookInstanceSummary) 

+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/NotebookInstanceSummary) 