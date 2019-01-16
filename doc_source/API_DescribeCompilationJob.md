# DescribeCompilationJob<a name="API_DescribeCompilationJob"></a>

Returns information about a model compilation job\.

To create a model compilation job, use [CreateCompilationJob](API_CreateCompilationJob.md)\. To get information about multiple model compilation jobs, use [ListCompilationJobs](API_ListCompilationJobs.md)\.

## Request Syntax<a name="API_DescribeCompilationJob_RequestSyntax"></a>

```
{
   "[CompilationJobName](#SageMaker-DescribeCompilationJob-request-CompilationJobName)": "string"
}
```

## Request Parameters<a name="API_DescribeCompilationJob_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [CompilationJobName](#API_DescribeCompilationJob_RequestSyntax) **   <a name="SageMaker-DescribeCompilationJob-request-CompilationJobName"></a>
The name of the model compilation job that you want information about\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*$`   
Required: Yes

## Response Syntax<a name="API_DescribeCompilationJob_ResponseSyntax"></a>

```
{
   "[CompilationEndTime](#SageMaker-DescribeCompilationJob-response-CompilationEndTime)": number,
   "[CompilationJobArn](#SageMaker-DescribeCompilationJob-response-CompilationJobArn)": "string",
   "[CompilationJobName](#SageMaker-DescribeCompilationJob-response-CompilationJobName)": "string",
   "[CompilationJobStatus](#SageMaker-DescribeCompilationJob-response-CompilationJobStatus)": "string",
   "[CompilationStartTime](#SageMaker-DescribeCompilationJob-response-CompilationStartTime)": number,
   "[CreationTime](#SageMaker-DescribeCompilationJob-response-CreationTime)": number,
   "[FailureReason](#SageMaker-DescribeCompilationJob-response-FailureReason)": "string",
   "[InputConfig](#SageMaker-DescribeCompilationJob-response-InputConfig)": { 
      "[DataInputConfig](API_InputConfig.md#SageMaker-Type-InputConfig-DataInputConfig)": "string",
      "[Framework](API_InputConfig.md#SageMaker-Type-InputConfig-Framework)": "string",
      "[S3Uri](API_InputConfig.md#SageMaker-Type-InputConfig-S3Uri)": "string"
   },
   "[LastModifiedTime](#SageMaker-DescribeCompilationJob-response-LastModifiedTime)": number,
   "[ModelArtifacts](#SageMaker-DescribeCompilationJob-response-ModelArtifacts)": { 
      "[S3ModelArtifacts](API_ModelArtifacts.md#SageMaker-Type-ModelArtifacts-S3ModelArtifacts)": "string"
   },
   "[OutputConfig](#SageMaker-DescribeCompilationJob-response-OutputConfig)": { 
      "[S3OutputLocation](API_OutputConfig.md#SageMaker-Type-OutputConfig-S3OutputLocation)": "string",
      "[TargetDevice](API_OutputConfig.md#SageMaker-Type-OutputConfig-TargetDevice)": "string"
   },
   "[RoleArn](#SageMaker-DescribeCompilationJob-response-RoleArn)": "string",
   "[StoppingCondition](#SageMaker-DescribeCompilationJob-response-StoppingCondition)": { 
      "[MaxRuntimeInSeconds](API_StoppingCondition.md#SageMaker-Type-StoppingCondition-MaxRuntimeInSeconds)": number
   }
}
```

## Response Elements<a name="API_DescribeCompilationJob_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [CompilationEndTime](#API_DescribeCompilationJob_ResponseSyntax) **   <a name="SageMaker-DescribeCompilationJob-response-CompilationEndTime"></a>
The time when the model compilation job on a compilation job instance ended\. For a successful or stopped job, this is when the job's model artifacts have finished uploading\. For a failed job, this is when Amazon SageMaker detected that the job failed\.   
Type: Timestamp

 ** [CompilationJobArn](#API_DescribeCompilationJob_ResponseSyntax) **   <a name="SageMaker-DescribeCompilationJob-response-CompilationJobArn"></a>
The Amazon Resource Name \(ARN\) of an IAM role that Amazon SageMaker assumes to perform the model compilation job\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:compilation-job/.*` 

 ** [CompilationJobName](#API_DescribeCompilationJob_ResponseSyntax) **   <a name="SageMaker-DescribeCompilationJob-response-CompilationJobName"></a>
The name of the model compilation job\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*$` 

 ** [CompilationJobStatus](#API_DescribeCompilationJob_ResponseSyntax) **   <a name="SageMaker-DescribeCompilationJob-response-CompilationJobStatus"></a>
The status of the model compilation job\.  
Type: String  
Valid Values:` INPROGRESS | COMPLETED | FAILED | STARTING | STOPPING | STOPPED` 

 ** [CompilationStartTime](#API_DescribeCompilationJob_ResponseSyntax) **   <a name="SageMaker-DescribeCompilationJob-response-CompilationStartTime"></a>
The time when the model compilation job started the `CompilationJob` instances\.   
You are billed for the time between this timestamp and the timestamp in the [DescribeCompilationJob:CompilationEndTime](#SageMaker-DescribeCompilationJob-response-CompilationEndTime) field\. In Amazon CloudWatch Logs, the start time might be later than this time\. That's because it takes time to download the compilation job, which depends on the size of the compilation job container\.   
Type: Timestamp

 ** [CreationTime](#API_DescribeCompilationJob_ResponseSyntax) **   <a name="SageMaker-DescribeCompilationJob-response-CreationTime"></a>
The time that the model compilation job was created\.  
Type: Timestamp

 ** [FailureReason](#API_DescribeCompilationJob_ResponseSyntax) **   <a name="SageMaker-DescribeCompilationJob-response-FailureReason"></a>
If a model compilation job failed, the reason it failed\.   
Type: String  
Length Constraints: Maximum length of 1024\.

 ** [InputConfig](#API_DescribeCompilationJob_ResponseSyntax) **   <a name="SageMaker-DescribeCompilationJob-response-InputConfig"></a>
Information about the location in Amazon S3 of the input model artifacts, the name and shape of the expected data inputs, and the framework in which the model was trained\.  
Type: [InputConfig](API_InputConfig.md) object

 ** [LastModifiedTime](#API_DescribeCompilationJob_ResponseSyntax) **   <a name="SageMaker-DescribeCompilationJob-response-LastModifiedTime"></a>
The time that the status of the model compilation job was last modified\.  
Type: Timestamp

 ** [ModelArtifacts](#API_DescribeCompilationJob_ResponseSyntax) **   <a name="SageMaker-DescribeCompilationJob-response-ModelArtifacts"></a>
Information about the location in Amazon S3 that has been configured for storing the model artifacts used in the compilation job\.  
Type: [ModelArtifacts](API_ModelArtifacts.md) object

 ** [OutputConfig](#API_DescribeCompilationJob_ResponseSyntax) **   <a name="SageMaker-DescribeCompilationJob-response-OutputConfig"></a>
Information about the output location for the compiled model and the target device that the model runs on\.  
Type: [OutputConfig](API_OutputConfig.md) object

 ** [RoleArn](#API_DescribeCompilationJob_ResponseSyntax) **   <a name="SageMaker-DescribeCompilationJob-response-RoleArn"></a>
The Amazon Resource Name \(ARN\) of the model compilation job\.  
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.  
Pattern: `^arn:aws[a-z\-]*:iam::\d{12}:role/?[a-zA-Z_0-9+=,.@\-_/]+$` 

 ** [StoppingCondition](#API_DescribeCompilationJob_ResponseSyntax) **   <a name="SageMaker-DescribeCompilationJob-response-StoppingCondition"></a>
The duration allowed for model compilation\.  
Type: [StoppingCondition](API_StoppingCondition.md) object

## Errors<a name="API_DescribeCompilationJob_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ResourceNotFound**   
Resource being access is not found\.  
HTTP Status Code: 400

## See Also<a name="API_DescribeCompilationJob_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/DescribeCompilationJob) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/DescribeCompilationJob) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/DescribeCompilationJob) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/DescribeCompilationJob) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/DescribeCompilationJob) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/DescribeCompilationJob) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/DescribeCompilationJob) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/DescribeCompilationJob) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/DescribeCompilationJob) 