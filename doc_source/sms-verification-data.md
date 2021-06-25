# Verify and Adjust Labels<a name="sms-verification-data"></a>

When the labels on a dataset need to be validated, Amazon SageMaker Ground Truth provides functionality to have workers verify that labels are correct or to adjust previous labels\. 

These types of jobs fall into two distinct categories:
+ *Label verification *— Workers indicate if the existing labels are correct, or rate their quality, and can add comments to explain their reasoning\. Workers will not be able to modify or adjust labels\. 

  If you create a 3D point cloud or video frame label adjustment or verification job, you can choose to make label category attributes \(not supported for 3D point cloud semantic segmentation\) and frame attributes editable by workers\. 
+ *Label adjustment *— Workers adjust prior annotations and, if applicable, label category and frame attributes to correct them\. 

The following Ground Truth [built\-in task types](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-task-types.html) support adjustment and verification labeling jobs:
+ Bounding box
+ Semantic segmentation 
+ 3D point cloud object detection, 3D point cloud object tracking, and 3D point cloud semantic segmentation
+ All video frame object detection and video frame object tracking task types — bounding box, polyline, polygon and keypoint

**Tip**  
For 3D point cloud and video frame labeling verification jobs, it is recommended that you add new label category attributes or frame attributes to the labeling job\. Workers can use these attribute to verify individual labels or the entire frame\. To learn more about label category and frame attributes, see [Worker User Interface \(UI\)](sms-point-cloud-general-information.md#sms-point-cloud-worker-task-ui) for 3D point cloud and [Worker User Interface \(UI\)](sms-video-overview.md#sms-video-worker-task-ui) for video frame\. 

You can start a label verification and adjustment jobs using the SageMaker console or the API\. 

**Topics**
+ [Requirements to Create Verification and Adjustment Labeling Jobs](#sms-data-verify-adjust-prereq)
+ [Create a Label Verification Job \(Console\)](#sms-data-verify-start-console)
+ [Create a Label Adjustment Job \(Console\)](#sms-data-adjust-start-console)
+ [Start a Label Verification or Adjustment Job \(API\)](#sms-data-verify-start-api)
+ [Label Verification and Adjustment Data in the Output Manifest](#sms-data-verify-manifest)
+ [Cautions and Considerations](#sms-data-verify-cautions)

## Requirements to Create Verification and Adjustment Labeling Jobs<a name="sms-data-verify-adjust-prereq"></a>

To create a label verification or adjustment job, the following criteria must be satisfied\. 
+ For non streaming labeling jobs: The input manifest file you use must contain the label attribute name \(`LabelAttributeName`\) of the labels that you want adjusted\. When you chain a successfully completed labeling job, the output manifest file is used as the input manifest file for the new, chained job\. To learn more about the format of the output manifest file Ground Truth produces for each task type, see [Output Data](sms-data-output.md)\.

  For streaming labeling jobs: The Amazon SNS message you sent to the Amazon SNS input topic of the adjustment or verification labeling job must contain the label attribute name of the labels you want adjusted or verified\. To see an example of how you can create an adjustment or verification labeling job with streaming lableing jobs, see this [Jupyter Notebook example](https://github.com/aws/amazon-sagemaker-examples/blob/master/ground_truth_labeling_jobs/ground_truth_streaming_labeling_jobs/ground_truth_create_chained_streaming_labeling_job.ipynb) in GitHub\.
+ The task type of the verification or adjustment labeling job must be the same as the task type of the original job unless you are using the [Image Label Verification](sms-label-verification.md) task type to verify bounding box or semantic segmentation image labels\. See the next bullet point for more details about the video frame task type requirements\.
+ For video frame annotation verification and adjustment jobs, you must use the same annotation task type used to create the annotations from the previous labeling job\. For example, if you create a video frame object detection job to have workers draw bounding boxes around objects, and then you create a video object detection adjustment job, you must specify *bounding boxes* as the annotation task type\. To learn more video frame annotation task types, see [Task Types](sms-video-overview.md#sms-video-frame-tools)\.
+ The task type you select for the adjustment or verification labeling job must support an audit workflow\. The following Ground Truth [built\-in task types](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-task-types.html) support adjustment and verification labeling jobs: bounding box, semantic segmentation, 3D point cloud object detection, 3D point cloud object tracking, and 3D point cloud semantic segmentation, and all video frame object detection and video frame object tracking task types — bounding box, polyline, polygon and keypoint\.

## Create a Label Verification Job \(Console\)<a name="sms-data-verify-start-console"></a>

Bounding box and semantic segmentation labeling jobs are created by choosing the **Label verification** task type in the console\. To create a verification job for 3D point cloud and video frame task types, you must choose the same task type as the original labeling job and choose to display existing labels\. Use one of the following sections to create a label verification job for your task type\.

**Topics**
+ [Create an Image Label Verification Job \(Console\)](#sms-data-verify-start-console-bb-ss)
+ [Create a Point Cloud or Video Frame Label Verification Job \(Console\)](#sms-data-verify-start-console-frame)

### Create an Image Label Verification Job \(Console\)<a name="sms-data-verify-start-console-bb-ss"></a>

Use the following procedure to create a bounding box or semantic segmentation verification job using the console\. This procedure assumes that you have already created a bounding box or semantic segmentation labeling job and its status is Complete\. This the labeling job that produces the labels you want verified\.

**To create an image label verification job:**

1. Open the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/) and choose **Labeling jobs**\.

1. Start a new labeling job by [chaining](sms-reusing-data.md) a prior job or start from scratch, specifying an input manifest that contains labeled data objects\.

1. In the **Task type** pane, select **Label verification**\.

1. Choose **Next**\.

1. In the **Workers** section, choose the type of workforce you would like to use\. For more details about your workforce options see [Create and Manage Workforces](sms-workforce-management.md)\.

1. \(Optional\) After you've selected your workforce, specify the **Task timeout** and **Task expiration time**\.

1. In the **Existing\-labels display options** pane, the system shows the available label attribute names in your manifest\. Choose the label attribute name that identifies the labels that you want workers to verify\. Ground Truth tries to detect and populate these values by analyzing the manifest, but you might need to set the correct value\. 

1. Use the instructions areas of the tool designer to provide context about what the previous labelers were asked to do and what the current verifiers need to check\.

   You can add new labels that workers choose from to verify labels\. For example, you can ask workers to verify the image quality, and provide the labels *Clear* and *Blurry*\. Workers will also have the option to add a comment to explain their selection\. 

1. Choose **See preview** to check that the tool is displaying the prior labels correctly and presents the label verification task clearly\.

1. Select **Create**\. This will create and start your labeling job\.

### Create a Point Cloud or Video Frame Label Verification Job \(Console\)<a name="sms-data-verify-start-console-frame"></a>

Use the following procedure to create a 3D point cloud or video frame verification job using the console\. This procedure assumes that you have already created a labeling job using the task type that produces the types of labels you want to be verified and its status is Complete\.

**To create an image label verification job:**

1. Open the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/) and choose **Labeling jobs**\.

1. Start a new labeling job by [chaining](sms-reusing-data.md) a prior job or start from scratch, specifying an input manifest that contains labeled data objects\.

1. In the **Task type** pane, select the same task type as the labeling job that you chained\. For example, if the original labeling job was a video frame object detection keypoint labeling job, select that task type\. 

1. Choose **Next**\.

1. In the **Workers** section, choose the type of workforce you would like to use\. For more details about your workforce options see [Create and Manage Workforces](sms-workforce-management.md)\.

1. \(Optional\) After you've selected your workforce, specify the **Task timeout** and **Task expiration time**\.

1. Toggle on the switch next to **Display existing labels**\.

1. Select **Verification**\.

1. For **Label attribute name**, choose the name from your manifest that corresponds to the labels that you want to display for verification\. You will only see label attribute names for labels that match the task type you selected on the previous screen\. Ground Truth tries to detect and populate these values by analyzing the manifest, but you might need to set the correct value\.

1. Use the instructions areas of the tool designer to provide context about what the previous labelers were asked to do and what the current verifiers need to check\. 

   You cannot modify or add new labels\. You can remove, modify and add new label category attributes or frame attributes\. It is recommended that you add new label category attributes or frame attributes to the labeling job\. Workers can use these attribute to verify individual labels or the entire frame\. 

   By default, preexisting label category attributes and frame attributes will not be editable by workers\. If you want to make a label category or frame attribute editable, select the **Allow workers to edit this attribute** check box for that attribute\.

   To learn more about label category and frame attributes, see [Worker User Interface \(UI\)](sms-point-cloud-general-information.md#sms-point-cloud-worker-task-ui) for 3D point cloud and [Worker User Interface \(UI\)](sms-video-overview.md#sms-video-worker-task-ui) for video frame\. 

1. Choose **See preview** to check that the tool is displaying the prior labels correctly and presents the label verification task clearly\.

1. Select **Create**\. This will create and start your labeling job\.

## Create a Label Adjustment Job \(Console\)<a name="sms-data-adjust-start-console"></a>

Use one of the following sections to create a label verification job for your task type\.

**Topics**
+ [Create an Image Label Adjustment Job \(Console\)](#sms-data-adjust-start-console-bb-ss)
+ [Create a Point Cloud or Video Frame Label Adjustment Job \(Console\)](#sms-data-adjust-start-console-frame)

### Create an Image Label Adjustment Job \(Console\)<a name="sms-data-adjust-start-console-bb-ss"></a>

Use the following procedure to create a bounding box or semantic segmentation adjustment labeling job using the console\. This procedure assumes that you have already created a bounding box or semantic segmentation labeling job and its status is Complete\. This the labeling job that produces the labels you want adjusted\.

**To create an image label adjustment job \(console\)**

1. Open the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/) and choose **Labeling jobs**\.

1. Start a new labeling job by [chaining](sms-reusing-data.md) a prior job or start from scratch, specifying an input manifest that contains labeled data objects\.

1. Choose the same task type as the original labeling job\.

1. Choose **Next**\.

1. In the **Workers** section, choose the type of workforce you would like to use\. For more details about your workforce options see [Create and Manage Workforces](sms-workforce-management.md)\.

1. \(Optional\) After you've selected your workforce, specify the **Task timeout** and **Task expiration time**\.

1. Expand **Existing\-labels display options** by selecting the arrow next to the title\.

1. Check the box next to **I want to display existing labels from the dataset for this job**\.

1. For **Label attribute name**, choose the name from your manifest that corresponds to the labels that you want to display for adjustment\. You will only see label attribute names for labels that match the task type you selected on the previous screen\. Ground Truth tries to detect and populate these values by analyzing the manifest, but you might need to set the correct value\.

1. Use the instructions areas of the tool designer to provide context about what the previous labelers were tasked with doing and what the current verifiers need to check and adjust\.

1. Choose **See preview** to check that the tool shows the prior labels correctly and presents the task clearly\.

1. Select **Create**\. This will create and start your labeling job\.

### Create a Point Cloud or Video Frame Label Adjustment Job \(Console\)<a name="sms-data-adjust-start-console-frame"></a>

Use the following procedure to create a 3D point cloud or video frame adjustment job using the console\. This procedure assumes that you have already created a labeling job using the task type that produces the types of labels you want to be verified and its status is Complete\.

**To create a 3D point cloud or video frame label adjustment job \(console\)**

1. Open the SageMaker console: [https://console.aws.amazon.com/sagemaker/](https://console.aws.amazon.com/sagemaker/) and choose **Labeling jobs**\.

1. Start a new labeling job by [chaining](sms-reusing-data.md) a prior job or start from scratch, specifying an input manifest that contains labeled data objects\.

1. Choose the same task type as the original labeling job\.

1. Toggle on the switch next to **Display existing labels**\.

1. Select **Adjustment**\.

1. For **Label attribute name**, choose the name from your manifest that corresponds to the labels that you want to display for adjustment\. You will only see label attribute names for labels that match the task type you selected on the previous screen\. Ground Truth tries to detect and populate these values by analyzing the manifest, but you might need to set the correct value\.

1. Use the instructions areas of the tool designer to provide context about what the previous labelers were asked to do and what the current adjusters need to check\. 

   You cannot remove or modify existing labels but you can add new labels\. You can remove, modify and add new label category attributes or frame attributes\.

   Be default, preexisting label category attributes and frame attributes will be editable by workers\. If you want to make a label category or frame attribute uneditable, deselect the **Allow workers to edit this attribute** check box for that attribute\.

   To learn more about label category and frame attributes, see [Worker User Interface \(UI\)](sms-point-cloud-general-information.md#sms-point-cloud-worker-task-ui) for 3D point cloud and [Worker User Interface \(UI\)](sms-video-overview.md#sms-video-worker-task-ui) for video frame\. 

1. Choose **See preview** to check that the tool shows the prior labels correctly and presents the task clearly\.

1. Select **Create**\. This will create and start your labeling job\.

## Start a Label Verification or Adjustment Job \(API\)<a name="sms-data-verify-start-api"></a>

### <a name="sms-data-verify-start-api"></a>

Start a label verification or adjustment job by chaining a successfully completed job or starting a new job from scratch using the [ `CreateLabelingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html) operation\. The procedure is almost the same as setting up a new labeling job with `CreateLabelingJob`, with a few modifications\. Use the following sections to learn what modifications are required to chain a labeling job to create an adjustment or verification labeling job\. 

When you create an adjustment or verification labeling job using the Ground Truth API, you *must* use a different `LabelAttributeName` than the original labeling job\. The original labeling job is the job used to create the labels you want adjusted or verified\. 

**Important**  
The label category configuration file you identify for an adjustment or verification job in [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#SageMaker-CreateLabelingJob-request-LabelCategoryConfigS3Uri](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#SageMaker-CreateLabelingJob-request-LabelCategoryConfigS3Uri) of `CreateLabelingJob` must contain the same labels used in the original labeling job\. You can add new labels\. For 3D point cloud and video frame jobs, you can add new label category and frame attributes to the label category configuration file\.

#### Bounding Box and Semantic Segmentation<a name="sms-data-verify-start-api-image"></a>

To create a bounding box or semantic segmentation label verification or adjustment job, use the following guidelines to specify API attributes for the `CreateLabelingJob` operation\. 
+ Use the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#SageMaker-CreateLabelingJob-request-LabelAttributeName](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#SageMaker-CreateLabelingJob-request-LabelAttributeName) parameter to specify the output label name that you want to use for verified or adjusted labels\. You must use a different `LabelAttributeName` than the one used for the original labeling job\.
+ If you are chaining the job, the labels from the previous labeling job to be adjusted or verified will be specified in the custom UI template\. To learn how to create a custom template, see [Create Custom Worker Task Templates](a2i-custom-templates.md)\.

  Identify the location of the UI template in the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UiConfig.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UiConfig.html) parameter\. SageMaker provides widgets that you can use in your custom template to display old labels\. Use the `initial-value` attribute in one of the following crowd elements to extract the labels that need verification or adjustment and include them in your task template:
  + [crowd\-semantic\-segmentation](sms-ui-template-crowd-semantic-segmentation.md)—Use this crowd element in your custom UI task template to specify semantic segmentation labels that need to be verified or adjusted\.
  + [crowd\-bounding\-box](sms-ui-template-crowd-bounding-box.md)—Use this crowd element in your custom UI task template to specify bounding box labels that need to be verified or adjusted\.
+ The [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#SageMaker-CreateLabelingJob-request-LabelCategoryConfigS3Uri](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#SageMaker-CreateLabelingJob-request-LabelCategoryConfigS3Uri) parameter must contain the same label categories as the previous labeling job\.
+ Use the bounding box or semantic segmentation adjustment or verification lambda ARNs for [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HumanTaskConfig.html#sagemaker-Type-HumanTaskConfig-PreHumanTaskLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HumanTaskConfig.html#sagemaker-Type-HumanTaskConfig-PreHumanTaskLambdaArn) and [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AnnotationConsolidationConfig.html#sagemaker-Type-AnnotationConsolidationConfig-AnnotationConsolidationLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AnnotationConsolidationConfig.html#sagemaker-Type-AnnotationConsolidationConfig-AnnotationConsolidationLambdaArn):
  + For bounding box, the adjustment labeling job lambda function ARNs end with `AdjustmentBoundingBox` and the verification lambda function ARNs end with `VerificationBoundingBox`\.
  + For semantic segmentation, the adjustment labeling job lambda function ARNs end with `AdjustmentSemanticSegmentation` and the verification lambda function ARNs end with `VerificationSemanticSegmentation`\.

#### 3D Point Cloud and Video Frame<a name="sms-data-verify-start-api-frame"></a>
+ Use the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#SageMaker-CreateLabelingJob-request-LabelAttributeName](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#SageMaker-CreateLabelingJob-request-LabelAttributeName) parameter to specify the output label name that you want to use for verified or adjusted labels\. You must use a different `LabelAttributeName` than the one used for the original labeling job\. 
+ You must use the human task UI Amazon Resource Name \(ARN\) \(`HumanTaskUiArn`\) used for the original labeling job\. To see supported ARNs, see [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UiConfig.html#sagemaker-Type-UiConfig-HumanTaskUiArn](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UiConfig.html#sagemaker-Type-UiConfig-HumanTaskUiArn)\.
+ In the label category configuration file, you must specify the label attribute name \([https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#SageMaker-CreateLabelingJob-request-LabelAttributeName](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#SageMaker-CreateLabelingJob-request-LabelAttributeName)\) of the previous labeling job that you use to create the adjustment or verification labeling job in the `auditLabelAttributeName` parameter\.
+ You specify whether your labeling job is a *verification* or *adjustment* labeling job using the `editsAllowed` parameter in your label category configuration file identified by the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#SageMaker-CreateLabelingJob-request-LabelCategoryConfigS3Uri](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#SageMaker-CreateLabelingJob-request-LabelCategoryConfigS3Uri) parameter\. 
  + For *verification* labeling jobs, you must use the `editsAllowed` parameter to specify that all labels cannot be modified\. `editsAllowed` must be set to `"none"` in each entry in `labels`\. Optionally, you can specify whether or not label categories attributes and frame attributes can be adjusted by workers\. 
  + Optionally, for *adjustment* labeling jobs, you can use the `editsAllowed` parameter to specify labels, label category attributes, and frame attributes that can or cannot be modified by workers\. If you do not use this parameter, all labels, label category attributes, and frame attributes will be adjustable\.

  To learn more about the `editsAllowed` parameter and configuring your label category configuration file, see [Label Category Configuration File Schema](sms-label-cat-config-attributes.md#sms-label-cat-config-attributes-schema)\. 
+ Use the 3D point cloud or video frame adjustment lambda ARNs for [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HumanTaskConfig.html#sagemaker-Type-HumanTaskConfig-PreHumanTaskLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HumanTaskConfig.html#sagemaker-Type-HumanTaskConfig-PreHumanTaskLambdaArn) and [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AnnotationConsolidationConfig.html#sagemaker-Type-AnnotationConsolidationConfig-AnnotationConsolidationLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AnnotationConsolidationConfig.html#sagemaker-Type-AnnotationConsolidationConfig-AnnotationConsolidationLambdaArn) for both adjustment and verification labeling jobs:
  + For 3D point clouds, the adjustment and verification labeling job lambda function ARNs end with `Adjustment3DPointCloudSemanticSegmentation`, `Adjustment3DPointCloudObjectTracking`, and `Adjustment3DPointCloudObjectDetection` for 3D point cloud semantic segmentation, object detection, and object tracking respectively\. 
  + For video frames, the adjustment and verification labeling job lambda function ARNs end with `AdjustmentVideoObjectDetection` and `AdjustmentVideoObjectTracking` for video frame object detection and object tracking respectively\. 

Ground Truth stores the output data from a label verification or adjustment job in the S3 bucket that you specified in the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_LabelingJobOutputConfig.html#SageMaker-Type-LabelingJobOutputConfig-S3OutputPath](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_LabelingJobOutputConfig.html#SageMaker-Type-LabelingJobOutputConfig-S3OutputPath) parameter of the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html) operation\. For more information about the output data from a label verification or adjustment labeling job, see [Label Verification and Adjustment Data in the Output Manifest](#sms-data-verify-manifest)\.

## Label Verification and Adjustment Data in the Output Manifest<a name="sms-data-verify-manifest"></a>

Amazon SageMaker Ground Truth writes label verification data to the output manifest within the metadata for the label\. It adds two properties to the metadata:
+ A `type` property, with a value of "`groundtruth/label-verification`\.
+ A `worker-feedback` property, with an array of `comment` values\. This property is added when the worker enters comments\. If there are no comments, the field doesn't appear\.

The following example output manifest shows how label verification data appears:

```
{
  "source-ref":"S3 bucket location", 
  "verify-bounding-box":"1",    
  "verify-bounding-box-metadata":  
  {
    "class-name": "bad", 
    "confidence": 0.93, 
    "type": "groundtruth/label-verification", 
    "job-name": "verify-bounding-boxes",
    "human-annotated": "yes",
    "creation-date": "2018-10-18T22:18:13.527256",
    "worker-feedback": [
      {"comment": "The bounding box on the bird is too wide on the right side."},
      {"comment": "The bird on the upper right is not labeled."}
    ]
  }
}
```

The worker output of adjustment tasks resembles the worker output of the original task, except that it contains the adjusted values and an `adjustment-status` property with the value of `adjusted` or `unadjusted` to indicate whether an adjustment was made\.

For more examples of the output of different tasks, see [Output Data](sms-data-output.md)\.

## Cautions and Considerations<a name="sms-data-verify-cautions"></a>

To get expected behavior when creating a label verification or adjustment job, carefully verify your input data\. 
+ If you are using image data, verify that your manifest file contains hexadecimal RGB color information\. 
+ To save money on processing costs, filter your data to ensure you are not including unwanted objects in your labeling job input manifest\.
+ Add required Amazon S3 permissions to ensure your input data is processed correctly\. 

When you create an adjustment or verification labeling job using the Ground Truth API, you *must* use a different `LabelAttributeName` than the original labeling job\.

### Color Information Requirements for Semantic Segmentation Jobs<a name="sms-data-verify-color-info"></a>

To properly reproduce color information in verification or adjustment tasks, the tool requires hexadecimal RGB color information in the manifest \(for example, \#FFFFFF for white\)\. When you set up a Semantic Segmentation verification or adjustment job, the tool examines the manifest to determine if this information is present\. If it can't find it,Amazon SageMaker Ground Truth displays an error message and the ends job setup\.

In prior iterations of the Semantic Segmentation tool, category color information wasn't output in hexadecimal RGB format to the output manifest\. That feature was introduced to the output manifest at the same time the verification and adjustment workflows were introduced\. Therefore, older output manifests aren't compatible with this new workflow\.

### Filter Your Data Before Starting the Job<a name="sms-data-verify-filter"></a>

Amazon SageMaker Ground Truth processes all objects in your input manifest\. If you have a partially labeled data set, you might want to create a custom manifest using an [Amazon S3 Select query](https://docs.aws.amazon.com/AmazonS3/latest/dev/selecting-content-from-objects.html) on your input manifest\. Unlabeled objects individually fail, but they don't cause the job to fail, and they might incur processing costs\. Filtering out objects you don't want verified reduces your costs\.

If you create a verification job using the console, you can use the filtering tools provided there\. If you create jobs using the API, make filtering your data part of your workflow where needed\.