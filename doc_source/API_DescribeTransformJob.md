# DescribeTransformJob<a name="API_DescribeTransformJob"></a>

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
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

## Response Syntax<a name="API_DescribeTransformJob_ResponseSyntax"></a>

```
{
   "[CreationTime](#SageMaker-DescribeTransformJob-response-CreationTime)": number,
   "[FailureReason](#SageMaker-DescribeTransformJob-response-FailureReason)": "string",
   "[MaxConcurrentTransforms](#SageMaker-DescribeTransformJob-response-MaxConcurrentTransforms)": number,
   "[MaxPayloadInMB](#SageMaker-DescribeTransformJob-response-MaxPayloadInMB)": number,
   "[MaxRecordsPerBatch](#SageMaker-DescribeTransformJob-response-MaxRecordsPerBatch)": number,
   "[ModelName](#SageMaker-DescribeTransformJob-response-ModelName)": "string",
   "[ObjectProgress](#SageMaker-DescribeTransformJob-response-ObjectProgress)": { 
      "[CompletedObjects](API_ObjectProgress.md#SageMaker-Type-ObjectProgress-CompletedObjects)": number,
      "[ErroredObjects](API_ObjectProgress.md#SageMaker-Type-ObjectProgress-ErroredObjects)": number,
      "[TotalInputObjects](API_ObjectProgress.md#SageMaker-Type-ObjectProgress-TotalInputObjects)": number
   },
   "[TransformEndTime](#SageMaker-DescribeTransformJob-response-TransformEndTime)": number,
   "[TransformInput](#SageMaker-DescribeTransformJob-response-TransformInput)": { 
      "[CompressionType](API_TransformInput.md#SageMaker-Type-TransformInput-CompressionType)": "string",
      "[ContentType](API_TransformInput.md#SageMaker-Type-TransformInput-ContentType)": "string",
      "[DataSource](API_TransformInput.md#SageMaker-Type-TransformInput-DataSource)": { 
         "[S3DataSource](API_DataSource.md#SageMaker-Type-DataSource-S3DataSource)": { 
            "[S3DataDistributionType](API_S3DataSource.md#SageMaker-Type-S3DataSource-S3DataDistributionType)": "string",
            "[S3DataType](API_S3DataSource.md#SageMaker-Type-S3DataSource-S3DataType)": "string",
            "[S3Uri](API_S3DataSource.md#SageMaker-Type-S3DataSource-S3Uri)": "string"
         }
      },
      "[SplitType](API_TransformInput.md#SageMaker-Type-TransformInput-SplitType)": "string"
   },
   "[TransformJobArn](#SageMaker-DescribeTransformJob-response-TransformJobArn)": "string",
   "[TransformJobName](#SageMaker-DescribeTransformJob-response-TransformJobName)": "string",
   "[TransformJobProgress](#SageMaker-DescribeTransformJob-response-TransformJobProgress)": number,
   "[TransformJobStatus](#SageMaker-DescribeTransformJob-response-TransformJobStatus)": "string",
   "[TransformOutput](#SageMaker-DescribeTransformJob-response-TransformOutput)": { 
      "[AssembleWith](API_TransformOutput.md#SageMaker-Type-TransformOutput-AssembleWith)": "string",
      "[CompressionType](API_TransformOutput.md#SageMaker-Type-TransformOutput-CompressionType)": "string",
      "[KmsKeyId](API_TransformOutput.md#SageMaker-Type-TransformOutput-KmsKeyId)": "string",
      "[S3OutputPath](API_TransformOutput.md#SageMaker-Type-TransformOutput-S3OutputPath)": "string"
   },
   "[TransformResources](#SageMaker-DescribeTransformJob-response-TransformResources)": { 
      "[InstanceCount](API_TransformResources.md#SageMaker-Type-TransformResources-InstanceCount)": number,
      "[InstanceType](API_TransformResources.md#SageMaker-Type-TransformResources-InstanceType)": "string"
   },
   "[TransformStartTime](#SageMaker-DescribeTransformJob-response-TransformStartTime)": number
}
```

