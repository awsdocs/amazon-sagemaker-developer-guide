# CreateCompilationJob<a name="API_CreateCompilationJob"></a>

Starts a model compilation job\. After the model has been compiled, Amazon SageMaker saves the resulting model artifacts to an Amazon Simple Storage Service \(Amazon S3\) bucket that you specify\. 

If you choose to host your model using Amazon SageMaker hosting services, you can use the resulting model artifacts as part of the model\. You can also use the artifacts with AWS IoT Greengrass\. In that case, deploy them as an ML resource\.

In the request body, you provide the following:
+ A name for the compilation job
+  Information about the input model artifacts 
+ The output location for the compiled model and the device \(target\) that the model runs on 
+  `The Amazon Resource Name (ARN) of the IAM role that Amazon SageMaker assumes to perform the model compilation job` 

You can also provide a `Tag` to track the model compilation job's resource use and costs\. The response body contains the `CompilationJobArn` for the compiled job\.

To stop a model compilation job, use [StopCompilationJob](API_StopCompilationJob.md)\. To get information about a particular model compilation job, use [DescribeCompilationJob](API_DescribeCompilationJob.md)\. To get information about multiple model compilation jobs, use [ListCompilationJobs](API_ListCompilationJobs.md)\.

## Request Syntax<a name="API_CreateCompilationJob_RequestSyntax"></a>

```
{
   "[CompilationJobName](#SageMaker-CreateCompilationJob-request-CompilationJobName)": "string",
   "[InputConfig](#SageMaker-CreateCompilationJob-request-InputConfig)": { 
      "[DataInputConfig](API_InputConfig.md#SageMaker-Type-InputConfig-DataInputConfig)": "string",
      "[Framework](API_InputConfig.md#SageMaker-Type-InputConfig-Framework)": "string",
      "[S3Uri](API_InputConfig.md#SageMaker-Type-InputConfig-S3Uri)": "string"
   },
   "[OutputConfig](#SageMaker-CreateCompilationJob-request-OutputConfig)": { 
      "[S3OutputLocation](API_OutputConfig.md#SageMaker-Type-OutputConfig-S3OutputLocation)": "string",
      "[TargetDevice](API_OutputConfig.md#SageMaker-Type-OutputConfig-TargetDevice)": "string"
   },
   "[RoleArn](#SageMaker-CreateCompilationJob-request-RoleArn)": "string",
   "[StoppingCondition](#SageMaker-CreateCompilationJob-request-StoppingCondition)": { 
      "[MaxRuntimeInSeconds](API_StoppingCondition.md#SageMaker-Type-StoppingCondition-MaxRuntimeInSeconds)": number
   }
}
```

## Request Parameters<a name="API_CreateCompilationJob_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [CompilationJobName](#API_CreateCompilationJob_RequestSyntax) **   <a name="SageMaker-CreateCompilationJob-request-CompilationJobName"></a>
A name for the model compilation job\. The name must be unique within the AWS Region and within your AWS account\.   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*$`   
Required: Yes

 ** [InputConfig](#API_CreateCompilationJob_RequestSyntax) **   <a name="SageMaker-CreateCompilationJob-request-InputConfig"></a>
Provides information about the location of input model artifacts, the name and shape of the expected data inputs, and the framework in which the model was trained\.  
Type: [InputConfig](API_InputConfig.md) object  
Required: Yes

 ** [OutputConfig](#API_CreateCompilationJob_RequestSyntax) **   <a name="SageMaker-CreateCompilationJob-request-OutputConfig"></a>
Provides information about the output location for the compiled model and the target device the model runs on\.  
Type: [OutputConfig](API_OutputConfig.md) object  
Required: Yes

 ** [RoleArn](#API_CreateCompilationJob_RequestSyntax) **   <a name="SageMaker-CreateCompilationJob-request-RoleArn"></a>
The Amazon Resource Name \(ARN\) of an IAM role that enables Amazon SageMaker to perform tasks on your behalf\.   
During model compilation, Amazon SageMaker needs your permission to:  
+ Read input data from an S3 bucket
+ Write model artifacts to an S3 bucket
+ Write logs to Amazon CloudWatch Logs
+ Publish metrics to Amazon CloudWatch
You grant permissions for all of these tasks to an IAM role\. To pass this role to Amazon SageMaker, the caller of this API must have the `iam:PassRole` permission\. For more information, see [Amazon SageMaker Roles\.](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html)   
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.  
Pattern: `^arn:aws[a-z\-]*:iam::\d{12}:role/?[a-zA-Z_0-9+=,.@\-_/]+$`   
Required: Yes

 ** [StoppingCondition](#API_CreateCompilationJob_RequestSyntax) **   <a name="SageMaker-CreateCompilationJob-request-StoppingCondition"></a>
The duration allowed for model compilation\.  
Type: [StoppingCondition](API_StoppingCondition.md) object  
Required: Yes

## Response Syntax<a name="API_CreateCompilationJob_ResponseSyntax"></a>

```
{
   "[CompilationJobArn](#SageMaker-CreateCompilationJob-response-CompilationJobArn)": "string"
}
```

## Response Elements<a name="API_CreateCompilationJob_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [CompilationJobArn](#API_CreateCompilationJob_ResponseSyntax) **   <a name="SageMaker-CreateCompilationJob-response-CompilationJobArn"></a>
If the action is successful, the service sends back an HTTP 200 response\. Amazon SageMaker returns the following data in JSON format:  
+  `CompilationJobArn`: The Amazon Resource Name \(ARN\) of the compiled job\.
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:compilation-job/.*` 

## Errors<a name="API_CreateCompilationJob_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ResourceInUse**   
Resource being accessed is in use\.  
HTTP Status Code: 400

 **ResourceLimitExceeded**   
 You have exceeded an Amazon SageMaker resource limit\. For example, you might have too many training jobs created\.   
HTTP Status Code: 400

## See Also<a name="API_CreateCompilationJob_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/CreateCompilationJob) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/CreateCompilationJob) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/CreateCompilationJob) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/CreateCompilationJob) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/CreateCompilationJob) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/CreateCompilationJob) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/CreateCompilationJob) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/CreateCompilationJob) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/CreateCompilationJob) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/CreateCompilationJob) 