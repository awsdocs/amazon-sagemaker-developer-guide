# CompilationJobSummary<a name="API_CompilationJobSummary"></a>

A summary of a model compilation job\.

## Contents<a name="API_CompilationJobSummary_Contents"></a>

 **CompilationEndTime**   <a name="SageMaker-Type-CompilationJobSummary-CompilationEndTime"></a>
The time when the model compilation job completed\.  
Type: Timestamp  
Required: No

 **CompilationJobArn**   <a name="SageMaker-Type-CompilationJobSummary-CompilationJobArn"></a>
The Amazon Resource Name \(ARN\) of the model compilation job\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:compilation-job/.*`   
Required: Yes

 **CompilationJobName**   <a name="SageMaker-Type-CompilationJobSummary-CompilationJobName"></a>
The name of the model compilation job that you want a summary for\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*$`   
Required: Yes

 **CompilationJobStatus**   <a name="SageMaker-Type-CompilationJobSummary-CompilationJobStatus"></a>
The status of the model compilation job\.  
Type: String  
Valid Values:` INPROGRESS | COMPLETED | FAILED | STARTING | STOPPING | STOPPED`   
Required: Yes

 **CompilationStartTime**   <a name="SageMaker-Type-CompilationJobSummary-CompilationStartTime"></a>
The time when the model compilation job started\.  
Type: Timestamp  
Required: No

 **CompilationTargetDevice**   <a name="SageMaker-Type-CompilationJobSummary-CompilationTargetDevice"></a>
The type of device that the model will run on after compilation has completed\.  
Type: String  
Valid Values:` lambda | ml_m4 | ml_m5 | ml_c4 | ml_c5 | ml_p2 | ml_p3 | jetson_tx1 | jetson_tx2 | jetson_nano | rasp3b | deeplens | rk3399 | rk3288 | aisage | sbe_c | qcs605 | qcs603`   
Required: Yes

 **CreationTime**   <a name="SageMaker-Type-CompilationJobSummary-CreationTime"></a>
The time when the model compilation job was created\.  
Type: Timestamp  
Required: Yes

 **LastModifiedTime**   <a name="SageMaker-Type-CompilationJobSummary-LastModifiedTime"></a>
The time when the model compilation job was last modified\.  
Type: Timestamp  
Required: No

## See Also<a name="API_CompilationJobSummary_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/CompilationJobSummary) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/CompilationJobSummary) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/CompilationJobSummary) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/CompilationJobSummary) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/CompilationJobSummary) 