# DescribeTransformJob<a name="API_DescribeTransformJob"></a>

Returns information about a transform job\.

## Request Syntax<a name="API_DescribeTransformJob_RequestSyntax"></a>

```
{
   "[TransformJobName](#SageMaker-DescribeTransformJob-request-TransformJobName)": "string"
}
```

## Request Parameters<a name="API_DescribeTransformJob_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [TransformJobName](#API_DescribeTransformJob_RequestSyntax) **   <a name="SageMaker-DescribeTransformJob-request-TransformJobName"></a>
The name of the transform job that you want to view details of\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

## Response Syntax<a name="API_DescribeTransformJob_ResponseSyntax"></a>

```
{
   "[BatchStrategy](#SageMaker-DescribeTransformJob-response-BatchStrategy)": "string",
   "[CreationTime](#SageMaker-DescribeTransformJob-response-CreationTime)": number,
   "[Environment](#SageMaker-DescribeTransformJob-response-Environment)": { 
      "string" : "string" 
   },
   "[FailureReason](#SageMaker-DescribeTransformJob-response-FailureReason)": "string",
   "[LabelingJobArn](#SageMaker-DescribeTransformJob-response-LabelingJobArn)": "string",
   "[MaxConcurrentTransforms](#SageMaker-DescribeTransformJob-response-MaxConcurrentTransforms)": number,
   "[MaxPayloadInMB](#SageMaker-DescribeTransformJob-response-MaxPayloadInMB)": number,
   "[ModelName](#SageMaker-DescribeTransformJob-response-ModelName)": "string",
   "[TransformEndTime](#SageMaker-DescribeTransformJob-response-TransformEndTime)": number,
   "[TransformInput](#SageMaker-DescribeTransformJob-response-TransformInput)": { 
      "[CompressionType](API_TransformInput.md#SageMaker-Type-TransformInput-CompressionType)": "string",
      "[ContentType](API_TransformInput.md#SageMaker-Type-TransformInput-ContentType)": "string",
      "[DataSource](API_TransformInput.md#SageMaker-Type-TransformInput-DataSource)": { 
         "[S3DataSource](API_TransformDataSource.md#SageMaker-Type-TransformDataSource-S3DataSource)": { 
            "[S3DataType](API_TransformS3DataSource.md#SageMaker-Type-TransformS3DataSource-S3DataType)": "string",
            "[S3Uri](API_TransformS3DataSource.md#SageMaker-Type-TransformS3DataSource-S3Uri)": "string"
         }
      },
      "[SplitType](API_TransformInput.md#SageMaker-Type-TransformInput-SplitType)": "string"
   },
   "[TransformJobArn](#SageMaker-DescribeTransformJob-response-TransformJobArn)": "string",
   "[TransformJobName](#SageMaker-DescribeTransformJob-response-TransformJobName)": "string",
   "[TransformJobStatus](#SageMaker-DescribeTransformJob-response-TransformJobStatus)": "string",
   "[TransformOutput](#SageMaker-DescribeTransformJob-response-TransformOutput)": { 
      "[Accept](API_TransformOutput.md#SageMaker-Type-TransformOutput-Accept)": "string",
      "[AssembleWith](API_TransformOutput.md#SageMaker-Type-TransformOutput-AssembleWith)": "string",
      "[KmsKeyId](API_TransformOutput.md#SageMaker-Type-TransformOutput-KmsKeyId)": "string",
      "[S3OutputPath](API_TransformOutput.md#SageMaker-Type-TransformOutput-S3OutputPath)": "string"
   },
   "[TransformResources](#SageMaker-DescribeTransformJob-response-TransformResources)": { 
      "[InstanceCount](API_TransformResources.md#SageMaker-Type-TransformResources-InstanceCount)": number,
      "[InstanceType](API_TransformResources.md#SageMaker-Type-TransformResources-InstanceType)": "string",
      "[VolumeKmsKeyId](API_TransformResources.md#SageMaker-Type-TransformResources-VolumeKmsKeyId)": "string"
   },
   "[TransformStartTime](#SageMaker-DescribeTransformJob-response-TransformStartTime)": number
}
```

