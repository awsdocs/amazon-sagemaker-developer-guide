# Video Frame Object Detection<a name="sms-video-object-detection"></a>

You can use the video frame object detection task type to have workers identify and locate objects in a sequence of video frames \(images extracted from a video\) using bounding boxes, polylines, polygons or keypoint *annotation tools*\. The tool you choose defines the video frame task type you create\. For example, you can use a bounding box video frame object detection task type workers to identify and localize various objects in a series of video frames, such as cars, bikes, and pedestrians\.

You can create a video frame object detection labeling job using the Amazon SageMaker Ground Truth console, the SageMaker API, and language\-specific AWS SDKs\. To learn more, see [Create a Video Frame Object Detection Labeling Job](#sms-video-od-create-labeling-job) and select your preferred method\. See [Task Types](sms-video-overview.md#sms-video-frame-tools) to learn more about the annotations tools you can choose from when you create a labeling job\.

Ground Truth provides a worker UI and tools to complete your labeling job tasks: [Preview the Worker UI](#sms-video-od-worker-ui)\.

You can create a job to adjust annotations created in a video object detection labeling job using the video object detection adjustment task type\. To learn more, see [Create Video Frame Object Detection Adjustment or Verification Labeling Job](#sms-video-od-adjustment)\.

## Preview the Worker UI<a name="sms-video-od-worker-ui"></a>

Ground Truth provides workers with a web user interface \(UI\) to complete your video frame object detection annotation tasks\. You can preview and interact with the worker UI when you create a labeling job in the console\. If you are a new user, we recommend that you create a labeling job through the console using a small input dataset to preview the worker UI and ensure your video frames, labels, and label attributes appear as expected\. 

The UI provides workers with the following assistive labeling tools to complete your object detection tasks:
+ For all tasks, workers can use the **Copy to next** and **Copy to all** features to copy an annotation to the next frame or to all subsequent frames respectively\. 
+ For tasks that include the bounding box tools, workers can use a **Predict next** feature to draw a bounding box in a single frame, and then have Ground Truth predict the location of boxes with the same label in all other frames\. Workers can then make adjustments to correct predicted box locations\. 

The following video shows how a worker might use the worker UI with the bounding box tool to complete your object detection tasks\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/kitti-od-general-labeling-job.gif)

## Create a Video Frame Object Detection Labeling Job<a name="sms-video-od-create-labeling-job"></a>

You can create a video frame object detection labeling job using the SageMaker console or the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html) API operation\. 

This section assumes that you have reviewed the [Video Frame Labeling Job Overview](sms-video-overview.md) and have chosen the type of input data and the input dataset connection you are using\. 

### Create a Labeling Job \(Console\)<a name="sms-video-od-create-labeling-job-console"></a>

You can follow the instructions in [Create a Labeling Job \(Console\)](sms-create-labeling-job-console.md) to learn how to create a video frame object tracking job in the SageMaker console\. In step 10, choose **Video \- Object detection** from the **Task category** dropdown list\. Select the task type you want by selecting one of the cards in **Task selection**\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/task-type-vod.gif)

### Create a Labeling Job \(API\)<a name="sms-video-od-create-labeling-job-api"></a>

You create an object detection labeling job using the SageMaker API operation `CreateLabelingJob`\. This API defines this operation for all AWS SDKs\. To see a list of language\-specific SDKs supported for this operation, review the **See Also** section of [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html)\. 

[Create a Labeling Job \(API\)](sms-create-labeling-job-api.md) provides an overview of the `CreateLabelingJob` operation\. Follow these instructions and do the following while you configure your request: 
+ You must enter an ARN for `HumanTaskUiArn`\. Use `arn:aws:sagemaker:<region>:394669845002:human-task-ui/VideoObjectDetection`\. Replace `<region>` with the AWS Region in which you are creating the labeling job\. 

  Do not include an entry for the `UiTemplateS3Uri` parameter\. 
