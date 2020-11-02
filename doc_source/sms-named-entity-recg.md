# Named Entity Recognition<a name="sms-named-entity-recg"></a>

To extract information from unstructured text and classify it into predefined categories, use an Amazon SageMaker Ground Truth named entity recognition \(NER\) labeling task\. Traditionally, NER involves sifting through text data to locate noun phrases, called *named entities*, and categorizing each with a label, such as "person," "organization," or "brand\." You can broaden this task to label longer spans of text and categorize those sequences with predefined labels that you specify\. 

When tasked with a named entity recognition labeling job, workers apply your labels to specific words or phrases within a larger text block\. They choose a label, then apply it by using the cursor to highlight the part of the text to which the label applies\. Workers can't apply multiple labels to the same text, and labels can't overlap\. 

You can create a named entity recognition labeling job using the Ground Truth section of the Amazon SageMaker console or the [ `CreateLabelingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html) operation\.

**Important**  
If you manually create an input manifest file, use `"source"` to identify the text that you want labeled\. For more information, see [Input Data](sms-data-input.md)\.

## Create a Named Entity Recognition Labeling Job \(Console\)<a name="sms-creating-ner-console"></a>

You can follow the instructions [Create a Labeling Job \(Console\)](sms-create-labeling-job-console.md) to learn how to create a named entity recognition labeling job in the SageMaker console\. In Step 10, choose **Text** from the **Task category** drop down menu, and choose **Named entity recognition ** as the task type\. 

Ground Truth provides a worker UI similar to the following for labeling tasks\. When you create the labeling job with the console, you specify instructions to help workers complete the job and labels that workers can choose from\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/named-entity-rec-example.png)

## Create a Named Entity Recognition Labeling Job \(API\)<a name="sms-creating-ner-api"></a>

To create a named entity recognition labeling job, using the SageMaker API operation `CreateLabelingJob`\. This API defines this operation for all AWS SDKs\. To see a list of language\-specific SDKs supported for this operation, review the **See Also** section of [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html)\.

Follow the instructions on [Create a Labeling Job \(API\)](sms-create-labeling-job-api.md) and do the following while you configure your request: 
+ Pre\-annotation Lambda functions for this task type end with `PRE-NamedEntityRecognition`\. To find the pre\-annotation Lambda ARN for your Region, see [PreHumanTaskLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/dg/API_HumanTaskConfig.html#SageMaker-Type-HumanTaskConfig-PreHumanTaskLambdaArn) \. 
+ Annotation\-consolidation Lambda functions for this task type end with `ACS-NamedEntityRecognition`\. To find the annotation\-consolidation Lambda ARN for your Region, see [AnnotationConsolidationLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/dg/API_AnnotationConsolidationConfig.html#SageMaker-Type-AnnotationConsolidationConfig-AnnotationConsolidationLambdaArn)\. 

The following is an example of an [AWS Python SDK \(Boto3\) request](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_labeling_job) to create a labeling job in the US East \(N\. Virginia\) Region\. All parameters in red should be replaced with your specifications and resources\. 

```
response = client.create_labeling_job(
    LabelingJobName='example-ner-labeling-job,
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
        'PreHumanTaskLambdaArn': 'arn:aws:lambda:us-east-1:432418664414:function:PRE-NamedEntityRecognition,
        'TaskKeywords': [
            'Named entity Recognition',
        ],
        'TaskTitle': 'Named entity Recognition task',
        'TaskDescription': 'Apply the labels provided to specific words or phrases within the larger text block.',
        'NumberOfHumanWorkersPerDataObject': 123,
        'TaskTimeLimitInSeconds': 123,
        'TaskAvailabilityLifetimeInSeconds': 123,
        'MaxConcurrentTaskCount': 123,
        'AnnotationConsolidationConfig': {
            'AnnotationConsolidationLambdaArn': 'arn:aws:lambda:us-east-1:432418664414:function:ACS-NamedEntityRecognition'
        },
    Tags=[
        {
            'Key': 'string',
            'Value': 'string'
        },
    ]
)
```

### Provide a Template for Named Entity Recognition Labeling Jobs<a name="worker-template-ner"></a>

If you create a labeling job using the API, you must supply a worker task template in `UiTemplateS3Uri`\. Copy and modify the following template\. Only modify the [https://docs.aws.amazon.com/sagemaker/latest/dg/sms-creating-instruction-pages.html#sms-creating-quick-instructions](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-creating-instruction-pages.html#sms-creating-quick-instructions), [https://docs.aws.amazon.com/sagemaker/latest/dg/sms-creating-instruction-pages.html#sms-creating-full-instructions](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-creating-instruction-pages.html#sms-creating-full-instructions), and `header`\. 

Upload this template to S3, and provide the S3 URI for this file in `UiTemplateS3Uri`\.

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>
<crowd-entity-annotation
  name="crowd-entity-annotation"
  header="please classify"
  labels="{{ task.input.labels | to_json | escape }}"
  text="{{ task.input.taskObject }}"
>
  <full-instructions header="Named entity recognition instructions">
    <ol><li><strong>Read</strong> the text carefully.</li>
    <li><strong>Highlight</strong> words, phrases, or sections of the text.</li>
    <li><strong>Choose</strong> the label that best matches what you have highlighted.</li>
    <li>To <strong>change</strong> a label, choose highlighted text and select a new label.</li>
    <li>To <strong>remove</strong> a label from highlighted text, choose the X next to the abbreviated label name on the highlighted text.</li>
    <li>You can select all of a previously highlighted text, but not a portion of it.</li></ol>
  </full-instructions>
  <short-instructions>
    <p>Enter description of the labels that workers have to choose from</p>
    <p><br></p><p><br></p>
    <p>Add examples to help workers understand the label</p>
    <p><br></p><p><br></p><p><br></p><p><br></p><p><br></p>
  </short-instructions>
 </crowd-entity-annotation>
```

## Named Entity Recognition Output Data<a name="sms-ner-output-data"></a>

Once you have created a named entity recognition labeling job, your output data will be located in the Amazon S3 bucket specified in the `S3OutputPath` parameter when using the API or in the **Output dataset location** field of the **Job overview** section of the console\. 

To learn more about the output manifest file generated by Ground Truth and the file structure the Ground Truth uses to store your output data, see [Output Data](sms-data-output.md)\. 