## Response Elements<a name="API_DescribeTransformJob_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [CreationTime](#API_DescribeTransformJob_ResponseSyntax) **   <a name="SageMaker-DescribeTransformJob-response-CreationTime"></a>
Type: Timestamp

 ** [FailureReason](#API_DescribeTransformJob_ResponseSyntax) **   <a name="SageMaker-DescribeTransformJob-response-FailureReason"></a>
Type: String  
Length Constraints: Maximum length of 1024\.

 ** [MaxConcurrentTransforms](#API_DescribeTransformJob_ResponseSyntax) **   <a name="SageMaker-DescribeTransformJob-response-MaxConcurrentTransforms"></a>
Type: Integer  
Valid Range: Minimum value of 0\.

 ** [MaxPayloadInMB](#API_DescribeTransformJob_ResponseSyntax) **   <a name="SageMaker-DescribeTransformJob-response-MaxPayloadInMB"></a>
Type: Integer  
Valid Range: Minimum value of 0\.

 ** [MaxRecordsPerBatch](#API_DescribeTransformJob_ResponseSyntax) **   <a name="SageMaker-DescribeTransformJob-response-MaxRecordsPerBatch"></a>
Type: Integer  
Valid Range: Minimum value of 0\.

 ** [ModelName](#API_DescribeTransformJob_ResponseSyntax) **   <a name="SageMaker-DescribeTransformJob-response-ModelName"></a>
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*` 

 ** [ObjectProgress](#API_DescribeTransformJob_ResponseSyntax) **   <a name="SageMaker-DescribeTransformJob-response-ObjectProgress"></a>
Type: [ObjectProgress](API_ObjectProgress.md) object

 ** [TransformEndTime](#API_DescribeTransformJob_ResponseSyntax) **   <a name="SageMaker-DescribeTransformJob-response-TransformEndTime"></a>
Type: Timestamp

 ** [TransformInput](#API_DescribeTransformJob_ResponseSyntax) **   <a name="SageMaker-DescribeTransformJob-response-TransformInput"></a>
Type: [TransformInput](API_TransformInput.md) object

 ** [TransformJobArn](#API_DescribeTransformJob_ResponseSyntax) **   <a name="SageMaker-DescribeTransformJob-response-TransformJobArn"></a>
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:transform-job/.*` 

 ** [TransformJobName](#API_DescribeTransformJob_ResponseSyntax) **   <a name="SageMaker-DescribeTransformJob-response-TransformJobName"></a>
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*` 

 ** [TransformJobProgress](#API_DescribeTransformJob_ResponseSyntax) **   <a name="SageMaker-DescribeTransformJob-response-TransformJobProgress"></a>
Type: Integer

 ** [TransformJobStatus](#API_DescribeTransformJob_ResponseSyntax) **   <a name="SageMaker-DescribeTransformJob-response-TransformJobStatus"></a>
Type: String  
Valid Values:` InProgress | Completed | Failed | Stopping | Stopped` 

 ** [TransformOutput](#API_DescribeTransformJob_ResponseSyntax) **   <a name="SageMaker-DescribeTransformJob-response-TransformOutput"></a>
Type: [TransformOutput](API_TransformOutput.md) object

 ** [TransformResources](#API_DescribeTransformJob_ResponseSyntax) **   <a name="SageMaker-DescribeTransformJob-response-TransformResources"></a>
Type: [TransformResources](API_TransformResources.md) object

 ** [TransformStartTime](#API_DescribeTransformJob_ResponseSyntax) **   <a name="SageMaker-DescribeTransformJob-response-TransformStartTime"></a>
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
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/DescribeTransformJob) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/DescribeTransformJob) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/DescribeTransformJob) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/DescribeTransformJob) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/DescribeTransformJob) 