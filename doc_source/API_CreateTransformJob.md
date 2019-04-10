# CreateTransformJob<a name="API_CreateTransformJob"></a>

Starts a transform job\. A transform job uses a trained model to get inferences on a dataset and saves these results to an Amazon S3 location that you specify\.

To perform batch transformations, you create a transform job and use the data that you have readily available\.

In the request body, you provide the following:
+  `TransformJobName` \- Identifies the transform job\. The name must be unique within an AWS Region in an AWS account\.
+  `ModelName` \- Identifies the model to use\. `ModelName` must be the name of an existing Amazon SageMaker model in the same AWS Region and AWS account\. For information on creating a model, see [CreateModel](API_CreateModel.md)\.
+  `TransformInput` \- Describes the dataset to be transformed and the Amazon S3 location where it is stored\.
+  `TransformOutput` \- Identifies the Amazon S3 location where you want Amazon SageMaker to save the results from the transform job\.
+  `TransformResources` \- Identifies the ML compute instances for the transform job\.

 For more information about how batch transformation works Amazon SageMaker, see [How It Works](https://docs.aws.amazon.com/sagemaker/latest/dg/batch-transform.html)\. 

## Request Syntax<a name="API_CreateTransformJob_RequestSyntax"></a>

```
{
   "[BatchStrategy](#SageMaker-CreateTransformJob-request-BatchStrategy)": "string",
   "[Environment](#SageMaker-CreateTransformJob-request-Environment)": { 
      "string" : "string" 
   },
   "[MaxConcurrentTransforms](#SageMaker-CreateTransformJob-request-MaxConcurrentTransforms)": number,
   "[MaxPayloadInMB](#SageMaker-CreateTransformJob-request-MaxPayloadInMB)": number,
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
         "[S3DataSource](API_TransformDataSource.md#SageMaker-Type-TransformDataSource-S3DataSource)": { 
            "[S3DataType](API_TransformS3DataSource.md#SageMaker-Type-TransformS3DataSource-S3DataType)": "string",
            "[S3Uri](API_TransformS3DataSource.md#SageMaker-Type-TransformS3DataSource-S3Uri)": "string"
         }
      },
      "[SplitType](API_TransformInput.md#SageMaker-Type-TransformInput-SplitType)": "string"
   },
   "[TransformJobName](#SageMaker-CreateTransformJob-request-TransformJobName)": "string",
   "[TransformOutput](#SageMaker-CreateTransformJob-request-TransformOutput)": { 
      "[Accept](API_TransformOutput.md#SageMaker-Type-TransformOutput-Accept)": "string",
      "[AssembleWith](API_TransformOutput.md#SageMaker-Type-TransformOutput-AssembleWith)": "string",
      "[KmsKeyId](API_TransformOutput.md#SageMaker-Type-TransformOutput-KmsKeyId)": "string",
      "[S3OutputPath](API_TransformOutput.md#SageMaker-Type-TransformOutput-S3OutputPath)": "string"
   },
   "[TransformResources](#SageMaker-CreateTransformJob-request-TransformResources)": { 
      "[InstanceCount](API_TransformResources.md#SageMaker-Type-TransformResources-InstanceCount)": number,
      "[InstanceType](API_TransformResources.md#SageMaker-Type-TransformResources-InstanceType)": "string",
      "[VolumeKmsKeyId](API_TransformResources.md#SageMaker-Type-TransformResources-VolumeKmsKeyId)": "string"
   }
}
```

## Request Parameters<a name="API_CreateTransformJob_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [BatchStrategy](#API_CreateTransformJob_RequestSyntax) **   <a name="SageMaker-CreateTransformJob-request-BatchStrategy"></a>
Specifies the number of records to include in a mini\-batch for an HTTP inference request\. A *record* ** is a single unit of input data that inference can be made on\. For example, a single line in a CSV file is a record\.   
To enable the batch strategy, you must set `SplitType` to `Line`, `RecordIO`, or `TFRecord`\.  
To use only one record when making an HTTP invocation request to a container, set `BatchStrategy` to `SingleRecord` and `SplitType` to `Line`\.  
To fit as many records in a mini\-batch as can fit within the `MaxPayloadInMB` limit, set `BatchStrategy` to `MultiRecord` and `SplitType` to `Line`\.  
Type: String  
Valid Values:` MultiRecord | SingleRecord`   
Required: No

 ** [Environment](#API_CreateTransformJob_RequestSyntax) **   <a name="SageMaker-CreateTransformJob-request-Environment"></a>
The environment variables to set in the Docker container\. We support up to 16 key and values entries in the map\.  
Type: String to string map  
Key Length Constraints: Maximum length of 1024\.  
Key Pattern: `[a-zA-Z_][a-zA-Z0-9_]*`   
Value Length Constraints: Maximum length of 10240\.  
Value Pattern: `[\S\s]*`   
Required: No

 ** [MaxConcurrentTransforms](#API_CreateTransformJob_RequestSyntax) **   <a name="SageMaker-CreateTransformJob-request-MaxConcurrentTransforms"></a>
The maximum number of parallel requests that can be sent to each instance in a transform job\. If `MaxConcurrentTransforms` is set to `0` or left unset, Amazon SageMaker checks the optional execution\-parameters to determine the optimal settings for your chosen algorithm\. If the execution\-parameters endpoint is not enabled, the default value is `1`\. For more information on execution\-parameters, see [How Containers Serve Requests](http://docs.aws.amazon.com/sagemaker/latest/dg/your-algorithms-batch-code.html#your-algorithms-batch-code-how-containe-serves-requests)\. For built\-in algorithms, you don't need to set a value for `MaxConcurrentTransforms`\.  
Type: Integer  
Valid Range: Minimum value of 0\.  
Required: No

 ** [MaxPayloadInMB](#API_CreateTransformJob_RequestSyntax) **   <a name="SageMaker-CreateTransformJob-request-MaxPayloadInMB"></a>
The maximum allowed size of the payload, in MB\. A *payload* is the data portion of a record \(without metadata\)\. The value in `MaxPayloadInMB` must be greater than, or equal to, the size of a single record\. To estimate the size of a record in MB, divide the size of your dataset by the number of records\. To ensure that the records fit within the maximum payload size, we recommend using a slightly larger value\. The default value is `6` MB\.   
For cases where the payload might be arbitrarily large and is transmitted using HTTP chunked encoding, set the value to `0`\. This feature works only in supported algorithms\. Currently, Amazon SageMaker built\-in algorithms do not support HTTP chunked encoding\.  
Type: Integer  
Valid Range: Minimum value of 0\.  
Required: No

 ** [ModelName](#API_CreateTransformJob_RequestSyntax) **   <a name="SageMaker-CreateTransformJob-request-ModelName"></a>
The name of the model that you want to use for the transform job\. `ModelName` must be the name of an existing Amazon SageMaker model within an AWS Region in an AWS account\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

 ** [Tags](#API_CreateTransformJob_RequestSyntax) **   <a name="SageMaker-CreateTransformJob-request-Tags"></a>
\(Optional\) An array of key\-value pairs\. For more information, see [Using Cost Allocation Tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html#allocation-what) in the *AWS Billing and Cost Management User Guide*\.  
Type: Array of [Tag](API_Tag.md) objects  
Array Members: Minimum number of 0 items\. Maximum number of 50 items\.  
Required: No

 ** [TransformInput](#API_CreateTransformJob_RequestSyntax) **   <a name="SageMaker-CreateTransformJob-request-TransformInput"></a>
Describes the input source and the way the transform job consumes it\.  
Type: [TransformInput](API_TransformInput.md) object  
Required: Yes

 ** [TransformJobName](#API_CreateTransformJob_RequestSyntax) **   <a name="SageMaker-CreateTransformJob-request-TransformJobName"></a>
The name of the transform job\. The name must be unique within an AWS Region in an AWS account\.   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

 ** [TransformOutput](#API_CreateTransformJob_RequestSyntax) **   <a name="SageMaker-CreateTransformJob-request-TransformOutput"></a>
Describes the results of the transform job\.  
Type: [TransformOutput](API_TransformOutput.md) object  
Required: Yes

 ** [TransformResources](#API_CreateTransformJob_RequestSyntax) **   <a name="SageMaker-CreateTransformJob-request-TransformResources"></a>
Describes the resources, including ML instance types and ML instance count, to use for the transform job\.  
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
The Amazon Resource Name \(ARN\) of the transform job\.  
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
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/CreateTransformJob) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/CreateTransformJob) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/CreateTransformJob) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/CreateTransformJob) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/CreateTransformJob) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/CreateTransformJob) 