# Text Classification \(Multi\-label\)<a name="sms-text-classification-multilabel"></a>

To categorize articles and text into multiple predefined categories, use the multi\-label text classification task type\. For example, you can use this task type to identify more than one emotion conveyed in text\. 

When working on a multi\-label text classification task, workers should choose all applicable labels, but must choose at least one\. When creating a job using this task type, you can provide up to 50 label categories\. 

Amazon SageMaker Ground Truth doesn't provide a "none" category for when none of the labels applies\. To provide this option to workers, include a label similar to "none" or "other" when you create a multi\-label text classification job\. 

When you create a multi\-label text classification job in the console, you will be provided with a default worker UI template that you can use to:
+ Specify the labels that you want the worker to see 
+ Create worker instructions

See [Create a Multi\-Label Text Classification Labeling Job \(Console\)](#sms-creating-multilabel-text-classification-console) for an example of the template provided in the console\. 

When you create a multi\-label text classification job using the Amazon SageMaker API or your preferred Amazon SageMaker SDK, you will need to provide a custom worker task template which will be used to generated your workers' UI\. For more information, see [Create a Custom Template for Multi\-label Text Classification](#custom-template-multi-label-text-classification)\.

To restrict workers to choosing a single label for each document or text selection, use the [Text Classification](sms-text-classification.md) task type\. 

**Note**  
Automated labeling \(auto\-labeling\) isn't supported for the multi\-label text classification task type\. 

**Topics**
+ [Create a Multi\-Label Text Classification Labeling Job \(Console\)](#sms-creating-multilabel-text-classification-console)
+ [Create a Multi\-Label Text Classification Labeling Job \(API\)](#sms-creating-multilabel-text-classification-api)
+ [Multi\-label Text Classification Output Data](#sms-text-classification-multi-select-output-data)

## Create a Multi\-Label Text Classification Labeling Job \(Console\)<a name="sms-creating-multilabel-text-classification-console"></a>

To create a multi\-label text classification labeling job, you use the Ground Truth section of the Amazon SageMaker console\. Ground Truth provides a worker console similar to the following for labeling tasks\. When you create the labeling job with the console, you can modify the contents that are shown\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/text-classification-multilabel-example.png)

To create a multi\-label text classification labeling job, use the instructions in the [Getting started](sms-getting-started.md) guide: For [Step 2: Create a Labeling Job](sms-getting-started-step2.md), choose **Text classification \(Multi\-label\)** as the** Task type**\.

## Create a Multi\-Label Text Classification Labeling Job \(API\)<a name="sms-creating-multilabel-text-classification-api"></a>

To create a multi\-label text classification labeling job using the Amazon SageMaker API, you use the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html) operation\. To use this operation, you need:
+ A custom worker task template\. For information, see [Create a Custom Template for Multi\-label Text Classification](#custom-template-multi-label-text-classification)\.
+ At least one Amazon Simple Storage Service \(Amazon S3\) bucket to store your input and output data\. 
+ An input manifest file that specifies your input data\. For information about creating an input manifest, see [Input Data](sms-data-input.md)\. 
+ An AWS Identity and Access Management \(IAM\) role with the AmazonSageMakerFullAccess IAM policy attached and with permissions to access your S3 buckets\. For example, if the text or documents specified in your manifest file are in an S3 bucket named `my_input_bucket`, and if you want the label data to be stored in a bucket named `my_output_bucket`, you would attach the following IAM policy to the role that is passed to the `CreateLabelingJob` operation\.

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Effect": "Allow",
              "Action": [
                  "s3:GetObject"
              ],
              "Resource": [
                  "arn:aws:s3:::my_input_bucket/*"
              ]
          },
          {
              "Effect": "Allow",
              "Action": [
                  "s3:PutObject"
              ],
              "Resource": [
                  "arn:aws:s3:::my_output_bucket/*"
              ]
          }
      ]
  }
  ```
+ The ARN for a pre\-annotation AWS Lambda function that will be used to process your input data\. These Lambda functions are predefined in each AWS Region for multi\-label text classification labeling jobs\. Pre\-annotation Lambda functions for this task type end with `PRE-TextMultiClassMultiLabel`\. To find the pre\-annotation Lambda ARN for your Region, see [PreHumanTaskLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/dg/API_HumanTaskConfig.html#SageMaker-Type-HumanTaskConfig-PreHumanTaskLambdaArn)\. 
+ A work team ARN\. To learn about work teams and workforces, see [Create and Manage Workforces](sms-workforce-management.md)\.

  If you use the Amazon Mechanical Turk public workforce, use the `ContentClassifiers` parameter in `CreateLabelingJob` to declare that your content is free of personally identifiable information or adult content\. If your content contains personally identifiable information or adult content, Amazon SageMaker might restrict the Amazon Mechanical Turk workers that can view your task\.
+ \(Optional\) To have multiple workers label a single text passage \(by inputting a number greater than one for the `NumberOfHumanWorkersPerDataObject` parameter\), include an annotation\-consolidation Lambda ARN as an input to the `AnnotationConsolidationLambdaArn` parameter\. These Lambda functions are predefined in each AWS Region for multi\-label text classification labeling jobs\. Annotation\-consolidation Lambda functions for this task type end with `ACS-TextMultiClassMultiLabel`\. To find the annotation\-consolidation Lambda ARN for your Region, see [AnnotationConsolidationLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/dg/API_AnnotationConsolidationConfig.html#SageMaker-Type-AnnotationConsolidationConfig-AnnotationConsolidationLambdaArn)\. 

The following is an example of an [AWS Python SDK \(Boto3\) request](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_labeling_job) to create a labeling job in the US East \(N\. Virginia\) Region\. 

```
response = client.create_labeling_job(
    LabelingJobName='example-multi-label-text-classification-labeling-job,
    LabelAttributeName='label',
    InputConfig={
        'DataSource': {
            'S3DataSource': {
                'ManifestS3Uri': 's3://bucket/path/manifest-with-input-data.json'
            }
        },
        'DataAttributes': {
            'ContentClassifiers': [
                'FreeOfPersonallyIdentifiableInformation'|'FreeOfAdultContent',
            ]
        }
    },
    OutputConfig={
        'S3OutputPath': 's3://bucket/path/file-to-store-output-data',
        'KmsKeyId': 'string'
    },
    RoleArn='arn:aws:iam::*:role/*,
    LabelCategoryConfigS3Uri='s3://bucket/path/label-categories.json',
    StoppingConditions={
        'MaxHumanLabeledObjectCount': 123,
        'MaxPercentageOfInputDatasetLabeled': 123
    },
    HumanTaskConfig={
        'WorkteamArn': 'arn:aws:sagemaker:region:*:workteam/private-crowd/*',
        'UiConfig': {
            'UiTemplateS3Uri': 's3://bucket/path/custom-worker-task-template.html'
        },
        'PreHumanTaskLambdaArn': 'arn:aws:lambda::function:PRE-TextMultiClassMultiLabel',
        'TaskKeywords': [
            'Text Classification',
        ],
        'TaskTitle': 'Multi-label text classification task',
        'TaskDescription': 'Select all labels that apply to the text shown',
        'NumberOfHumanWorkersPerDataObject': 123,
        'TaskTimeLimitInSeconds': 123,
        'TaskAvailabilityLifetimeInSeconds': 123,
        'MaxConcurrentTaskCount': 123,
        'AnnotationConsolidationConfig': {
            'AnnotationConsolidationLambdaArn': 'arn:aws:lambda:us-east-1:432418664414:function:ACS-TextMultiClassMultiLabel'
        },
    Tags=[
        {
            'Key': 'string',
            'Value': 'string'
        },
    ]
)
```

For more information about this operation, see [CreateLabelingJob](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateLabelingJob.html)\. For information about how to use other language\-specific SDKs, see [See Also](https://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateLabelingJob.html#API_CreateLabelingJob_SeeAlso) in the `CreateLabelingJobs` topic\. 

### Create a Custom Template for Multi\-label Text Classification<a name="custom-template-multi-label-text-classification"></a>

If you create a labeling job using the API, you must create and supply a custom worker task template\. You can use the Amazon SageMaker [crowd\-classifier\-multi\-select](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-ui-template-crowd-classifier-multi-select.html) HTML crowd element to create a template for a multi\-label text classification job\. The following example shows how to use this crowd element\.

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>

<crowd-form>
    <crowd-classifier-multi-select
      name="category"
      categories="['Positive', 'Negative', 'Neutral']"
      header="Select the relevant categories"
    >
      <classification-target>
        {{ task.input.taskObject }}
      </classification-target>
      
      <full-instructions header="Text Categorization Instructions">
        <p><strong>Positive</strong> sentiment include: joy, excitement, delight</p>
        <p><strong>Negative</strong> sentiment include: anger, sarcasm, anxiety</p>
        <p><strong>Neutral</strong>: neither positive or negative, such as stating a fact</p>
        <p><strong>N/A</strong>: when the text cannot be understood</p>
        <p>When the sentiment is mixed, such as both joy and sadness, use your judgment to choose the stronger emotion.</p>
      </full-instructions>

      <short-instructions>
       Choose all categories that are expressed by the text. 
      </short-instructions>
    </crowd-classifier-multi-select>
</crowd-form>
```

To learn how to create a custom template, see [Creating Custom Labeling Workflows](sms-custom-templates.md)\. 

## Multi\-label Text Classification Output Data<a name="sms-text-classification-multi-select-output-data"></a>

Once you have created a multi\-label text classification labeling job, your output data will be located in the Amazon S3 bucket specified in the `S3OutputPath` parameter when using the API or in the **Output dataset location** field of the **Job overview** section of the console\. 

To learn more about the output manifest file generated by Ground Truth and the file structure the Ground Truth uses to store your output data, see [Output Data](sms-data-output.md)\. 

To see an example of output manifest files for multi\-label text classification labeling job, see [Multi\-label Classification Job Output](sms-data-output.md#sms-output-multi-label-classification)\.