+ Your [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#sagemaker-CreateLabelingJob-request-LabelAttributeName](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#sagemaker-CreateLabelingJob-request-LabelAttributeName) must end in `-ref`\. For example, `video-od-labels-ref`\. 
+ Your input manifest file must be a video frame sequence manifest file\. You can create this manifest file using the SageMaker console, or create it manually and upload it to Amazon S3\. For more information, see [Input Data Setup](sms-video-data-setup.md)\. 
+ You can only use private or vendor work teams to create video frame object detection labeling jobs\. 
+ You specify your labels, label category and frame attributes, the task type, and worker instructions in a label category configuration file\. Specify the task type \(bounding boxes, polylines, polygons or keypoint\) using `annotationType` in your label category configuration file\. For more information, see [Create a Labeling Category Configuration File with Label Category and Frame Attributes](sms-label-cat-config-attributes.md) to learn how to create this file\. 
+ You need to provide pre\-defined ARNs for the pre\-annotation and post\-annotation \(ACS\) Lambda functions\. These ARNs are specific to the AWS Region you use to create your labeling job\. 
  + To find the pre\-annotation Lambda ARN, refer to [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HumanTaskConfig.html#sagemaker-Type-HumanTaskConfig-PreHumanTaskLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HumanTaskConfig.html#sagemaker-Type-HumanTaskConfig-PreHumanTaskLambdaArn)\. Use the Region in which you are creating your labeling job to find the correct ARN that ends with `PRE-VideoObjectDetection`\. 
  + To find the post\-annotation Lambda ARN, refer to [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AnnotationConsolidationConfig.html#sagemaker-Type-AnnotationConsolidationConfig-AnnotationConsolidationLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AnnotationConsolidationConfig.html#sagemaker-Type-AnnotationConsolidationConfig-AnnotationConsolidationLambdaArn)\. Use the Region in which you are creating your labeling job to find the correct ARN that ends with `ACS-VideoObjectDetection`\. 
+ The number of workers specified in `NumberOfHumanWorkersPerDataObject` must be `1`\. 
+ Automated data labeling is not supported for video frame labeling jobs\. Do not specify values for parameters in `[LabelingJobAlgorithmsConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#sagemaker-CreateLabelingJob-request-LabelingJobAlgorithmsConfig)`\. 
+ Video frame object tracking labeling jobs can take multiple hours to complete\. You can specify a longer time limit for these labeling jobs in `TaskTimeLimitInSeconds` \(up to 7 days, or 604,800 seconds\)\. 

The following is an example of an [AWS Python SDK \(Boto3\) request](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_labeling_job) to create a labeling job in the US East \(N\. Virginia\) Region\. 

```
response = client.create_labeling_job(
    LabelingJobName='example-video-od-labeling-job,
    LabelAttributeName='label',
    InputConfig={
        'DataSource': {
            'S3DataSource': {
                'ManifestS3Uri': 's3://DOC-EXAMPLE-BUCKET/path/video-frame-sequence-input-manifest.json'
            }
        },
        'DataAttributes': {
            'ContentClassifiers': [
                'FreeOfPersonallyIdentifiableInformation'|'FreeOfAdultContent',
            ]
        }
    },
    OutputConfig={
        'S3OutputPath': 's3://DOC-EXAMPLE-BUCKET/prefix/file-to-store-output-data',
        'KmsKeyId': 'string'
    },
    RoleArn='arn:aws:iam::*:role/*,
    LabelCategoryConfigS3Uri='s3://bucket/prefix/label-categories.json',
    StoppingConditions={
        'MaxHumanLabeledObjectCount': 123,
        'MaxPercentageOfInputDatasetLabeled': 123
    },
    HumanTaskConfig={
        'WorkteamArn': 'arn:aws:sagemaker:us-east-1:*:workteam/private-crowd/*',
        'UiConfig': {
            'HumanTaskUiArn: 'arn:aws:sagemaker:us-east-1:394669845002:human-task-ui/VideoObjectDetection'
        },
        'PreHumanTaskLambdaArn': 'arn:aws:lambda:us-east-1:432418664414:function:PRE-VideoObjectDetection',
        'TaskKeywords': [
            'Video Frame Object Detection',
        ],
        'TaskTitle': 'Video frame object detection task',
        'TaskDescription': 'Classify and identify the location of objects and people in video frames',
        'NumberOfHumanWorkersPerDataObject': 123,
        'TaskTimeLimitInSeconds': 123,
        'TaskAvailabilityLifetimeInSeconds': 123,
        'MaxConcurrentTaskCount': 123,
        'AnnotationConsolidationConfig': {
            'AnnotationConsolidationLambdaArn': 'arn:aws:lambda:us-east-1:432418664414:function:ACS-VideoObjectDetection'
        },
    Tags=[
        {
            'Key': 'string',
            'Value': 'string'
        },
    ]
)
```

## Create Video Frame Object Detection Adjustment or Verification Labeling Job<a name="sms-video-od-adjustment"></a>

You can create an adjustment and verification labeling job using the Ground Truth console or `CreateLabelingJob` API\. To learn more about adjustment and verification labeling jobs, and to learn how create one, see [Verify and Adjust Labels](sms-verification-data.md)\.

## Output Data Format<a name="sms-video-od-output-data"></a>

When you create a video frame object detection labeling job, tasks are sent to workers\. When these workers complete their tasks, labels are written to the Amazon S3 output location you specified when you created the labeling job\. To learn about the video frame object detection output data format, see [Video Frame Object Detection Output](sms-data-output.md#sms-output-video-object-detection)\. If you are a new user of Ground Truth, see [Output Data](sms-data-output.md) to learn more about the Ground Truth output data format\. 