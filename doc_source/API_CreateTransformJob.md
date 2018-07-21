# CreateTransformJob<a name="API_CreateTransformJob"></a>

## Request Syntax<a name="API_CreateTransformJob_RequestSyntax"></a>

```
{
   "[MaxConcurrentTransforms](#SageMaker-CreateTransformJob-request-MaxConcurrentTransforms)": number,
   "[MaxPayloadInMB](#SageMaker-CreateTransformJob-request-MaxPayloadInMB)": number,
   "[MaxRecordsPerBatch](#SageMaker-CreateTransformJob-request-MaxRecordsPerBatch)": number,
   "[ModelName](#SageMaker-CreateTransformJob-request-ModelName)": "string",
   "[Tags](#SageMaker-CreateTransformJob-request-Tags)": [ 
      { 
         "[Key](API_Tag.md#SageMaker-Type-Tag-Key)": "string",
         "[Value](API_Tag.md#SageMaker-Type-Tag-Value)": "string"
      }
   ],
   "[TransformInput](#SageMaker-CreateTransformJob-request-TransformInput)": { 
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
   "[TransformJobName](#SageMaker-CreateTransformJob-request-TransformJobName)": "string",
   "[TransformOutput](#SageMaker-CreateTransformJob-request-TransformOutput)": { 
      "[AssembleWith](API_TransformOutput.md#SageMaker-Type-TransformOutput-AssembleWith)": "string",
      "[CompressionType](API_TransformOutput.md#SageMaker-Type-TransformOutput-CompressionType)": "string",
      "[KmsKeyId](API_TransformOutput.md#SageMaker-Type-TransformOutput-KmsKeyId)": "string",
      "[S3OutputPath](API_TransformOutput.md#SageMaker-Type-TransformOutput-S3OutputPath)": "string"
   },
   "[TransformResources](#SageMaker-CreateTransformJob-request-TransformResources)": { 
      "[InstanceCount](API_TransformResources.md#SageMaker-Type-TransformResources-InstanceCount)": number,
      "[InstanceType](API_TransformResources.md#SageMaker-Type-TransformResources-InstanceType)": "string"
   }
}
```

## Request Parameters<a name="API_CreateTransformJob_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [MaxConcurrentTransforms](#API_CreateTransformJob_RequestSyntax) **   <a name="SageMaker-CreateTransformJob-request-MaxConcurrentTransforms"></a>
Type: Integer  
Valid Range: Minimum value of 0\.  
Required: No

 ** [MaxPayloadInMB](#API_CreateTransformJob_RequestSyntax) **   <a name="SageMaker-CreateTransformJob-request-MaxPayloadInMB"></a>
Type: Integer  
Valid Range: Minimum value of 0\.  
Required: No

 ** [MaxRecordsPerBatch](#API_CreateTransformJob_RequestSyntax) **   <a name="SageMaker-CreateTransformJob-request-MaxRecordsPerBatch"></a>
Type: Integer  
Valid Range: Minimum value of 0\.  
Required: No

 ** [ModelName](#API_CreateTransformJob_RequestSyntax) **   <a name="SageMaker-CreateTransformJob-request-ModelName"></a>
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

 ** [Tags](#API_CreateTransformJob_RequestSyntax) **   <a name="SageMaker-CreateTransformJob-request-Tags"></a>
Type: Array of [Tag](API_Tag.md) objects  
Array Members: Minimum number of 0 items\. Maximum number of 50 items\.  
Required: No

 ** [TransformInput](#API_CreateTransformJob_RequestSyntax) **   <a name="SageMaker-CreateTransformJob-request-TransformInput"></a>
Type: [TransformInput](API_TransformInput.md) object  
Required: Yes

 ** [TransformJobName](#API_CreateTransformJob_RequestSyntax) **   <a name="SageMaker-CreateTransformJob-request-TransformJobName"></a>
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

 ** [TransformOutput](#API_CreateTransformJob_RequestSyntax) **   <a name="SageMaker-CreateTransformJob-request-TransformOutput"></a>
Type: [TransformOutput](API_TransformOutput.md) object  
Required: Yes

 ** [TransformResources](#API_CreateTransformJob_RequestSyntax) **   <a name="SageMaker-CreateTransformJob-request-TransformResources"></a>
Type: [TransformResources](API_TransformResources.md) object  
Required: Yes

## Response Syntax<a name="API_CreateTransformJob_ResponseSyntax"></a>

```
{
   "[TransformJobArn](#SageMaker-CreateTransformJob-response-TransformJobArn)": "string"
}
```

## Response Elements<a name="API_CreateTransformJob_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [TransformJobArn](#API_CreateTransformJob_ResponseSyntax) **   <a name="SageMaker-CreateTransformJob-response-TransformJobArn"></a>
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:transform-job/.*` 

## Errors<a name="API_CreateTransformJob_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ResourceInUse**   
Resource being accessed is in use\.  
HTTP Status Code: 400

 **ResourceLimitExceeded**   
 You have exceeded an Amazon SageMaker resource limit\. For example, you might have too many training jobs created\.   
HTTP Status Code: 400

## See Also<a name="API_CreateTransformJob_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/CreateTransformJob) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/CreateTransformJob) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/CreateTransformJob) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/CreateTransformJob) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/CreateTransformJob) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/CreateTransformJob) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/CreateTransformJob) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/CreateTransformJob) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/CreateTransformJob) 