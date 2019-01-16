# CreateLabelingJob<a name="API_CreateLabelingJob"></a>

Creates a job that uses workers to label the data objects in your input dataset\. You can use the labeled data to train machine learning models\.

You can select your workforce from one of three providers:
+ A private workforce that you create\. It can include employees, contractors, and outside experts\. Use a private workforce when want the data to stay within your organization or when a specific set of skills is required\.
+ One or more vendors that you select from the AWS Marketplace\. Vendors provide expertise in specific areas\. 
+ The Amazon Mechanical Turk workforce\. This is the largest workforce, but it should only be used for public data or data that has been stripped of any personally identifiable information\.

You can also use *automated data labeling* to reduce the number of data objects that need to be labeled by a human\. Automated data labeling uses *active learning* to determine if a data object can be labeled by machine or if it needs to be sent to a human worker\. For more information, see [Using Automated Data Labeling](http://docs.aws.amazon.com/sagemaker/latest/dg/sms-automated-labeling.html)\.

The data objects to be labeled are contained in an Amazon S3 bucket\. You create a *manifest file* that describes the location of each object\. For more information, see [Using Input and Output Data](http://docs.aws.amazon.com/sagemaker/latest/dg/sms-data.html)\.

The output can be used as the manifest file for another labeling job or as training data for your machine learning models\.

## Request Syntax<a name="API_CreateLabelingJob_RequestSyntax"></a>

```
{
   "[HumanTaskConfig](#SageMaker-CreateLabelingJob-request-HumanTaskConfig)": { 
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
   "[InputConfig](#SageMaker-CreateLabelingJob-request-InputConfig)": { 
      "[DataAttributes](API_LabelingJobInputConfig.md#SageMaker-Type-LabelingJobInputConfig-DataAttributes)": { 
         "[ContentClassifiers](API_LabelingJobDataAttributes.md#SageMaker-Type-LabelingJobDataAttributes-ContentClassifiers)": [ "string" ]
      },
      "[DataSource](API_LabelingJobInputConfig.md#SageMaker-Type-LabelingJobInputConfig-DataSource)": { 
         "[S3DataSource](API_LabelingJobDataSource.md#SageMaker-Type-LabelingJobDataSource-S3DataSource)": { 
            "[ManifestS3Uri](API_LabelingJobS3DataSource.md#SageMaker-Type-LabelingJobS3DataSource-ManifestS3Uri)": "string"
         }
      }
   },
   "[LabelAttributeName](#SageMaker-CreateLabelingJob-request-LabelAttributeName)": "string",
   "[LabelCategoryConfigS3Uri](#SageMaker-CreateLabelingJob-request-LabelCategoryConfigS3Uri)": "string",
   "[LabelingJobAlgorithmsConfig](#SageMaker-CreateLabelingJob-request-LabelingJobAlgorithmsConfig)": { 
      "[InitialActiveLearningModelArn](API_LabelingJobAlgorithmsConfig.md#SageMaker-Type-LabelingJobAlgorithmsConfig-InitialActiveLearningModelArn)": "string",
      "[LabelingJobAlgorithmSpecificationArn](API_LabelingJobAlgorithmsConfig.md#SageMaker-Type-LabelingJobAlgorithmsConfig-LabelingJobAlgorithmSpecificationArn)": "string",
      "[LabelingJobResourceConfig](API_LabelingJobAlgorithmsConfig.md#SageMaker-Type-LabelingJobAlgorithmsConfig-LabelingJobResourceConfig)": { 
         "[VolumeKmsKeyId](API_LabelingJobResourceConfig.md#SageMaker-Type-LabelingJobResourceConfig-VolumeKmsKeyId)": "string"
      }
   },
   "[LabelingJobName](#SageMaker-CreateLabelingJob-request-LabelingJobName)": "string",
   "[OutputConfig](#SageMaker-CreateLabelingJob-request-OutputConfig)": { 
      "[KmsKeyId](API_LabelingJobOutputConfig.md#SageMaker-Type-LabelingJobOutputConfig-KmsKeyId)": "string",
      "[S3OutputPath](API_LabelingJobOutputConfig.md#SageMaker-Type-LabelingJobOutputConfig-S3OutputPath)": "string"
   },
   "[RoleArn](#SageMaker-CreateLabelingJob-request-RoleArn)": "string",
   "[StoppingConditions](#SageMaker-CreateLabelingJob-request-StoppingConditions)": { 
      "[MaxHumanLabeledObjectCount](API_LabelingJobStoppingConditions.md#SageMaker-Type-LabelingJobStoppingConditions-MaxHumanLabeledObjectCount)": number,
      "[MaxPercentageOfInputDatasetLabeled](API_LabelingJobStoppingConditions.md#SageMaker-Type-LabelingJobStoppingConditions-MaxPercentageOfInputDatasetLabeled)": number
   },
   "[Tags](#SageMaker-CreateLabelingJob-request-Tags)": [ 
      { 
         "[Key](API_Tag.md#SageMaker-Type-Tag-Key)": "string",
         "[Value](API_Tag.md#SageMaker-Type-Tag-Value)": "string"
      }
   ]
}
```

## Request Parameters<a name="API_CreateLabelingJob_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [HumanTaskConfig](#API_CreateLabelingJob_RequestSyntax) **   <a name="SageMaker-CreateLabelingJob-request-HumanTaskConfig"></a>
Configures the information required for human workers to complete a labeling task\.  
Type: [HumanTaskConfig](API_HumanTaskConfig.md) object  
Required: Yes

 ** [InputConfig](#API_CreateLabelingJob_RequestSyntax) **   <a name="SageMaker-CreateLabelingJob-request-InputConfig"></a>
Input data for the labeling job, such as the Amazon S3 location of the data objects and the location of the manifest file that describes the data objects\.  
Type: [LabelingJobInputConfig](API_LabelingJobInputConfig.md) object  
Required: Yes

 ** [LabelAttributeName](#API_CreateLabelingJob_RequestSyntax) **   <a name="SageMaker-CreateLabelingJob-request-LabelAttributeName"></a>
The attribute name to use for the label in the output manifest file\. This is the key for the key/value pair formed with the label that a worker assigns to the object\. The name can't end with "\-metadata"\. If you are running a semantic segmentation labeling job, the attribute name must end with "\-ref"\. If you are running any other kind of labeling job, the attribute name must not end with "\-ref"\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 127\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

 ** [LabelCategoryConfigS3Uri](#API_CreateLabelingJob_RequestSyntax) **   <a name="SageMaker-CreateLabelingJob-request-LabelCategoryConfigS3Uri"></a>
The S3 URL of the file that defines the categories used to label the data objects\.  
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
Required: No

 ** [LabelingJobAlgorithmsConfig](#API_CreateLabelingJob_RequestSyntax) **   <a name="SageMaker-CreateLabelingJob-request-LabelingJobAlgorithmsConfig"></a>
Configures the information required to perform automated data labeling\.  
Type: [LabelingJobAlgorithmsConfig](API_LabelingJobAlgorithmsConfig.md) object  
Required: No

 ** [LabelingJobName](#API_CreateLabelingJob_RequestSyntax) **   <a name="SageMaker-CreateLabelingJob-request-LabelingJobName"></a>
The name of the labeling job\. This name is used to identify the job in a list of labeling jobs\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

 ** [OutputConfig](#API_CreateLabelingJob_RequestSyntax) **   <a name="SageMaker-CreateLabelingJob-request-OutputConfig"></a>
The location of the output data and the AWS Key Management Service key ID for the key used to encrypt the output data, if any\.  
Type: [LabelingJobOutputConfig](API_LabelingJobOutputConfig.md) object  
Required: Yes

 ** [RoleArn](#API_CreateLabelingJob_RequestSyntax) **   <a name="SageMaker-CreateLabelingJob-request-RoleArn"></a>
The Amazon Resource Number \(ARN\) that Amazon SageMaker assumes to perform tasks on your behalf during data labeling\. You must grant this role the necessary permissions so that Amazon SageMaker can successfully complete data labeling\.  
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.  
Pattern: `^arn:aws[a-z\-]*:iam::\d{12}:role/?[a-zA-Z_0-9+=,.@\-_/]+$`   
Required: Yes

 ** [StoppingConditions](#API_CreateLabelingJob_RequestSyntax) **   <a name="SageMaker-CreateLabelingJob-request-StoppingConditions"></a>
A set of conditions for stopping the labeling job\. If any of the conditions are met, the job is automatically stopped\. You can use these conditions to control the cost of data labeling\.  
Type: [LabelingJobStoppingConditions](API_LabelingJobStoppingConditions.md) object  
Required: No

 ** [Tags](#API_CreateLabelingJob_RequestSyntax) **   <a name="SageMaker-CreateLabelingJob-request-Tags"></a>
An array of key/value pairs\. For more information, see [Using Cost Allocation Tags](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html#allocation-what) in the *AWS Billing and Cost Management User Guide*\.  
Type: Array of [Tag](API_Tag.md) objects  
Array Members: Minimum number of 0 items\. Maximum number of 50 items\.  
Required: No

## Response Syntax<a name="API_CreateLabelingJob_ResponseSyntax"></a>

```
{
   "[LabelingJobArn](#SageMaker-CreateLabelingJob-response-LabelingJobArn)": "string"
}
```

## Response Elements<a name="API_CreateLabelingJob_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [LabelingJobArn](#API_CreateLabelingJob_ResponseSyntax) **   <a name="SageMaker-CreateLabelingJob-response-LabelingJobArn"></a>
The Amazon Resource Name \(ARN\) of the labeling job\. You use this ARN to identify the labeling job\.  
Type: String  
Length Constraints: Maximum length of 2048\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:labeling-job/.*` 

## Errors<a name="API_CreateLabelingJob_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ResourceInUse**   
Resource being accessed is in use\.  
HTTP Status Code: 400

 **ResourceLimitExceeded**   
 You have exceeded an Amazon SageMaker resource limit\. For example, you might have too many training jobs created\.   
HTTP Status Code: 400

## See Also<a name="API_CreateLabelingJob_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/CreateLabelingJob) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/CreateLabelingJob) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/CreateLabelingJob) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/CreateLabelingJob) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/CreateLabelingJob) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/CreateLabelingJob) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/CreateLabelingJob) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/CreateLabelingJob) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/CreateLabelingJob) 