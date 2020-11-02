# Text Classification \(Single Label\)<a name="sms-text-classification"></a>

To categorize articles and text into predefined categories, use text classification\. For example, you can use text classification to identify the sentiment conveyed in a review or the emotion underlying a section of text\. Use Amazon SageMaker Ground Truth text classification to have workers sort text into categories that you define\. 

You create a text classification labeling job using the Ground Truth section of the Amazon SageMaker console or the [ `CreateLabelingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html) operation\. 

**Important**  
If you manually create an input manifest file, use `"source"` to identify the text that you want labeled\. For more information, see [Input Data](sms-data-input.md)\.

## Create a Text Classification Labeling Job \(Console\)<a name="sms-creating-text-classification-console"></a>

You can follow the instructions [Create a Labeling Job \(Console\)](sms-create-labeling-job-console.md) to learn how to create a text classification labeling job in the SageMaker console\. In Step 10, choose **Text** from the **Task category** drop down menu, and choose **Text Classification \(Single Label\)** as the task type\. 

Ground Truth provides a worker UI similar to the following for labeling tasks\. When you create the labeling job with the console, you specify instructions to help workers complete the job and labels that workers can choose from\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/text-classification-example.png)

## Create a Text Classification Labeling Job \(API\)<a name="sms-creating-text-classification-api"></a>

To create a text classification labeling job, use the SageMaker API operation `CreateLabelingJob`\. This API defines this operation for all AWS SDKs\. To see a list of language\-specific SDKs supported for this operation, review the **See Also** section of [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html)\.

Follow the instructions on [Create a Labeling Job \(API\)](sms-create-labeling-job-api.md) and do the following while you configure your request: 
+ Pre\-annotation Lambda functions for this task type end with `PRE-TextMultiClass`\. To find the pre\-annotation Lambda ARN for your Region, see [PreHumanTaskLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/dg/API_HumanTaskConfig.html#SageMaker-Type-HumanTaskConfig-PreHumanTaskLambdaArn) \. 
+ Annotation\-consolidation Lambda functions for this task type end with `ACS-TextMultiClass`\. To find the annotation\-consolidation Lambda ARN for your Region, see [AnnotationConsolidationLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/dg/API_AnnotationConsolidationConfig.html#SageMaker-Type-AnnotationConsolidationConfig-AnnotationConsolidationLambdaArn)\. 

The following is an example of an [AWS Python SDK \(Boto3\) request](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_labeling_job) to create a labeling job in the US East \(N\. Virginia\) Region\. All parameters in red should be replaced with your specifications and resources\. 

```
response = client.create_labeling_job(
    LabelingJobName='example-text-classification-labeling-job,
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
            'UiTemplateS3Uri': 's3://bucket/path/worker-task-template.html'
        },
        'PreHumanTaskLambdaArn': 'arn:aws:lambda:us-east-1:432418664414:function:PRE-TextMultiClass,
        'TaskKeywords': [
            Text classification',
        ],
        'TaskTitle': Text classification task',
        'TaskDescription': 'Carefully read and classify this text using the categories provided.',
        'NumberOfHumanWorkersPerDataObject': 123,
        'TaskTimeLimitInSeconds': 123,
        'TaskAvailabilityLifetimeInSeconds': 123,
        'MaxConcurrentTaskCount': 123,
        'AnnotationConsolidationConfig': {
            'AnnotationConsolidationLambdaArn': 'arn:aws:lambda:us-east-1:432418664414:function:ACS-TextMultiClass'
        },
    Tags=[
        {
            'Key': 'string',
            'Value': 'string'
        },
    ]
)
```

### Provide a Template for Text Classification Labeling Jobs<a name="worker-template-text-classification"></a>

If you create a labeling job using the API, you must supply a worker task template in `UiTemplateS3Uri`\. Copy and modify the following template\. Only modify the [https://docs.aws.amazon.com/sagemaker/latest/dg/sms-creating-instruction-pages.html#sms-creating-quick-instructions](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-creating-instruction-pages.html#sms-creating-quick-instructions), [https://docs.aws.amazon.com/sagemaker/latest/dg/sms-creating-instruction-pages.html#sms-creating-full-instructions](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-creating-instruction-pages.html#sms-creating-full-instructions), and `header`\. 

Upload this template to S3, and provide the S3 URI for this file in `UiTemplateS3Uri`\.

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>
<crowd-form>
  <crowd-classifier
    name="crowd-classifier"
    categories="{{ task.input.labels | to_json | escape }}"
    header="classify text"
  >
    <classification-target style="white-space: pre-wrap">
      {{ task.input.taskObject }}
    </classification-target>
    <full-instructions header="Classifier instructions">
      <ol><li><strong>Read</strong> the text carefully.</li>
      <li><strong>Read</strong> the examples to understand more about the options.</li>
      <li><strong>Choose</strong> the appropriate labels that best suit the text.</li></ol>
    </full-instructions>
    <short-instructions>
      <p>Enter description of the labels that workers have to choose from</p>
      <p><br></p><p><br></p><p>Add examples to help workers understand the label</p>
      <p><br></p><p><br></p><p><br></p><p><br></p><p><br></p>
    </short-instructions>
  </crowd-classifier>
  </crowd-form>
```

## Text Classification Output Data<a name="sms-text-classification-output-data"></a>

Once you have created a text classification labeling job, your output data will be located in the Amazon S3 bucket specified in the `S3OutputPath` parameter when using the API or in the **Output dataset location** field of the **Job overview** section of the console\. 

To learn more about the output manifest file generated by Ground Truth and the file structure the Ground Truth uses to store your output data, see [Output Data](sms-data-output.md)\. 

To see an example of an output manifest files from a text classification labeling job, see [Classification Job Output](sms-data-output.md#sms-output-class)\.