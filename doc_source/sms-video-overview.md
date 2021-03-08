# Video Frame Labeling Job Overview<a name="sms-video-overview"></a>

Use this page to learn about the object detection and object tracking video frame labeling jobs\. The information on this page applies to both of these built\-in task types\. 

The video frame labeling job is unique because of the following:
+ You can either provide data objects that are ready to be annotated \(video frames\), or you can provide video files and have Ground Truth automatically extract video frames\. 
+ Workers have the ability to save work as they go\. 
+ You cannot use the Amazon Mechanical Turk workforce to complete your labeling tasks\. 
+ Ground Truth provides a worker UI, as well as assistive and basic labeling tools, to help workers complete your tasks\. You do not need to provide a worker task template\. 

Use the following topics to learn more\. 

**Topics**
+ [Input Data](#sms-video-input-overview)
+ [Job Completion Times](#sms-video-job-completion-times)
+ [Task Types](#sms-video-frame-tools)
+ [Workforces](#sms-video-workforces)
+ [Worker User Interface \(UI\)](#sms-video-worker-task-ui)
+ [Video Frame Job Permission Requirements](#sms-security-permission-video-frame)

## Input Data<a name="sms-video-input-overview"></a>

The video frame labeling job uses *sequences* of video frames\. A single sequence is a series of images that have been extracted from a single video\. You can either provide your own sequences of video frames, or have Ground Truth automatically extract video frame sequences from your video files\. To learn more, see [Provide Video Files](sms-point-cloud-video-input-data.md#sms-point-cloud-video-frame-extraction)\.

Ground Truth uses sequence files to identify all images in a single sequence\. All of the sequences that you want to include in a single labeling job are identified in an input manifest file\. Each sequence is used to create a single worker task\. You can automatically create sequence files and an input manifest file using Ground Truth automatic data setup\. To learn more, see [Automated Video Frame Input Data Setup](sms-video-automated-data-setup.md)\. 

To learn how to manually create sequence files and an input manifest file, see [Create a Video Frame Input Manifest File](sms-video-manual-data-setup.md#sms-video-create-manifest)\. 

## Job Completion Times<a name="sms-video-job-completion-times"></a>

Video and video frame labeling jobs can take workers hours to complete\. You can set the total amount of time that workers can work on each task when you create a labeling job\. The maximum time you can set for workers to work on tasks is 7 days\. The default value is 3 days\. 

We strongly recommend that you create tasks that workers can complete within 12 hours\. Workers must keep the worker UI open while working on a task\. They can save work as they go and Ground Truth saves their work every 15 minutes\.

When using the SageMaker `CreateLabelingJob` API operation, set the total time a task is available to workers in the `TaskTimeLimitInSeconds` parameter of `HumanTaskConfig`\.

When you create a labeling job in the console, you can specify this time limit when you select your workforce type and your work team\.

## Task Types<a name="sms-video-frame-tools"></a>

When you create a video object tracking or video object detection labeling job, you specify the type of annotation that you want workers to create while working on your labeling task\. The annotation type determines the type of output data Ground Truth returns and defines the *task type* for your labeling job\. 

If you are creating a labeling job using the API operation [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html), you specify the task type using the label category configuration file parameter `annotationType`\. To learn more, see [Create a Labeling Category Configuration File with Label Category and Frame Attributes](sms-label-cat-config-attributes.md)\.

The following task types are available for both video object tracking or video object detection labeling jobs: 
+ **Bounding box ** – Workers are provided with tools to create bounding box annotations\. A bounding box is a box that a worker draws around an objects to identify the pixel\-location and label of that object in the frame\. 
+ **Polyline** – Workers are provided with tools to create polyline annotations\. A polyline is defined by the series of ordered x, y coordinates\. Each point added to the polyline is connected to the previous point by a line\. The polyline does not have to be closed \(the start point and end point do not have to be the same\) and there are no restrictions on the angles formed between lines\. 
+ **Polygon ** – Workers are provided with tools to create polygon annotations\. A polygon is a closed shape defined by a series of ordered x, y coordinates\. Each point added to the polygon is connected to the previous point by a line and there are no restrictions on the angles formed between lines\. Two lines \(sides\) of the polygon cannot cross\. The start and end point of a polygon must be the same\. 
+ **Keypoint** – Workers are provided with tools to create keypoint annotations\. A keypoint is a single point associated with an x, y coordinate in the video frame\.

## Workforces<a name="sms-video-workforces"></a>

When you create a video frame labeling job, you need to specify a work team to complete your annotation tasks\. You can choose a work team from a private workforce of your own workers, or from a vendor workforce that you select in the AWS Marketplace\. You cannot use the Amazon Mechanical Turk workforce for video frame labeling jobs\. 

To learn more about vendor workforces, see [Managing Vendor Workforces](sms-workforce-management-vendor.md)\.

To learn how to create and manage a private workforce, see [Use a Private Workforce](sms-workforce-private.md)\.

## Worker User Interface \(UI\)<a name="sms-video-worker-task-ui"></a>

Ground Truth provides a worker user interface \(UI\), tools, and assistive labeling features to help workers complete your video labeling tasks\. You can preview the worker UI when you create a labeling job in the console\.

When you create a labeling job using the API operation `CreateLabelingJob`, you must provide an ARN provided by Ground Truth in the parameter [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UiConfig.html#sagemaker-Type-UiConfig-UiTemplateS3Uri](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UiConfig.html#sagemaker-Type-UiConfig-UiTemplateS3Uri) to specify the worker UI for your task type\. You can use `HumanTaskUiArn` with the SageMaker [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_RenderUiTemplate.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_RenderUiTemplate.html) API operation to preview the worker UI\. 

You provide worker instructions, labels, and optionally, attributes that workers can use to provide more information about labels and video frames\. These attributes are referred to as label category attributes and frame attributes respectively\. They are all displayed in the worker UI\.

### Label Category and Frame Attributes<a name="sms-video-label-attributes"></a>

When you create a video object tracking or video object detection labeling job, you can add one or more *label category attributes* and *frame attributes*:
+ **Label category attribute** – A list of options \(strings\), a free form text box, or a numeric field associated with one or more labels\. It is used by workers to provide metadata about a label\. 
+ **Frame attribute** – A list of options \(strings\), a free form text box, or a numeric field that appears on each video frame a worker is sent to annotate\. It is used by workers to provide metadata about video frames\. 

Additionally, you can use label and frame attributes to have workers verify labels in a video frame label verification job\. 

Use the following sections to learn more about these attributes\. To learn how to add label category and frame attributes to a labeling job, use the **Create Labeling Job** sections on the [task type page](sms-video-task-types.md) of your choice\.

#### Label Category Attributes<a name="sms-video-label-attributes"></a>

Add label category attributes to labels to give workers the ability to provide more information about the annotations they create\. A label category attribute is added to an individual label, or to all labels\. When a label category attribute is applied to all labels it is referred to as a *global label category attribute*\. 

For example, if you add the label category *car*, you might also want to capture additional data about your labeled cars, such as if they are occluded or the size of the car\. You can capture this metadata using label category attributes\. In this example, if you added the attribute *occluded* to the car label category, you can assign *partial*, *completely*, *no* to the *occluded* attribute and enable workers to select one of these options\. 

When you create a label verification job, you add labels category attributes to each label you want workers to verify\.

#### Frame level Attributes<a name="sms-video-frame-attributes"></a>

Add frame attributes to give workers the ability to provide more information about individual video frames\. Each frame attribute you add appears on all frames\. 

For example, you can add a number\-frame attribute to have workers identify the number of objects they see in a particular frame\. 

In another example, you may want to provide a free\-form text box to give workers the ability to provide an answer to a question\. 

When you create a label verification job, you can add one or more frame attributes to ask workers to provide feedback on all labels in a video frame\.

### Worker Instructions<a name="sms-video-worker-instructions-general"></a>

You can provide worker instructions to help your workers complete your video frame labeling tasks\. You might want to cover the following topics when writing your instructions: 
+ Best practices and things to avoid when annotating objects\.
+ The label category attributes provided \(for object detection and object tracking tasks\) and how to use them\.
+ How to save time while labeling by using keyboard shortcuts\. 

You can add your worker instructions using the SageMaker console while creating a labeling job\. If you create a labeling job using the API operation `CreateLabelingJob`, you specify worker instructions in your label category configuration file\. 

In addition to your instructions, Ground Truth provides a link to help workers navigate and use the worker portal\. View these instructions by selecting the task type on [Worker Instructions](sms-video-worker-instructions.md)\.

### Declining Tasks<a name="sms-decline-task-video"></a>

Workers are able to decline tasks\. 

Workers decline a task if the instructions are not clear, input data is not displaying correctly, or if they encounter some other issue with the task\. If the number of workers per dataset object \([https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HumanTaskConfig.html#sagemaker-Type-HumanTaskConfig-NumberOfHumanWorkersPerDataObject](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HumanTaskConfig.html#sagemaker-Type-HumanTaskConfig-NumberOfHumanWorkersPerDataObject)\) decline the task, the data object is marked as expired and will not be sent to additional workers\.

## Video Frame Job Permission Requirements<a name="sms-security-permission-video-frame"></a>

When you create a video frame labeling job, in addition to the permission requirements found in [Assign IAM Permissions to Use Ground Truth](sms-security-permission.md), you must add a CORS policy to your S3 bucket that contains your input manifest file\. 

### Add a CORS Permission Policy to S3 Bucket<a name="sms-permissions-add-cors-video-frame"></a>

When you create a video frame labeling job, you specify buckets in S3 where your input data and manifest file are located and where your output data will be stored\. These buckets may be the same\. You must attach the following Cross\-origin resource sharing \(CORS\) policy to your input and output buckets\. If you use the Amazon S3 console to add the policy to your bucket, you must use the JSON format\.

**JSON**

```
[
    {
        "AllowedHeaders": [
            "*"
        ],
        "AllowedMethods": [
            "GET",
            "HEAD",
            "PUT"
        ],
        "AllowedOrigins": [
            "*"
        ],
        "ExposeHeaders": [
            "Access-Control-Allow-Origin"
        ],
        "MaxAgeSeconds": 3000
    }
]
```

**XML**

```
<?xml version="1.0" encoding="UTF-8"?>
<CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
<CORSRule>
    <AllowedOrigin>*</AllowedOrigin>
    <AllowedMethod>GET</AllowedMethod>
    <AllowedMethod>HEAD</AllowedMethod>
    <AllowedMethod>PUT</AllowedMethod>
    <MaxAgeSeconds>3000</MaxAgeSeconds>
    <ExposeHeader>Access-Control-Allow-Origin</ExposeHeader>
    <AllowedHeader>*</AllowedHeader>
</CORSRule>
</CORSConfiguration>
```

To learn how to add a CORS policy to an S3 bucket, see [ How do I add cross\-domain resource sharing with CORS?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/add-cors-configuration.html) in the Amazon Simple Storage Service Console User Guide\.