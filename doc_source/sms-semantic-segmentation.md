# Image Semantic Segmentation<a name="sms-semantic-segmentation"></a>

To identify the contents of an image at the pixel level, use an Amazon SageMaker Ground Truth semantic segmentation labeling task\. When assigned a semantic segmentation labeling job, workers classify pixels in the image into a set of predefined labels or classes\. Ground Truth supports single and multi\-class semantic segmentation labeling jobs\.

Images that contain large numbers of objects that need to be segmented require more time\. To help workers \(from a private or vendor workforce\) label these objects in less time and with greater accuracy, Ground Truth provides an AI\-assisted auto\-segmentation tool\. For information, see [Auto\-Segmentation Tool](sms-auto-segmentation.md)\.

You create a semantic segmentation labeling job using the Ground Truth section of the Amazon SageMaker console or the [ `CreateLabelingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html) operation\. 

**Important**  
For this task type, if you create your own manifest file, use `"source-ref"` to identify the location of each image file in Amazon S3 that you want labeled\. For more information, see [Input Data](sms-data-input.md)\.

## Creating a Semantic Segmentation Labeling Job \(Console\)<a name="sms-creating-ss-labeling-job-console"></a>

You can follow the instructions [Create a Labeling Job \(Console\)](sms-create-labeling-job-console.md) to learn how to create a semantic segmentation labeling job in the SageMaker console\. In Step 10, choose **Image** from the **Task category** drop down menu, and choose **Semantic segmentation** as the task type\. 

Ground Truth provides a worker UI similar to the following for labeling tasks\. When you create the labeling job with the console, you specify instructions to help workers complete the job and labels that workers can choose from\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/semantic_segmentation_sample.gif)

## Create a Semantic Segmentation Labeling Job \(API\)<a name="sms-creating-ss-labeling-job-api"></a>

To create a semantic segmentation labeling job, use the SageMaker API operation `CreateLabelingJob`\. This API defines this operation for all AWS SDKs\. To see a list of language\-specific SDKs supported for this operation, review the **See Also** section of [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html)\.

Follow the instructions on [Create a Labeling Job \(API\)](sms-create-labeling-job-api.md) and do the following while you configure your request: 
+ Pre\-annotation Lambda functions for this task type end with `PRE-SemanticSegmentation`\. To find the pre\-annotation Lambda ARN for your Region, see [PreHumanTaskLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/dg/API_HumanTaskConfig.html#SageMaker-Type-HumanTaskConfig-PreHumanTaskLambdaArn) \. 
+ Annotation\-consolidation Lambda functions for this task type end with `ACS-SemanticSegmentation`\. To find the annotation\-consolidation Lambda ARN for your Region, see [AnnotationConsolidationLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/dg/API_AnnotationConsolidationConfig.html#SageMaker-Type-AnnotationConsolidationConfig-AnnotationConsolidationLambdaArn)\. 

The following is an example of an [AWS Python SDK \(Boto3\) request](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_labeling_job) to create a labeling job in the US East \(N\. Virginia\) Region\. All parameters in red should be replaced with your specifications and resources\. 

```
response = client.create_labeling_job(
    LabelingJobName='example-semantic-segmentation-labeling-job,
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
        'PreHumanTaskLambdaArn': 'arn:aws:lambda:us-east-1:432418664414:function:PRE-SemanticSegmentation,
        'TaskKeywords': [
            'Semantic Segmentation',
        ],
        'TaskTitle': 'Semantic segmentation task',
        'TaskDescription': 'For each category provided, segment out each relevant object using the color associated with that category',
        'NumberOfHumanWorkersPerDataObject': 123,
        'TaskTimeLimitInSeconds': 123,
        'TaskAvailabilityLifetimeInSeconds': 123,
        'MaxConcurrentTaskCount': 123,
        'AnnotationConsolidationConfig': {
            'AnnotationConsolidationLambdaArn': 'arn:aws:lambda:us-east-1:432418664414:function:ACS-SemanticSegmentation'
        },
    Tags=[
        {
            'Key': 'string',
            'Value': 'string'
        },
    ]
)
```

### Provide a Template for Semantic Segmentation Labeling Jobs<a name="sms-create-labeling-job-ss-api-template"></a>

If you create a labeling job using the API, you must supply a worker task template in `UiTemplateS3Uri`\. Copy and modify the following template\. Only modify the [https://docs.aws.amazon.com/sagemaker/latest/dg/sms-creating-instruction-pages.html#sms-creating-quick-instructions](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-creating-instruction-pages.html#sms-creating-quick-instructions), [https://docs.aws.amazon.com/sagemaker/latest/dg/sms-creating-instruction-pages.html#sms-creating-full-instructions](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-creating-instruction-pages.html#sms-creating-full-instructions), and `header`\. 

Upload this template to S3, and provide the S3 URI for this file in `UiTemplateS3Uri`\.

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>
<crowd-form>
  <crowd-semantic-segmentation
    name="crowd-semantic-segmentation"
    src="{{ task.input.taskObject | grant_read_access }}"
    header="Please segment out all pedestrians."
    labels="{{ task.input.labels | to_json | escape }}"
  >
    <full-instructions header="Segmentation instructions">
      <ol><li><strong>Read</strong> the task carefully and inspect the image.</li>
      <li><strong>Read</strong> the options and review the examples provided to understand more about the labels.</li>
      <li><strong>Choose</strong> the appropriate label that best suits an object and paint that object using the tools provided.</li></ol>
    </full-instructions>
    <short-instructions>
      <h2><span style="color: rgb(0, 138, 0);">Good example</span></h2>
      <p>Enter description to explain a correctly done segmentation</p>
      <p><br></p><h2><span style="color: rgb(230, 0, 0);">Bad example</span></h2>
      <p>Enter description of an incorrectly done segmentation</p>
    </short-instructions>
  </crowd-semantic-segmentation>
</crowd-form>
```

## Semantic Segmentation Output Data<a name="sms-ss-ouput-data"></a>

Once you have created a semantic segmentation labeling job, your output data will be located in the Amazon S3 bucket specified in the `S3OutputPath` parameter when using the API or in the **Output dataset location** field of the **Job overview** section of the console\. 

To learn more about the output manifest file generated by Ground Truth and the file structure the Ground Truth uses to store your output data, see [Output Data](sms-data-output.md)\. 

To see an example of an output manifest file for a semantic segmentation labeling job, see [3D Point Cloud Semantic Segmentation Output](sms-data-output.md#sms-output-point-cloud-segmentation)\.