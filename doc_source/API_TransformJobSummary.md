# TransformJobSummary<a name="API_TransformJobSummary"></a>

Provides a summary of a transform job\. Multiple `TransformJobSummary` objects are returned as a list after in response to a [ListTransformJobs](API_ListTransformJobs.md) call\.

## Contents<a name="API_TransformJobSummary_Contents"></a>

 **CreationTime**   <a name="SageMaker-Type-TransformJobSummary-CreationTime"></a>
A timestamp that shows when the transform Job was created\.  
Type: Timestamp  
Required: Yes

 **FailureReason**   <a name="SageMaker-Type-TransformJobSummary-FailureReason"></a>
If the transform job failed, the reason it failed\.  
Type: String  
Length Constraints: Maximum length of 1024\.  
Required: No

 **LastModifiedTime**   <a name="SageMaker-Type-TransformJobSummary-LastModifiedTime"></a>
Indicates when the transform job was last modified\.  
Type: Timestamp  
Required: No

 **TransformEndTime**   <a name="SageMaker-Type-TransformJobSummary-TransformEndTime"></a>
Indicates when the transform job ends on compute instances\. For successful jobs and stopped jobs, this is the exact time recorded after the results are uploaded\. For failed jobs, this is when Amazon SageMaker detected that the job failed\.  
Type: Timestamp  
Required: No

 **TransformJobArn**   <a name="SageMaker-Type-TransformJobSummary-TransformJobArn"></a>
The Amazon Resource Name \(ARN\) of the transform job\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:transform-job/.*`   
Required: Yes

 **TransformJobName**   <a name="SageMaker-Type-TransformJobSummary-TransformJobName"></a>
The name of the transform job\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

 **TransformJobStatus**   <a name="SageMaker-Type-TransformJobSummary-TransformJobStatus"></a>
The status of the transform job\.  
Type: String  
Valid Values:` InProgress | Completed | Failed | Stopping | Stopped`   
Required: Yes

## See Also<a name="API_TransformJobSummary_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/TransformJobSummary) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/TransformJobSummary) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/TransformJobSummary) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/TransformJobSummary) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/TransformJobSummary) 