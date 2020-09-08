# Image Classification \(Multi\-label\)<a name="sms-image-classification-multilabel"></a>

Use an Amazon SageMaker Ground Truth multi\-label image classification labeling task when you need workers to classify multiple objects in an image\. For example, the following image features a dog and a cat\. You can use multi\-label image classification to associate the labels "dog" and "cat" with this image\.

![\[Photo by Anusha Barwa on Unsplash\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/dog-cat-photo.jpg)

When working on a multi\-label image classification task, workers should choose all applicable labels, but must choose at least one\. When creating a job using this task type, you can provide up to 50 label\-categories\. 

When creating a labeling job in the console, Ground Truth doesn't provide a "none" category for when none of the labels applies to an image\. To provide this option to workers, include a label similar to "none" or "other" when you create a multi\-label image classification job\. 

To restrict workers to choosing a single label for each image, use the [Image Classification \(Single Label\)](sms-image-classification.md) task type\.

**Important**  
For this task type, if you create your own manifest file, use `"source-ref"` to identify the location of each image file in Amazon S3 that you want labeled\. For more information, see [Input Data](sms-data-input.md)\.

## Create a Multi\-Label Image Classification Labeling Job \(Console\)<a name="sms-creating-multilabel-image-classification-console"></a>

You can follow the instructions [Create a Labeling Job \(Console\)](sms-create-labeling-job-console.md) to learn how to create a multi\-label image classification labeling job in the SageMaker console\. In Step 10, choose **Image** from the **Task category** drop down menu, and choose **Image Classification \(Multi\-label\)** as the task type\. 

Ground Truth provides a worker UI similar to the following for labeling tasks\. When you create a labeling job in the console, you specify instructions to help workers complete the job and labels that workers can choose from\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/image-classification-multilabel-example.png)

## Create a Multi\-Label Image Classification Labeling Job \(API\)<a name="sms-create-multi-select-image-classification-job-api"></a>

To create a multi\-label image classification labeling job, use the SageMaker API operation `CreateLabelingJob`\. This API defines this operation for all AWS SDKs\. To see a list of language\-specific SDKs supported for this operation, review the **See Also** section of [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html)\.

Follow the instructions on [Create a Labeling Job \(API\)](sms-create-labeling-job-api.md) and do the following while you configure your request: 
+ Pre\-annotation Lambda functions for this task type end with `PRE-ImageMultiClassMultiLabel`\. To find the pre\-annotation Lambda ARN for your Region, see [PreHumanTaskLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/dg/API_HumanTaskConfig.html#SageMaker-Type-HumanTaskConfig-PreHumanTaskLambdaArn) \. 
+ Annotation\-consolidation Lambda functions for this task type end with `ACS-ImageMultiClassMultiLabel`\. To find the annotation\-consolidation Lambda ARN for your Region, see [AnnotationConsolidationLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/dg/API_AnnotationConsolidationConfig.html#SageMaker-Type-AnnotationConsolidationConfig-AnnotationConsolidationLambdaArn)\. 

The following is an example of an [AWS Python SDK \(Boto3\) request](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_labeling_job) to create a labeling job in the US East \(N\. Virginia\) Region\. All parameters in red should be replaced with your specifications and resources\. 

```
response = client.create_labeling_job(
    LabelingJobName='example-multi-label-image-classification-labeling-job,
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
        'PreHumanTaskLambdaArn': 'arn:aws:lambda:us-east-1:432418664414:function:PRE-ImageMultiClassMultiLabel',
        'TaskKeywords': [
            'Image Classification',
        ],
        'TaskTitle': 'Multi-label image classification task',
        'TaskDescription': 'Select all labels that apply to the images shown',
        'NumberOfHumanWorkersPerDataObject': 123,
        'TaskTimeLimitInSeconds': 123,
        'TaskAvailabilityLifetimeInSeconds': 123,
        'MaxConcurrentTaskCount': 123,
        'AnnotationConsolidationConfig': {
            'AnnotationConsolidationLambdaArn': 'arn:aws:lambda:us-east-1:432418664414:function:ACS-ImageMultiClassMultiLabel'
        },
    Tags=[
        {
            'Key': 'string',
            'Value': 'string'
        },
    ]
)
```

### Provide a Template for Multi\-label Image Classification<a name="sms-custom-template-multi-image-label-classification"></a>

If you create a labeling job using the API, you must supply a worker task template in `UiTemplateS3Uri`\. Copy and modify the following template\. Only modify the [https://docs.aws.amazon.com/sagemaker/latest/dg/sms-creating-instruction-pages.html#sms-creating-quick-instructions](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-creating-instruction-pages.html#sms-creating-quick-instructions), [https://docs.aws.amazon.com/sagemaker/latest/dg/sms-creating-instruction-pages.html#sms-creating-full-instructions](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-creating-instruction-pages.html#sms-creating-full-instructions), and `header`\. 

Upload this template to S3, and provide the S3 URI for this file in `UiTemplateS3Uri`\.

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>
<crowd-form>
  <crowd-image-classifier-multi-select
    name="crowd-image-classifier-multi-select"
    src="{{ task.input.taskObject | grant_read_access }}"
    header="Please identify all classes in image"
    categories="{{ task.input.labels | to_json | escape }}"
  >
    <full-instructions header="Multi Label Image classification instructions">
      <ol><li><strong>Read</strong> the task carefully and inspect the image.</li>
      <li><strong>Read</strong> the options and review the examples provided to understand more about the labels.</li>
       <li><strong>Choose</strong> the appropriate labels that best suit the image.</li></ol>
    </full-instructions>
    <short-instructions>
      <h3><span style="color: rgb(0, 138, 0);">Good example</span></h3>
      <p>Enter description to explain the correct label to the workers</p>
      <h3><span style="color: rgb(230, 0, 0);">Bad example</span></h3>
      <p>Enter description of an incorrect label</p>
   </short-instructions>
  </crowd-image-classifier-multi-select>
</crowd-form>
```

## Multi\-label Image Classification Output Data<a name="sms-image-classification-multi-output-data"></a>

Once you have created a multi\-label image classification labeling job, your output data will be located in the Amazon S3 bucket specified in the `S3OutputPath` parameter when using the API or in the **Output dataset location** field of the **Job overview** section of the console\. 

To learn more about the output manifest file generated by Ground Truth and the file structure the Ground Truth uses to store your output data, see [Output Data](sms-data-output.md)\. 

To see an example of output manifest files for multi\-label image classification labeling job, see [Multi\-label Classification Job Output](sms-data-output.md#sms-output-multi-label-classification)\.