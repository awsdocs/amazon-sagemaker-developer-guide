# 3D Point Cloud Labeling Jobs Overview<a name="sms-point-cloud-general-information"></a>

This topic provides an overview of the unique features of a Ground Truth 3D point cloud labeling job\. You can use the 3D point cloud labeling jobs to have workers label objects in a 3D point cloud generated from a 3D sensors like LiDAR and depth cameras or generated from 3D reconstruction by stitching images captured by an agent like a drone\. 

## Job Pre\-processing Time<a name="sms-point-cloud-job-creation-time"></a>

When you create a 3D point cloud labeling job, you need to provide an [input manifest file](sms-point-cloud-input-data.md)\. The input manifest file can be:
+ A *frame input manifest file* that has a single point cloud frame on each line\. 
+ A *sequence input manifest file* that has a single sequence on each line\. A sequence is defined as a temporal series of point cloud frames\. 

For both types of manifest files, *job pre\-processing time* \(that is, the time before Ground Truth starts sending tasks to your workers\) depends on the total number and size of point cloud frames you provide in your input manifest file\. For frame input manifest files, this is the number of lines in your manifest file\. For sequence manifest files, this is the number of frames in each sequence multiplied by the total number of sequences, or lines, in your manifest file\. 

Additionally, the number of points per point cloud and the number of fused sensor data objects \(like images\) factor into job pre\-processing times\. On average, Ground Truth can pre\-process 200 point cloud frames in approximately 5 minutes\. If you create a 3D point cloud labeling job with a large number of point cloud frames, you might experience longer job pre\-processing times\. For example, if you create a sequence input manifest file with 4 point cloud sequences, and each sequence contains 200 point clouds, Ground Truth pre\-processes 800 point clouds and so your job pre\-processing time might be around 20 minutes\. During this time, your labeling job status is `InProgress`\. 

While your 3D point cloud labeling job is pre\-processing, you receive CloudWatch messages notifying you of the status of your job\. To identify these messages, search for `3D_POINT_CLOUD_PROCESSING_STATUS` in your labeling job logs\. 

For **frame input manifest files**, your CloudWatch logs will have a message similar to the following:

```
{
    "labeling-job-name": "example-point-cloud-labeling-job",
    "event-name": "3D_POINT_CLOUD_PROCESSING_STATUS",
    "event-log-message": "datasetObjectId from: 0 to 10, status: IN_PROGRESS"
}
```

The event log message, `datasetObjectId from: 0 to 10, status: IN_PROGRESS` identifies the number of frames from your input manifest that have been processed\. You receive a new message every time a frame has been processed\. For example, after a single frame has processed, you receive another message that says `datasetObjectId from: 1 to 10, status: IN_PROGRESS`\. 

For **sequence input manifest files**, your CloudWatch logs will have a message similar to the following:

```
{
    "labeling-job-name": "example-point-cloud-labeling-job",
    "event-name": "3D_POINT_CLOUD_PROCESSING_STATUS",
    "event-log-message": "datasetObjectId: 0, status: IN_PROGRESS"
}
```

The event log message, `datasetObjectId from: 0, status: IN_PROGRESS` identifies the number of sequences from your input manifest that have been processed\. You receive a new message every time a sequence has been processed\. For example, after a single sequence has processed, you receive a message that says `datasetObjectId from: 1, status: IN_PROGRESS` as the next sequence begins processing\. 

## Job Completion Times<a name="sms-point-cloud-job-completion-times"></a>

3D point cloud labeling jobs can take workers hours to complete\. You can set the total amount of time that workers can work on each task when you create a labeling job\. The maximum time you can set for workers to work on tasks is 7 days\. The default value is 3 days\. 

It is strongly recommended that you create tasks that workers can complete within 12 hours\. Workers must keep the worker UI open while working on a task\. They can save work as they go and Ground Truth will save their work every 15 minutes\.

