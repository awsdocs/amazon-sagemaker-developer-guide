# StopCompilationJob<a name="API_StopCompilationJob"></a>

Stops a model compilation job\.

 To stop a job, Amazon SageMaker sends the algorithm the SIGTERM signal\. This gracefully shuts the job down\. If the job hasn't stopped, it sends the SIGKILL signal\.

When it receives a `StopCompilationJob` request, Amazon SageMaker changes the [CompilationJobSummary:CompilationJobStatus](API_CompilationJobSummary.md#SageMaker-Type-CompilationJobSummary-CompilationJobStatus) of the job to `Stopping`\. After Amazon SageMaker stops the job, it sets the [CompilationJobSummary:CompilationJobStatus](API_CompilationJobSummary.md#SageMaker-Type-CompilationJobSummary-CompilationJobStatus) to `Stopped`\. 

## Request Syntax<a name="API_StopCompilationJob_RequestSyntax"></a>

```
{
   "[CompilationJobName](#SageMaker-StopCompilationJob-request-CompilationJobName)": "string"
}
```

## Request Parameters<a name="API_StopCompilationJob_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [CompilationJobName](#API_StopCompilationJob_RequestSyntax) **   <a name="SageMaker-StopCompilationJob-request-CompilationJobName"></a>
The name of the model compilation job to stop\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*$`   
Required: Yes

## Response Elements<a name="API_StopCompilationJob_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response with an empty HTTP body\.

## Errors<a name="API_StopCompilationJob_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ResourceNotFound**   
Resource being access is not found\.  
HTTP Status Code: 400

## See Also<a name="API_StopCompilationJob_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/StopCompilationJob) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/StopCompilationJob) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/StopCompilationJob) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/StopCompilationJob) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/StopCompilationJob) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/StopCompilationJob) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/StopCompilationJob) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/StopCompilationJob) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/StopCompilationJob) 