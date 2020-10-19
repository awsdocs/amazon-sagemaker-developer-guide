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
+ [Workforces](#sms-video-workforces)
+ [Worker User Interface \(UI\)](#sms-video-worker-task-ui)

## Input Data<a name="sms-video-input-overview"></a>

The video frame labeling job uses *sequences* of video frames\. A single sequence is a series of images that have been extracted from a single video\. You can either provide your own sequences of video frames, or have Ground Truth automatically extract video frame sequences from your video files\. To learn more, see [Provide Video Files](sms-point-cloud-video-input-data.md#sms-point-cloud-video-frame-extraction)\.

Ground Truth uses sequence files to identify all images in a single sequence\. All of the sequences that you want to include in a single labeling job are identitified in an input manifest file\. Each sequence is used to create a single worker task\. You can automatically create sequence files and an input manifest file using Ground Truth automatic data setup\. To learn more, see [Automated Video Frame Input Data Setup](sms-video-automated-data-setup.md)\. 

To learn how to manually create sequence files and an input manifest file, see [Create a Video Frame Input Manifest File](sms-video-manual-data-setup.md#sms-video-create-manifest)\. 

## Job Completion Times<a name="sms-video-job-completion-times"></a>

Video and video frame labeling jobs can take workers hours to complete\. You can set the total amount of time that workers can work on each task when you create a labeling job\. The maximum time you can set for workers to work on tasks is 7 days\. The default value is 3 days\. 

We strongly recommend that you create tasks that workers can complete within 12 hours\. Workers must keep the worker UI open while working on a task\. They can save work as they go and Ground Truth saves their work every 15 minutes\.

When using the SageMaker `CreateLabelingJob` API operation, set the total time a task is available to workers in the `TaskTimeLimitInSeconds` parameter of `HumanTaskConfig`\. 

When you create a labeling job in the console, you can specify this time limit when you select your workforce type and your work team\.

**Important**  
If you set your task time limit to be greater than 8 hours, you must set `MaxSessionDuration` for your IAM execution role to at least 8 hours\. 

## Workforces<a name="sms-video-workforces"></a>

When you create a video frame labeling job, you need to specify a work team to complete your annotation tasks\. You can choose a work team from a private workforce of your own workers, or from a vendor workforce that you select in the AWS Marketplace\. You cannot use the Amazon Mechanical Turk workforce for video frame labeling jobs\. 

To learn more about vendor workforces, see [Managing Vendor Workforces](sms-workforce-management-vendor.md)\.

To learn how to create and manage a private workforce, see [Use a Private Workforce](sms-workforce-private.md)\.

## Worker User Interface \(UI\)<a name="sms-video-worker-task-ui"></a>

Ground Truth provides a worker user interface \(UI\), tools, and assistive labeling features to help workers complete your video labeling tasks\. 

You can preview the worker UI when you create a labeling job in the console\.

When you create a labeling job using the API operation `CreateLabelingJob`, you must provide an ARN provided by Ground Truth in the parameter [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UiConfig.html#sagemaker-Type-UiConfig-UiTemplateS3Uri](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UiConfig.html#sagemaker-Type-UiConfig-UiTemplateS3Uri) to specify the worker UI for your task type\. You can use `HumanTaskUiArn` with the SageMaker [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_RenderUiTemplate.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_RenderUiTemplate.html) API operation to preview the worker UI\. 

You provide worker instructions, labels, and optionally, attributes that workers can use to provide more information about labels \(label category attributes\)\. They are all displayed in the worker UI\.

### Label Category Attributes<a name="sms-video-label-attributes"></a>

When you create a video object tracking or video object detection labeling job, you can add one or more *label category attributes* to each label category that you specify\. Use label category attributes to enable workers to provide more information about the objects they annotate\. 

For example, you may create the label category *car* because you want workers to identify cars in your video frames\. You might also want to capture additional data about your labeled cars, such as if they are occluded or the size of the car\. You can capture this metadata using label category attributes\. 

For each attribute you assign to a label, you can add multiple options that workers select from\. Workers can select a single value from those options\. In the previous example, if you added the attribute *occluded* to the car label category, you might assign *partial*, *completely*, *no* to the *occluded* attribute and enable workers to select one of these options\.

To learn how to add label category attributes, use the **Create Labeling Job** section on the [task type page](sms-video-task-types.md) of your choice\.

### Task Types<a name="sms-video-frame-tools"></a>

When you create a video object tracking or video object detection labeling job, you specify the type of annotation that you want workers to create while working on your labeling task\. The annotation type determines the type of output data Ground Truth returns and defines the *task type* for your labeling job\. 

If you are creating a labeling job using the API operation [CreateLabelingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html), you specify the task type using the label category configuration file parameter `annotationType`\. To learn more, see [Create a Labeling Category Configuration File with Label Category Attributes](sms-label-cat-config-attributes.md)\.

The following task types are available for both video object tracking or video object detection labeling jobs: 
+ **Bounding box ** – Workers are provided with tools to create bounding box annotations\. A bounding box is a box that a worker draws around an objects to identify the pixel\-location and label of that object in the frame\. 
+ **Polyline** – Workers are provided with tools to create polyline annotations\. A polyline is defined by the series of ordered x, y coordinates\. Each point added to the polyline is connected to the previous point by a line\. The polyline does not have to be closed \(the start point and end point do not have to be the same\) and there are no restrictions on the angles formed between lines\. 
+ **Polygon ** – Workers are provided with tools to create polygon annotations\. A polygon is a closed shape defined by a series of ordered x, y coordinates\. Each point added to the polygon is connected to the previous point by a line and there are no restrictions on the angles formed between lines\. Two lines \(sides\) of the polygon cannot cross\. The start and end point of a polygon must be the same\. 
+ **Keypoint** – Workers are provided with tools to create keypoint annotations\. A keypoint is a single point associated with an x, y coordinate in the video frame\.

### Worker Instructions<a name="sms-video-worker-instructions-general"></a>

You can provide worker instructions to help your workers complete your video frame labeling tasks\. You might want to cover the following topics when writing your instructions: 
+ Best practices and things to avoid when annotating objects\.
+ The label category attributes provided \(for object detection and object tracking tasks\) and how to use them\.
+ How to save time while labeling by using keyboard shortcuts\. 

You can add your worker instructions using the SageMaker console while creating a labeling job\. If you create a labeling job using the API operation `CreateLabelingJob`, you specify worker instructions in your label category configuration file\. 

In addition to your instructions, Ground Truth provides a link to help workers navigate and use the worker portal\. View these instructions by selecting the task type on [Worker Instructions](sms-video-worker-instructions.md)\.