## Response Elements<a name="API_DescribeTransformJob_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [BatchStrategy](#API_DescribeTransformJob_ResponseSyntax) **   <a name="SageMaker-DescribeTransformJob-response-BatchStrategy"></a>
Specifies the number of records to include in a mini\-batch for an HTTP inference request\. A *record* ** is a single unit of input data that inference can be made on\. For example, a single line in a CSV file is a record\.   
To enable the batch strategy, you must set `SplitType` to `Line`, `RecordIO`, or `TFRecord`\.  
Type: String  
Valid Values:` MultiRecord | SingleRecord` 

 ** [CreationTime](#API_DescribeTransformJob_ResponseSyntax) **   <a name="SageMaker-DescribeTransformJob-response-CreationTime"></a>
A timestamp that shows when the transform Job was created\.  
Type: Timestamp

 ** [Environment](#API_DescribeTransformJob_ResponseSyntax) **   <a name="SageMaker-DescribeTransformJob-response-Environment"></a>
The environment variables to set in the Docker container\. We support up to 16 key and values entries in the map\.  
Type: String to string map  
Key Length Constraints: Maximum length of 1024\.  
Key Pattern: `[a-zA-Z_][a-zA-Z0-9_]*`   
Value Length Constraints: Maximum length of 10240\.  
Value Pattern: `[\S\s]*` 

 ** [FailureReason](#API_DescribeTransformJob_ResponseSyntax) **   <a name="SageMaker-DescribeTransformJob-response-FailureReason"></a>
If the transform job failed, `FailureReason` describes why it failed\. A transform job creates a log file, which includes error messages, and stores it as an Amazon S3 object\. For more information, see [Log Amazon SageMaker Events with Amazon CloudWatch](http://docs.aws.amazon.com/sagemaker/latest/dg/logging-cloudwatch.html)\.  
Type: String  
Length Constraints: Maximum length of 1024\.

 ** [LabelingJobArn](#API_DescribeTransformJob_ResponseSyntax) **   <a name="SageMaker-DescribeTransformJob-response-LabelingJobArn"></a>
The Amazon Resource Name \(ARN\) of the Amazon SageMaker Ground Truth labeling job that created the transform or training job\.  
Type: String  
Length Constraints: Maximum length of 2048\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:labeling-job/.*` 

 ** [MaxConcurrentTransforms](#API_DescribeTransformJob_ResponseSyntax) **   <a name="SageMaker-DescribeTransformJob-response-MaxConcurrentTransforms"></a>
The maximum number of parallel requests on each instance node that can be launched in a transform job\. The default value is 1\.  
Type: Integer  
Valid Range: Minimum value of 0\.

 ** [MaxPayloadInMB](#API_DescribeTransformJob_ResponseSyntax) **   <a name="SageMaker-DescribeTransformJob-response-MaxPayloadInMB"></a>
The maximum payload size, in MB, used in the transform job\.  
Type: Integer  
Valid Range: Minimum value of 0\.

 ** [ModelName](#API_DescribeTransformJob_ResponseSyntax) **   <a name="SageMaker-DescribeTransformJob-response-ModelName"></a>
The name of the model used in the transform job\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*` 

 ** [TransformEndTime](#API_DescribeTransformJob_ResponseSyntax) **   <a name="SageMaker-DescribeTransformJob-response-TransformEndTime"></a>
Indicates when the transform job has been completed, or has stopped or failed\. You are billed for the time interval between this time and the value of `TransformStartTime`\.  
Type: Timestamp

 ** [TransformInput](#API_DescribeTransformJob_ResponseSyntax) **   <a name="SageMaker-DescribeTransformJob-response-TransformInput"></a>
Describes the dataset to be transformed and the Amazon S3 location where it is stored\.  
Type: [TransformInput](API_TransformInput.md) object

 ** [TransformJobArn](#API_DescribeTransformJob_ResponseSyntax) **   <a name="SageMaker-DescribeTransformJob-response-TransformJobArn"></a>
The Amazon Resource Name \(ARN\) of the transform job\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:transform-job/.*` 

 ** [TransformJobName](#API_DescribeTransformJob_ResponseSyntax) **   <a name="SageMaker-DescribeTransformJob-response-TransformJobName"></a>
The name of the transform job\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*` 

 ** [TransformJobStatus](#API_DescribeTransformJob_ResponseSyntax) **   <a name="SageMaker-DescribeTransformJob-response-TransformJobStatus"></a>
The status of the transform job\. If the transform job failed, the reason is returned in the `FailureReason` field\.  
Type: String  
Valid Values:` InProgress | Completed | Failed | Stopping | Stopped` 

 ** [TransformOutput](#API_DescribeTransformJob_ResponseSyntax) **   <a name="SageMaker-DescribeTransformJob-response-TransformOutput"></a>
Identifies the Amazon S3 location where you want Amazon SageMaker to save the results from the transform job\.  
Type: [TransformOutput](API_TransformOutput.md) object

 ** [TransformResources](#API_DescribeTransformJob_ResponseSyntax) **   <a name="SageMaker-DescribeTransformJob-response-TransformResources"></a>
Describes the resources, including ML instance types and ML instance count, to use for the transform job\.  
Type: [TransformResources](API_TransformResources.md) object

 ** [TransformStartTime](#API_DescribeTransformJob_ResponseSyntax) **   <a name="SageMaker-DescribeTransformJob-response-TransformStartTime"></a>
Indicates when the transform job starts on ML instances\. You are billed for the time interval between this time and the value of `TransformEndTime`\.  
Type: Timestamp

## Errors<a name="API_DescribeTransformJob_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ResourceNotFound**   
Resource being access is not found\.  
HTTP Status Code: 400

## See Also<a name="API_DescribeTransformJob_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/DescribeTransformJob) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/DescribeTransformJob) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/DescribeTransformJob) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/DescribeTransformJob) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/DescribeTransformJob) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/DescribeTransformJob) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/DescribeTransformJob) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/DescribeTransformJob) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/DescribeTransformJob) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/DescribeTransformJob) 