# Video Frame Object Tracking<a name="sms-video-object-tracking"></a>

You can use the video frame object tracking task type to have workers track the movement of objects in a sequence of video frames \(images extracted from a video\) using bounding boxes\. For example, you can use this task to ask workers to track the movement of objects, such as cars, bikes, and pedestrians\.

A *bounding box* is a box that is associated with a label and is used to identify the pixel location of the object\. You provide a list of categories, and each bounding box that a worker adds to a video frame is identified as an *instance* of that category using an instance ID\. For example, if you provide the label category car, the first car that a worker annotates will have the instance ID car:1\. The second car the worker annotates will have the instance ID car:2\. To track an object's movement, the worker adds bounding boxes associated with the same instance ID around that object in all frames\. 

You can create a video frame object tracking labeling job using the Amazon SageMaker Ground Truth console, the SageMaker API, and language\-specific AWS SDKs\. To learn more, see [Create a Video Frame Object Detection Labeling Job](sms-video-object-detection.md#sms-video-od-create-labeling-job) and select your preferred method\. 

Ground Truth provides a worker UI and tools to complete your labeling job tasks: [Preview the Worker UI](sms-video-object-detection.md#sms-video-od-worker-ui)\.

You can create a job to adjust annotations created in a video object detection labeling job using the video object detection adjustment task type\. To learn more, see [Create an Adjustment Labeling Job](sms-video-object-detection.md#sms-video-od-adjustment)\.

## Preview the Worker UI<a name="sms-video-ot-worker-ui"></a>

Ground Truth provides workers with a web user interface \(UI\) to complete your video frame object tracking annotation tasks\. You can preview and interact with the worker UI when you create a labeling job in the console\. If you are a new user, we recommend that you create a labeling job through the console using a small input dataset to preview the worker UI and ensure your video frames, labels, and label attributes appear as expected\. 

The UI provides workers with labeling tools to complete your object detection tasks\. Workers are able to use a **Predict next** feature to draw a bounding box in a single frame, and then have Ground Truth predict the location of boxes with the same ID in all other frames\. Workers can then make adjustments to correct the predicted box locations\.

The following video shows how a worker might use the worker UI and tools to complete your object tracking tasks\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/video/ot_predict_next.gif)

## Create a Video Frame Object Tracking Labeling Job<a name="sms-video-ot-create-labeling-job"></a>

You can create a video frame object tracking labeling job using the SageMaker console or the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html) API operation\. 

This section assumes that you have reviewed the [Video Frame Labeling Job Overview](sms-video-overview.md) and have chosen the type of input data and the input dataset connection you are using\. 

### Create a Labeling Job \(Console\)<a name="sms-video-ot-create-labeling-job-console"></a>

You can follow the instructions in [Create a Labeling Job \(Console\)](sms-create-labeling-job-console.md) to learn how to create a video frame object tracking job in the SageMaker console\. In step 10, choose **Video** from the **Task category** dropdown list, and choose **Video frame object tracking** as the task type\. 

### Create a Labeling Job \(API\)<a name="sms-video-ot-create-labeling-job-api"></a>

You create an object tracking labeling job using the SageMaker API operation `CreateLabelingJob`\. This API defines this operation for all AWS SDKs\. To see a list of language\-specific SDKs supported for this operation, review the **See Also** section of [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html)\. 

[Create a Labeling Job \(API\)](sms-create-labeling-job-api.md) provides an overview of the `CreateLabelingJob` operation\. Follow these instructions and do the following while you configure your request: 
+ You must enter an ARN for `HumanTaskUiArn`\. Use `arn:aws:sagemaker:<region>:394669845002:human-task-ui/VideoObjectTracking`\. Replace `<region>` with the AWS Region in which you are creating the labeling job\. 

  Do not include an entry for the `UiTemplateS3Uri` parameter\. 
+ Your [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#sagemaker-CreateLabelingJob-request-LabelAttributeName](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#sagemaker-CreateLabelingJob-request-LabelAttributeName) must end in `-ref`\. For example, `ot-labels-ref`\. 
+ Your input manifest file must be a video frame sequence manifest file\. You can create this manifest file using the SageMaker console, or create it manually and upload it to Amazon S3\. For more information, see [Input Data Setup](sms-video-data-setup.md)\. 
+ You can only use private or vendor work teams to create video frame object detection labeling jobs\. 
+ You specify your labels and worker instructions in a label category configuration file\. For more information, see [Create a Labeling Category Configuration File with Label Category Attributes](sms-label-cat-config-attributes.md) to learn how to create this file\. 
+ You need to provide pre\-defined ARNs for the pre\-annotation and post\-annotation \(ACS\) Lambda functions\. These ARNs are specific to the AWS Region you use to create your labeling job\. 
  + To find the pre\-annotation Lambda ARN, refer to [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HumanTaskConfig.html#sagemaker-Type-HumanTaskConfig-PreHumanTaskLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HumanTaskConfig.html#sagemaker-Type-HumanTaskConfig-PreHumanTaskLambdaArn)\. Use the Region in which you are creating your labeling job to find the correct ARN that ends with `PRE-VideoObjectTracking`\. 
  + To find the post\-annotation Lambda ARN, refer to [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AnnotationConsolidationConfig.html#sagemaker-Type-AnnotationConsolidationConfig-AnnotationConsolidationLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AnnotationConsolidationConfig.html#sagemaker-Type-AnnotationConsolidationConfig-AnnotationConsolidationLambdaArn)\. Use the Region in which you are creating your labeling job to find the correct ARN that ends with `ACS-VideoObjectTracking`\. 
+ The number of workers specified in `NumberOfHumanWorkersPerDataObject` must be `1`\. 
+ Automated data labeling is not supported for 3D point cloud labeling jobs\. Do not specify values for parameters in `[LabelingJobAlgorithmsConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#sagemaker-CreateLabelingJob-request-LabelingJobAlgorithmsConfig)`\. 
+ 3D point cloud object tracking labeling jobs can take multiple hours to complete\. You can specify a longer time limit for these labeling jobs in `TaskTimeLimitInSeconds` \(up to 7 days, or 604,800 seconds\)\. 
**Important**  
If you set your take time limit to be greater than 8 hours, you must set `MaxSessionDuration` for your IAM execution role to at least 8 hours\. To see how to update this value for your IAM role, see [Modifying a Role ](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_modify.html) in the IAM User Guide, choose your preferred method to modify the role, and then follow the steps in [Modifying a Role Maximum Session Duration](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-managingrole-editing-console.html#roles-modify_max-session-duration)\. 

The following is an example of an [AWS Python SDK \(Boto3\) request](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_labeling_job) to create a labeling job in the US East \(N\. Virginia\) Region\. 

```
response = client.create_labeling_job(
    LabelingJobName='example-video-ot-labeling-job,
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
            'HumanTaskUiArn: 'arn:aws:sagemaker:us-east-1:394669845002:human-task-ui/VideoObjectTracking'
        },
        'PreHumanTaskLambdaArn': 'arn:aws:lambda:us-east-1:432418664414:function:PRE-VideoObjectTracking',
        'TaskKeywords': [
            'Video Frame Object Tracking,
        ],
        'TaskTitle': 'Video frame object tracking task',
        'TaskDescription': Tracking the location of objects and people across video frames',
        'NumberOfHumanWorkersPerDataObject': 123,
        'TaskTimeLimitInSeconds': 123,
        'TaskAvailabilityLifetimeInSeconds': 123,
        'MaxConcurrentTaskCount': 123,
        'AnnotationConsolidationConfig': {
            'AnnotationConsolidationLambdaArn': 'arn:aws:lambda:us-east-1:432418664414:function:ACS-VideoObjectTracking'
        },
    Tags=[
        {
            'Key': 'string',
            'Value': 'string'
        },
    ]
)
```

## Create an Adjustment Labeling Job<a name="sms-video-ot-adjustment"></a>

Use the video frame object tracking adjustment task type to have workers verify and adjust labels from a video frame object tracking labeling job\. You can create an adjustment labeling job in the console by *chaining* a successfully completed object tracking labeling job\. To learn more, see [Start an Label Adjustment Job \(Console\)](sms-verification-data.md#sms-data-adjust-start-console)\.

## Output Data Format<a name="sms-video-ot-output-data"></a>

When you create a video frame object tracking labeling job, tasks are sent to workers\. When these workers complete their tasks, labels are written to the Amazon S3 output location you specified when you created the labeling job\. To learn about the video frame object tracking output data format, see [Video Frame Object Tracking Output](sms-data-output.md#sms-output-video-object-tracking)\. If you are a new user of Ground Truth, see [Output Data](sms-data-output.md) to learn more about the Ground Truth output data format\. 