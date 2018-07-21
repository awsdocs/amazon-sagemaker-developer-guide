# NotebookInstanceSummary<a name="API_NotebookInstanceSummary"></a>

Provides summary information for an Amazon SageMaker notebook instance\.

## Contents<a name="API_NotebookInstanceSummary_Contents"></a>

 **CreationTime**   <a name="SageMaker-Type-NotebookInstanceSummary-CreationTime"></a>
A timestamp that shows when the notebook instance was created\.  
Type: Timestamp  
Required: No

 **InstanceType**   <a name="SageMaker-Type-NotebookInstanceSummary-InstanceType"></a>
The type of ML compute instance that the notebook instance is running on\.  
Type: String  
Valid Values:` ml.t2.medium | ml.t2.large | ml.t2.xlarge | ml.t2.2xlarge | ml.m4.xlarge | ml.m4.2xlarge | ml.m4.4xlarge | ml.m4.10xlarge | ml.m4.16xlarge | ml.p2.xlarge | ml.p2.8xlarge | ml.p2.16xlarge | ml.p3.2xlarge | ml.p3.8xlarge | ml.p3.16xlarge`   
Required: No

 **LastModifiedTime**   <a name="SageMaker-Type-NotebookInstanceSummary-LastModifiedTime"></a>
A timestamp that shows when the notebook instance was last modified\.  
Type: Timestamp  
Required: No

 **NotebookInstanceArn**   <a name="SageMaker-Type-NotebookInstanceSummary-NotebookInstanceArn"></a>
The Amazon Resource Name \(ARN\) of the notebook instance\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Required: Yes

 **NotebookInstanceName**   <a name="SageMaker-Type-NotebookInstanceSummary-NotebookInstanceName"></a>
The name of the notebook instance that you want a summary for\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

 **NotebookInstanceStatus**   <a name="SageMaker-Type-NotebookInstanceSummary-NotebookInstanceStatus"></a>
The status of the notebook instance\.  
Type: String  
Valid Values:` Pending | InService | Stopping | Stopped | Failed | Deleting | Updating`   
Required: No

 **Url**   <a name="SageMaker-Type-NotebookInstanceSummary-Url"></a>
The URL that you use to connect to the Jupyter instance running in your notebook instance\.   
Type: String  
Required: No

## See Also<a name="API_NotebookInstanceSummary_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/NotebookInstanceSummary) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/NotebookInstanceSummary) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/NotebookInstanceSummary) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/NotebookInstanceSummary) 