When using the SageMaker `CreateLabelingJob` API operation, set the total time a task is available to workers in the `TaskTimeLimitInSeconds` parameter of `HumanTaskConfig`\. 

When you create a labeling job in the console, you can specify this time limit when you select your workforce type and your work team\.

**Important**  
If you set your task time limit to be greater than 8 hours, you must set `MaxSessionDuration` for your IAM execution role to at least 8 hours\. See [3D Point Cloud Labeling Job Permission Requirements](#sms-security-permission-3d-point-cloud) for more information\. 

## Workforces<a name="sms-point-cloud-workforces"></a>

When you create a 3D point cloud labeling job, you need to specify a work team that will complete your point cloud annotation tasks\. You can choose a work team from a private workforce of your own workers, or from a vendor workforce that you select in the AWS Marketplace\. You cannot use the Amazon Mechanical Turk workforce for 3D point cloud labeling jobs\. 

To learn more about vendor workforce, see [Managing Vendor Workforces](sms-workforce-management-vendor.md)\.

To learn how to create and manage a private workforce, see [Use a Private Workforce](sms-workforce-private.md)\.

## Worker User Interface \(UI\)<a name="sms-point-cloud-worker-task-ui"></a>

Ground Truth provides a worker user interface \(UI\), tools, and assistive labeling features to help workers complete your 3D point cloud labeling tasks\. 

You can preview the worker UI when you create a labeling job in the console\.

When you create a labeling job using the API operation `CreateLabelingJob`, you must provide an ARN provided by Ground Truth in the parameter [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UiConfig.html#sagemaker-Type-UiConfig-UiTemplateS3Uri](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UiConfig.html#sagemaker-Type-UiConfig-UiTemplateS3Uri) to specify the worker UI for your task type\. You can use `HumanTaskUiArn` with the SageMaker [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_RenderUiTemplate.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_RenderUiTemplate.html) API operation to preview the worker UI\. 

You provide worker instructions, labels, and optionally, label category attributes that are displayed in the worker UI\.

### Label Category Attributes<a name="sms-point-cloud-label-and-frame-attributes"></a>

When you create a 3D point cloud object tracking or object detection labeling job, you can add one or more *label category attributes*\. You can add *frame attributes* to all 3D point cloud task types: 
+ **Label category attribute** – A list of options \(strings\), a free form text box, or a numeric field associated with one or more labels\. It is used by workers to to provide metadata about a label\. 
+ **Frame attribute** – A list of options \(strings\), a free form text box, or a numeric field that appears on each point cloud frame a worker is sent to annotate\. It is used by workers to provide metadata about frames\. 

Additionally, you can use label and frame attributes to have workers verify labels in a 3D point cloud label verification job\. 

Use the following sections to learn more about these attributes\. To learn how to add label category and frame attributes to a labeling job, use the **Create Labeling Job** section on the [task type page](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-point-cloud-task-types) of your choice\.

#### Label Category Attributes<a name="sms-point-cloud-label-attributes"></a>

Add label category attributes to labels to give workers the ability to provide more information about the annotations they create\. A label category attribute is added to an individual label, or to all labels\. When a label category attribute is applied to all labels it is referred to as a *global label category attribute*\. 

For example, if you add the label category *car*, you might also want to capture additional data about your labeled cars, such as if they are occluded or the size of the car\. You can capture this metadata using label category attributes\. In this example, if you added the attribute *occluded* to the car label category, you can assign *partial*, *completely*, *no* to the *occluded* attribute and enable workers to select one of these options\. 

When you create a label verification job, you add labels category attributes to each label you want workers to verify\.

#### Frame Attributes<a name="sms-point-cloud-frame-attributes"></a>

Add frame attributes to give workers the ability to provide more information about individual point cloud frames\. You can specify up to 10 frame attributes, and these attributes will appear on all frames\.

For example, you can add a frame attribute that allows workers to enter a number\. You may want to use this attribute to have workers identify the number of objects they see in a particular frame\. 

In another example, you may want to provide a free\-form text box to give workers the ability to provide a free form answer to a question\.

When you create a label verification job, you can add one or more frame attributes to ask workers to provide feedback on all labels in a point cloud frame\. 

### Worker Instructions<a name="sms-point-cloud-worker-instructions-general"></a>

You can provide worker instructions to help your workers complete your point cloud labeling tasks\. You might want to use these instructions to do the following:
+ Best practices and things to avoid when annotating objects\.
+ Explanation of the label category attributes provided \(for object detection and object tracking tasks\), and how to use them\.
+ Advice on how to save time while labeling by using keyboard shortcuts\. 

You can add your worker instructions using the SageMaker console while creating a labeling job\. If you create a labeling job using the API operation `CreateLabelingJob`, you specify worker instructions in your label category configuration file\. 

In addition to your instructions, Ground Truth provides a link to help workers navigate and use the worker portal\. View these instructions by selecting the task type on [Worker Instructions](sms-point-cloud-worker-instructions.md)\. 

### Declining Tasks<a name="sms-decline-task-point-cloud"></a>

Workers are able to decline tasks\. 

Workers decline a task if the instructions are not clear, input data is not displaying correctly, or if they encounter some other issue with the task\. If the number of workers per dataset object \([https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HumanTaskConfig.html#sagemaker-Type-HumanTaskConfig-NumberOfHumanWorkersPerDataObject](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HumanTaskConfig.html#sagemaker-Type-HumanTaskConfig-NumberOfHumanWorkersPerDataObject)\) decline the task, the data object is marked as expired and will not be sent to additional workers\.

## 3D Point Cloud Labeling Job Permission Requirements<a name="sms-security-permission-3d-point-cloud"></a>

When you create a 3D point cloud labeling job, in addition to the permission requirements found in [Assign IAM Permissions to Use Ground Truth](sms-security-permission.md), you must add a CORS policy to your S3 bucket that contains your input manifest file\. 

Additionally if you choose to allow workers to work on tasks for more than 8 hours, you must increase the `MaxSessionDuration` of the IAM execution role you use to create the labeling job\. 

### Add a CORS Permission Policy to S3 Bucket<a name="sms-permissions-execution-role"></a>

When you create a 3D point cloud labeling job, you specify buckets in S3 where your input data and manifest file are located and where your output data will be stored\. These buckets may be the same\. You must attach the following Cross\-origin resource sharing \(CORS\) policy to your input and output buckets\. If you use the Amazon S3 console to add the policy to your bucket, you must use the JSON format\.

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

To learn how to add a CORS policy to an S3 bucket, see [How do I add cross\-domain resource sharing with CORS?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/add-cors-configuration.html) in the Amazon Simple Storage Service Console User Guide\.

### Increase MaxSessionDuration for Execution Role<a name="sms-3d-pointcloud-maxsessduration"></a>

3D point cloud labeling tasks can take workers more time to complete than other task types\. You can set the total amount of time that workers can work on each task by doing one of the following: 
+ When you create a labeling job in the console, set **Task Timeout** when you select your work team\.
+ Using the `TaskTimeLimitInSeconds` parameter when creating a labeling job using the SageMaker API\. 

The maximum time you can set for workers to work on tasks is 7 days\. The default value is 3 days\. If you set your task time limit to be greater than 8 hours, you must set `MaxSessionDuration` for your IAM execution role to at least 8 hours\. 

To see how to update this value for your IAM role, see [Modifying a Role ](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_modify.html) in the IAM User Guide, choose your preferred method to modify the role, and then follow the steps in [Modifying a Role Maximum Session Duration](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-managingrole-editing-console.html#roles-modify_max-session-duration)\. To learn more about permission requirements for the Ground Truth execution role, see [Create an Execution Role to Start a Labeling Job](sms-security-permission.md#sms-security-permission-execution-role)\.