# DescribeLabelingJob<a name="API_DescribeLabelingJob"></a>

Gets information about a labeling job\.

## Request Syntax<a name="API_DescribeLabelingJob_RequestSyntax"></a>

```
{
   "[LabelingJobName](#SageMaker-DescribeLabelingJob-request-LabelingJobName)": "string"
}
```

## Request Parameters<a name="API_DescribeLabelingJob_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [LabelingJobName](#API_DescribeLabelingJob_RequestSyntax) **   <a name="SageMaker-DescribeLabelingJob-request-LabelingJobName"></a>
The name of the labeling job to return information for\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

## Response Syntax<a name="API_DescribeLabelingJob_ResponseSyntax"></a>

```
{
   "[CreationTime](#SageMaker-DescribeLabelingJob-response-CreationTime)": number,
   "[FailureReason](#SageMaker-DescribeLabelingJob-response-FailureReason)": "string",
   "[HumanTaskConfig](#SageMaker-DescribeLabelingJob-response-HumanTaskConfig)": { 
      "[AnnotationConsolidationConfig](API_HumanTaskConfig.md#SageMaker-Type-HumanTaskConfig-AnnotationConsolidationConfig)": { 
         "[AnnotationConsolidationLambdaArn](API_AnnotationConsolidationConfig.md#SageMaker-Type-AnnotationConsolidationConfig-AnnotationConsolidationLambdaArn)": "string"
      },
      "[MaxConcurrentTaskCount](API_HumanTaskConfig.md#SageMaker-Type-HumanTaskConfig-MaxConcurrentTaskCount)": number,
      "[NumberOfHumanWorkersPerDataObject](API_HumanTaskConfig.md#SageMaker-Type-HumanTaskConfig-NumberOfHumanWorkersPerDataObject)": number,
      "[PreHumanTaskLambdaArn](API_HumanTaskConfig.md#SageMaker-Type-HumanTaskConfig-PreHumanTaskLambdaArn)": "string",
      "[PublicWorkforceTaskPrice](API_HumanTaskConfig.md#SageMaker-Type-HumanTaskConfig-PublicWorkforceTaskPrice)": { 
         "[AmountInUsd](API_PublicWorkforceTaskPrice.md#SageMaker-Type-PublicWorkforceTaskPrice-AmountInUsd)": { 
            "[Cents](API_USD.md#SageMaker-Type-USD-Cents)": number,
            "[Dollars](API_USD.md#SageMaker-Type-USD-Dollars)": number,
            "[TenthFractionsOfACent](API_USD.md#SageMaker-Type-USD-TenthFractionsOfACent)": number
         }
      },
      "[TaskAvailabilityLifetimeInSeconds](API_HumanTaskConfig.md#SageMaker-Type-HumanTaskConfig-TaskAvailabilityLifetimeInSeconds)": number,
      "[TaskDescription](API_HumanTaskConfig.md#SageMaker-Type-HumanTaskConfig-TaskDescription)": "string",
      "[TaskKeywords](API_HumanTaskConfig.md#SageMaker-Type-HumanTaskConfig-TaskKeywords)": [ "string" ],
      "[TaskTimeLimitInSeconds](API_HumanTaskConfig.md#SageMaker-Type-HumanTaskConfig-TaskTimeLimitInSeconds)": number,
      "[TaskTitle](API_HumanTaskConfig.md#SageMaker-Type-HumanTaskConfig-TaskTitle)": "string",
      "[UiConfig](API_HumanTaskConfig.md#SageMaker-Type-HumanTaskConfig-UiConfig)": { 
         "[UiTemplateS3Uri](API_UiConfig.md#SageMaker-Type-UiConfig-UiTemplateS3Uri)": "string"
      },
      "[WorkteamArn](API_HumanTaskConfig.md#SageMaker-Type-HumanTaskConfig-WorkteamArn)": "string"
   },
   "[InputConfig](#SageMaker-DescribeLabelingJob-response-InputConfig)": { 
      "[DataAttributes](API_LabelingJobInputConfig.md#SageMaker-Type-LabelingJobInputConfig-DataAttributes)": { 
         "[ContentClassifiers](API_LabelingJobDataAttributes.md#SageMaker-Type-LabelingJobDataAttributes-ContentClassifiers)": [ "string" ]
      },
      "[DataSource](API_LabelingJobInputConfig.md#SageMaker-Type-LabelingJobInputConfig-DataSource)": { 
         "[S3DataSource](API_LabelingJobDataSource.md#SageMaker-Type-LabelingJobDataSource-S3DataSource)": { 
            "[ManifestS3Uri](API_LabelingJobS3DataSource.md#SageMaker-Type-LabelingJobS3DataSource-ManifestS3Uri)": "string"
         }
      }
   },
   "[JobReferenceCode](#SageMaker-DescribeLabelingJob-response-JobReferenceCode)": "string",
   "[LabelAttributeName](#SageMaker-DescribeLabelingJob-response-LabelAttributeName)": "string",
   "[LabelCategoryConfigS3Uri](#SageMaker-DescribeLabelingJob-response-LabelCategoryConfigS3Uri)": "string",
   "[LabelCounters](#SageMaker-DescribeLabelingJob-response-LabelCounters)": { 
      "[FailedNonRetryableError](API_LabelCounters.md#SageMaker-Type-LabelCounters-FailedNonRetryableError)": number,
      "[HumanLabeled](API_LabelCounters.md#SageMaker-Type-LabelCounters-HumanLabeled)": number,
      "[MachineLabeled](API_LabelCounters.md#SageMaker-Type-LabelCounters-MachineLabeled)": number,
      "[TotalLabeled](API_LabelCounters.md#SageMaker-Type-LabelCounters-TotalLabeled)": number,
      "[Unlabeled](API_LabelCounters.md#SageMaker-Type-LabelCounters-Unlabeled)": number
   },
   "[LabelingJobAlgorithmsConfig](#SageMaker-DescribeLabelingJob-response-LabelingJobAlgorithmsConfig)": { 
      "[InitialActiveLearningModelArn](API_LabelingJobAlgorithmsConfig.md#SageMaker-Type-LabelingJobAlgorithmsConfig-InitialActiveLearningModelArn)": "string",
      "[LabelingJobAlgorithmSpecificationArn](API_LabelingJobAlgorithmsConfig.md#SageMaker-Type-LabelingJobAlgorithmsConfig-LabelingJobAlgorithmSpecificationArn)": "string",
      "[LabelingJobResourceConfig](API_LabelingJobAlgorithmsConfig.md#SageMaker-Type-LabelingJobAlgorithmsConfig-LabelingJobResourceConfig)": { 
         "[VolumeKmsKeyId](API_LabelingJobResourceConfig.md#SageMaker-Type-LabelingJobResourceConfig-VolumeKmsKeyId)": "string"
      }
   },
   "[LabelingJobArn](#SageMaker-DescribeLabelingJob-response-LabelingJobArn)": "string",
   "[LabelingJobName](#SageMaker-DescribeLabelingJob-response-LabelingJobName)": "string",
   "[LabelingJobOutput](#SageMaker-DescribeLabelingJob-response-LabelingJobOutput)": { 
      "[FinalActiveLearningModelArn](API_LabelingJobOutput.md#SageMaker-Type-LabelingJobOutput-FinalActiveLearningModelArn)": "string",
      "[OutputDatasetS3Uri](API_LabelingJobOutput.md#SageMaker-Type-LabelingJobOutput-OutputDatasetS3Uri)": "string"
   },
   "[LabelingJobStatus](#SageMaker-DescribeLabelingJob-response-LabelingJobStatus)": "string",
   "[LastModifiedTime](#SageMaker-DescribeLabelingJob-response-LastModifiedTime)": number,
   "[OutputConfig](#SageMaker-DescribeLabelingJob-response-OutputConfig)": { 
      "[KmsKeyId](API_LabelingJobOutputConfig.md#SageMaker-Type-LabelingJobOutputConfig-KmsKeyId)": "string",
      "[S3OutputPath](API_LabelingJobOutputConfig.md#SageMaker-Type-LabelingJobOutputConfig-S3OutputPath)": "string"
   },
   "[RoleArn](#SageMaker-DescribeLabelingJob-response-RoleArn)": "string",
   "[StoppingConditions](#SageMaker-DescribeLabelingJob-response-StoppingConditions)": { 
      "[MaxHumanLabeledObjectCount](API_LabelingJobStoppingConditions.md#SageMaker-Type-LabelingJobStoppingConditions-MaxHumanLabeledObjectCount)": number,
      "[MaxPercentageOfInputDatasetLabeled](API_LabelingJobStoppingConditions.md#SageMaker-Type-LabelingJobStoppingConditions-MaxPercentageOfInputDatasetLabeled)": number
   },
   "[Tags](#SageMaker-DescribeLabelingJob-response-Tags)": [ 
      { 
         "[Key](API_Tag.md#SageMaker-Type-Tag-Key)": "string",
         "[Value](API_Tag.md#SageMaker-Type-Tag-Value)": "string"
      }
   ]
}
```

