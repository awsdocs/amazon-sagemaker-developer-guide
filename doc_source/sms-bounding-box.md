# Bounding Box<a name="sms-bounding-box"></a>

The images used to train a machine learning model often contain more than one object\. To classify and localize one or more objects within images, use the Amazon SageMaker Ground Truth bounding box labeling job task type\. In this context, localization means the pixel\-location of the bounding box\. 

You create a bounding box labeling job using the Ground Truth section of the Amazon SageMaker console or the [ `CreateLabelingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html) operation\.

**Important**  
For this task type, if you create your own manifest file, use `"source-ref"` to identify the location of each image file in Amazon S3 that you want labeled\. For more information, see [Input Data](sms-data-input.md)\.

## Creating a Bounding Box Labeling Job \(Console\)<a name="sms-creating-bounding-box-labeling-job-console"></a>

You can follow the instructions [Create a Labeling Job \(Console\)](sms-create-labeling-job-console.md) to learn how to create a bounding box labeling job in the SageMaker console\. In Step 10, choose **Image** from the **Task category** drop down menu, and choose **Bounding box** as the task type\. 

Ground Truth provides a worker UI similar to the following for labeling tasks\. When you create the labeling job with the console, you specify instructions to help workers complete the job and labels that workers can choose from\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/box-bounding-example.png)

## Create a Bounding Box Labeling Job \(API\)<a name="sms-creating-bounding-box-labeling-job-api"></a>

To create a bounding box labeling job, use the SageMaker API operation `CreateLabelingJob`\. This API defines this operation for all AWS SDKs\. To see a list of language\-specific SDKs supported for this operation, review the **See Also** section of [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html)\.

Follow the instructions on [Create a Labeling Job \(API\)](sms-create-labeling-job-api.md) and do the following while you configure your request: 
+ Pre\-annotation Lambda functions for this task type end with `PRE-BoundingBox`\. To find the pre\-annotation Lambda ARN for your Region, see [PreHumanTaskLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/dg/API_HumanTaskConfig.html#SageMaker-Type-HumanTaskConfig-PreHumanTaskLambdaArn) \. 
+ Annotation\-consolidation Lambda functions for this task type end with `ACS-BoundingBox`\. To find the annotation\-consolidation Lambda ARN for your Region, see [AnnotationConsolidationLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/dg/API_AnnotationConsolidationConfig.html#SageMaker-Type-AnnotationConsolidationConfig-AnnotationConsolidationLambdaArn)\. 

The following is an example of an [AWS Python SDK \(Boto3\) request](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_labeling_job) to create a labeling job in the US East \(N\. Virginia\) Region\. All parameters in red should be replaced with your specifications and resources\. 

```
response = client.create_labeling_job(
    LabelingJobName='example-bounding-box-labeling-job,
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
        'PreHumanTaskLambdaArn': 'arn:aws:lambda:us-east-1:432418664414:function:PRE-BoundingBox',
        'TaskKeywords': [
            'Bounding Box',
        ],
        'TaskTitle': 'Bounding Box task',
        'TaskDescription': 'Draw bounding boxes around objects in an image',
        'NumberOfHumanWorkersPerDataObject': 123,
        'TaskTimeLimitInSeconds': 123,
        'TaskAvailabilityLifetimeInSeconds': 123,
        'MaxConcurrentTaskCount': 123,
        'AnnotationConsolidationConfig': {
            'AnnotationConsolidationLambdaArn': 'arn:aws:lambda:us-east-1:432418664414:function:ACS-BoundingBox'
        },
    Tags=[
        {
            'Key': 'string',
            'Value': 'string'
        },
    ]
)
```

### Provide a Template for Bounding Box Labeling Jobs<a name="sms-create-labeling-job-bounding-box-api-template"></a>

If you create a labeling job using the API, you must supply a worker task template in `UiTemplateS3Uri`\. Copy and modify the following template\. Only modify the [https://docs.aws.amazon.com/sagemaker/latest/dg/sms-creating-instruction-pages.html#sms-creating-quick-instructions](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-creating-instruction-pages.html#sms-creating-quick-instructions), [https://docs.aws.amazon.com/sagemaker/latest/dg/sms-creating-instruction-pages.html#sms-creating-full-instructions](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-creating-instruction-pages.html#sms-creating-full-instructions), and `header`\. Upload this template to S3, and provide the S3 URI for this file in `UiTemplateS3Uri`\.

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>
<crowd-form>
  <crowd-bounding-box
    name="boundingBox"
    src="{{ task.input.taskObject | grant_read_access }}"
    header="please draw box"
    labels="{{ task.input.labels | to_json | escape }}"
  >

    <full-instructions header="Bounding box instructions">
      <ol><li><strong>Inspect</strong> the image</li><li><strong>Determine</strong> 
      if the specified label is/are visible in the picture.</li>
      <li><strong>Outline</strong> each instance of the specified label in the image using the provided “Box” tool.</li></ol>
      <ul><li>Boxes should fit tight around each object</li>
      <li>Do not include parts of the object are overlapping or that cannot be seen, even though you think you can interpolate the whole shape.</li>
      <li>Avoid including shadows.</li>
      <li>If the target is off screen, draw the box up to the edge of the image.</li>    
    </full-instructions>
  
    <short-instructions>
      <h3><span style="color: rgb(0, 138, 0);">Good example</span></h3>
      <p>Enter description of a correct bounding box label and add images</p>
      <h3><span style="color: rgb(230, 0, 0);">Bad example</span></h3>
      <p>Enter description of an incorrect bounding box label and add images</p>
    </short-instructions>
  
  </crowd-bounding-box>
</crowd-form>
```

## Bounding Box Output Data<a name="sms-bounding-box-output-data"></a>

Once you have created a bounding box labeling job, your output data will be located in the Amazon S3 bucket specified in the `S3OutputPath` parameter when using the API or in the **Output dataset location** field of the **Job overview** section of the console\. 

For example, the output manifest file of a successfully completed single\-class bounding box task will contain the following: 

```
[
  {
    "boundingBox": {
      "boundingBoxes": [
        {
          "height": 2833,
          "label": "bird",
          "left": 681,
          "top": 599,
          "width": 1364
        }
      ],
      "inputImageProperties": {
        "height": 3726,
        "width": 2662
      }
    }
  }
]
```

The `boundingBoxes` parameter identifies the location of the bounding box drawn around an object identified as a "bird" relative to the top\-left corner of the image which is taken to be the \(0,0\) pixel\-coordinate\. In the previous example, **`left`** and **`top`** identify the location of the pixel in the top\-left corner of the bounding box relative to the top\-left corner of the image\. The dimensions of the bounding box are identified with **`height`** and **`width`**\. The `inputImageProperties` parameter gives the pixel\-dimensions of the original input image\.

When you use the bounding box task type, you can create single\- and multi\-class bounding box labeling jobs\. The output manifest file of a successfully completed multi\-class bounding box will contain the following: 

```
[
  {
    "boundingBox": {
      "boundingBoxes": [
        {
          "height": 938,
          "label": "squirrel",
          "left": 316,
          "top": 218,
          "width": 785
        },
        {
          "height": 825,
          "label": "rabbit",
          "left": 1930,
          "top": 2265,
          "width": 540
        },
        {
          "height": 1174,
          "label": "bird",
          "left": 748,
          "top": 2113,
          "width": 927
        },
        {
          "height": 893,
          "label": "bird",
          "left": 1333,
          "top": 847,
          "width": 736
        }
      ],
      "inputImageProperties": {
        "height": 3726,
        "width": 2662
      }
    }
  }
]
```

To learn more about the output manifest file that results from a bounding box labeling job, see [Bounding Box Job Output](sms-data-output.md#sms-output-box)\.

To learn more about the output manifest file generated by Ground Truth and the file structure the Ground Truth uses to store your output data, see [Output Data](sms-data-output.md)\. 