## Response Elements<a name="API_DescribeLabelingJob_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [CreationTime](#API_DescribeLabelingJob_ResponseSyntax) **   <a name="SageMaker-DescribeLabelingJob-response-CreationTime"></a>
The date and time that the labeling job was created\.  
Type: Timestamp

 ** [FailureReason](#API_DescribeLabelingJob_ResponseSyntax) **   <a name="SageMaker-DescribeLabelingJob-response-FailureReason"></a>
If the job failed, the reason that it failed\.   
Type: String  
Length Constraints: Maximum length of 1024\.

 ** [HumanTaskConfig](#API_DescribeLabelingJob_ResponseSyntax) **   <a name="SageMaker-DescribeLabelingJob-response-HumanTaskConfig"></a>
Configuration information required for human workers to complete a labeling task\.  
Type: [HumanTaskConfig](API_HumanTaskConfig.md) object

 ** [InputConfig](#API_DescribeLabelingJob_ResponseSyntax) **   <a name="SageMaker-DescribeLabelingJob-response-InputConfig"></a>
Input configuration information for the labeling job, such as the Amazon S3 location of the data objects and the location of the manifest file that describes the data objects\.  
Type: [LabelingJobInputConfig](API_LabelingJobInputConfig.md) object

 ** [JobReferenceCode](#API_DescribeLabelingJob_ResponseSyntax) **   <a name="SageMaker-DescribeLabelingJob-response-JobReferenceCode"></a>
A unique identifier for work done as part of a labeling job\.  
Type: String  
Length Constraints: Minimum length of 1\.  
Pattern: `.+` 

 ** [LabelAttributeName](#API_DescribeLabelingJob_ResponseSyntax) **   <a name="SageMaker-DescribeLabelingJob-response-LabelAttributeName"></a>
The attribute used as the label in the output manifest file\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 127\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*` 

 ** [LabelCategoryConfigS3Uri](#API_DescribeLabelingJob_ResponseSyntax) **   <a name="SageMaker-DescribeLabelingJob-response-LabelCategoryConfigS3Uri"></a>
The S3 location of the JSON file that defines the categories used to label data objects\.  
The file is a JSON structure in the following format:  
 `{`   
 ` "document-version": "2018-11-28"`   
 ` "labels": [`   
 ` {`   
 ` "label": "label 1"`   
 ` },`   
 ` {`   
 ` "label": "label 2"`   
 ` },`   
 ` ...`   
 ` {`   
 ` "label": "label n"`   
 ` }`   
 ` ]`   
 `}`   
Type: String  
Length Constraints: Maximum length of 1024\.  
Pattern: `^(https|s3)://([^/]+)/?(.*)$` 

 ** [LabelCounters](#API_DescribeLabelingJob_ResponseSyntax) **   <a name="SageMaker-DescribeLabelingJob-response-LabelCounters"></a>
Provides a breakdown of the number of data objects labeled by humans, the number of objects labeled by machine, the number of objects than couldn't be labeled, and the total number of objects labeled\.   
Type: [LabelCounters](API_LabelCounters.md) object

 ** [LabelingJobAlgorithmsConfig](#API_DescribeLabelingJob_ResponseSyntax) **   <a name="SageMaker-DescribeLabelingJob-response-LabelingJobAlgorithmsConfig"></a>
Configuration information for automated data labeling\.  
Type: [LabelingJobAlgorithmsConfig](API_LabelingJobAlgorithmsConfig.md) object

 ** [LabelingJobArn](#API_DescribeLabelingJob_ResponseSyntax) **   <a name="SageMaker-DescribeLabelingJob-response-LabelingJobArn"></a>
The Amazon Resource Name \(ARN\) of the labeling job\.  
Type: String  
Length Constraints: Maximum length of 2048\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:labeling-job/.*` 

 ** [LabelingJobName](#API_DescribeLabelingJob_ResponseSyntax) **   <a name="SageMaker-DescribeLabelingJob-response-LabelingJobName"></a>
The name assigned to the labeling job when it was created\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*` 

 ** [LabelingJobOutput](#API_DescribeLabelingJob_ResponseSyntax) **   <a name="SageMaker-DescribeLabelingJob-response-LabelingJobOutput"></a>
The location of the output produced by the labeling job\.  
Type: [LabelingJobOutput](API_LabelingJobOutput.md) object

 ** [LabelingJobStatus](#API_DescribeLabelingJob_ResponseSyntax) **   <a name="SageMaker-DescribeLabelingJob-response-LabelingJobStatus"></a>
The processing status of the labeling job\.   
Type: String  
Valid Values:` InProgress | Completed | Failed | Stopping | Stopped` 

 ** [LastModifiedTime](#API_DescribeLabelingJob_ResponseSyntax) **   <a name="SageMaker-DescribeLabelingJob-response-LastModifiedTime"></a>
The date and time that the labeling job was last updated\.  
Type: Timestamp

 ** [OutputConfig](#API_DescribeLabelingJob_ResponseSyntax) **   <a name="SageMaker-DescribeLabelingJob-response-OutputConfig"></a>
The location of the job's output data and the AWS Key Management Service key ID for the key used to encrypt the output data, if any\.  
Type: [LabelingJobOutputConfig](API_LabelingJobOutputConfig.md) object

 ** [RoleArn](#API_DescribeLabelingJob_ResponseSyntax) **   <a name="SageMaker-DescribeLabelingJob-response-RoleArn"></a>
The Amazon Resource Name \(ARN\) that Amazon SageMaker assumes to perform tasks on your behalf during data labeling\.  
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.  
Pattern: `^arn:aws[a-z\-]*:iam::\d{12}:role/?[a-zA-Z_0-9+=,.@\-_/]+$` 

 ** [StoppingConditions](#API_DescribeLabelingJob_ResponseSyntax) **   <a name="SageMaker-DescribeLabelingJob-response-StoppingConditions"></a>
A set of conditions for stopping a labeling job\. If any of the conditions are met, the job is automatically stopped\.  
Type: [LabelingJobStoppingConditions](API_LabelingJobStoppingConditions.md) object

 ** [Tags](#API_DescribeLabelingJob_ResponseSyntax) **   <a name="SageMaker-DescribeLabelingJob-response-Tags"></a>
An array of key/value pairs\. For more information, see [Using Cost Allocation Tags](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html#allocation-what) in the *AWS Billing and Cost Management User Guide*\.  
Type: Array of [Tag](API_Tag.md) objects  
Array Members: Minimum number of 0 items\. Maximum number of 50 items\.

## Errors<a name="API_DescribeLabelingJob_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ResourceNotFound**   
Resource being access is not found\.  
HTTP Status Code: 400

## See Also<a name="API_DescribeLabelingJob_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/DescribeLabelingJob) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/DescribeLabelingJob) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/DescribeLabelingJob) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/DescribeLabelingJob) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/DescribeLabelingJob) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/DescribeLabelingJob) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/DescribeLabelingJob) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/DescribeLabelingJob) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/DescribeLabelingJob) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/DescribeLabelingJob) 