# 3D Point Cloud Object Tracking<a name="sms-point-cloud-object-tracking"></a>

Use this task type when you want workers to add and fit 3D cuboids around objects to track their movement across 3D point cloud frames\. For example, you can use this task type to ask workers to track the movement of vehicles across multiple point cloud frames\. 

For this task type, the data object that workers label is a sequence of point cloud frames\. A *sequence* is defined as a temporal series of point cloud frames\. Ground Truth renders a series of 3D point cloud visualizations using a sequence you provide and workers can switch between these 3D point cloud frames in the worker task interface\. 

Ground Truth providers workers with tools to annotate objects with 9 degrees of freedom: \(x,y,z,rx,ry,rz,l,w,h\) in three dimensions in both 3D scene and projected side views \(top, side, and back\)\. When a worker draws a cuboid around an object, that cuboid is given a unique ID, for example `Car:1` for one car in the sequence and `Car:2` for another\. Workers use that ID to label the same object in multiple frames\. 

You can also provide camera data to give workers more visual information about scenes in the frame, and to help workers draw 3D cuboids around objects\. When a worker adds a 3D cuboid to identify an object in either the 2D image or the 3D point cloud, and the cuboid shows up in the other view\. 

You can adjust annotations created in a 3D point cloud object detection labeling job using the 3D point cloud object tracking adjustment task type\. 

If you are a new user of the Ground Truth 3D point cloud labeling modality, we recommend you review [3D Point Cloud Labeling Jobs Overview](sms-point-cloud-general-information.md)\. This labeling modality is different from other Ground Truth task types, and this page provides an overview of important details you should be aware of when creating a 3D point cloud labeling job\.

**Topics**
+ [View the Worker Task Interface](#sms-point-cloud-object-tracking-worker-ui)
+ [Create a 3D Point Cloud Object Tracking Labeling Job](#sms-point-cloud-object-tracking-create-labeling-job)
+ [Output Data Format](#sms-point-cloud-object-tracking-output-data)

## View the Worker Task Interface<a name="sms-point-cloud-object-tracking-worker-ui"></a>

Ground Truth provides workers with a web portal and tools to complete your 3D point cloud object tracking annotation tasks\. When you create the labeling job, you provide the Amazon Resource Name \(ARN\) for a pre\-built Ground Truth UI in the `HumanTaskUiArn` parameter\. When you create a labeling job using this task type in the console, this UI is automatically used\. You can preview and interact with the worker UI when you create a labeling job in the console\. If you are a new use, it is recommended that you create a labeling job using the console to ensure your label attributes, point cloud frames, and if applicable, images, appear as expected\. 

The following is a GIF of the 3D point cloud object tracking worker task interface and demonstrates how the worker can navigate the point cloud frames in the sequence\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pointcloud/gifs/object_tracking/nav_frames.gif)

Once workers add a single cuboid, that cuboid is replicated in all frames of the sequence with the same ID\. Once workers adjust the cuboid in another frame, Ground Truth will interpolate the movement of that object and adjust all cuboids between the manually adjusted frames\. The following GIF demonstrates this interpolation feature\. In the navigation bar on the bottom\-left, red\-areas indicate manually adjusted frames\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pointcloud/gifs/object_tracking/label-interpolation.gif)

If you provide camera data for sensor fusion, images are matched up with scenes in point cloud frames\. These images appear in the worker portal as shown in the following GIF\. 

Worker can navigate in the 3D scene using their keyboard and mouse\. They can:
+ Double click on specific objects in the point cloud to zoom into them\.
+ Use a mouse\-scroller or trackpad to zoom in and out of the point cloud\.
+ Use both keyboard arrow keys and Q, E, A, and D keys to move Up, Down, Left, Right\. Use keyboard keys W and S to zoom in and out\. 

Once a worker places a cuboids in the 3D scene, a side\-view will appear with the three projected side views: top, side, and back\. These side\-views show points in and around the placed cuboid and help workers refine cuboid boundaries in that area\. Workers can zoom in and out of each of those side\-views using their mouse\. 

The following video demonstrates movements around the 3D point cloud and in the side\-view\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pointcloud/gifs/object_tracking/nav_general_UI.gif)

Additional view options and features are available\. See the [worker instruction page](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-point-cloud-worker-instructions-object-tracking.html) for a comprehensive overview of the Worker UI\. 

### Worker Tools<a name="sms-point-cloud-object-tracking-worker-tools"></a>

Workers can navigate through the 3D point cloud by zooming in and out, and moving in all directions around the cloud using the mouse and keyboard shortcuts\. If workers click on a point in the point cloud, the UI will automatically zoom into that area\. Workers can use various tools to draw 3D cuboid around objects\. For more information, see **Assistive Labeling Tools**\. 

After workers have placed a 3D cuboid in the point cloud, they can adjust these cuboids to fit tightly around cars using a variety of views: directly in the 3D cuboid, in a side\-view featuring three zoomed\-in perspectives of the point cloud around the box, and if you include images for sensor fusion, directly in the 2D image\. 

View options that enable workers to easily hide or view label text, a ground mesh, and additional point attributes\. Workers can also choose between perspective and orthogonal projections\. 

**Assistive Labeling Tools**  
Ground Truth helps workers annotate 3D point clouds faster and more accurately using UX, machine learning and computer vision powered assistive labeling tools for 3D point cloud object tracking tasks\. The following assistive labeling tools are available for this task type:
+ **Label autofill** – When a worker adds a cuboid to a frame, a cuboid with the same dimensions and orientation is automatically added to all frames in the sequence\. 
+ **Label interpolation** – After a worker has labeled a single object in two frames, Ground Truth uses those annotations to interpolate the movement of that object between those two frames\.
+ **Bulk label management** – Workers can add, delete, and rename annotations in bulk\. 
  + Workers can manually delete annotations for a given object before or after a frame\. For example, a worker can delete all labels for an object after frame 10 if that object is no longer located in the scene after that frame\. 
  + If a worker accidentally bulk deletes all annotations for a object, they can add them back\. For example, if a worker deletes all annotations for an object before frame 100, they can bulk add them to those frames\. 
  + Workers can rename a label in one frame and all 3D cuboids assigned that label are updated with the new name across all frames\. 
+ **Snapping** – Workers can add a cuboid around an object and use a keyboard shortcut or menu option to have Ground Truth's autofit tool snap the cuboid tightly around the object's boundaries\. 
+ **Fit to ground** – After a worker adds a cuboid to the 3D scene, the worker can automatically snap the cuboid to the ground\. For example, the worker can use this feature to snap a cuboid to the road or sidewalk in the scene\. 
+ **Multi\-view labeling** – After a worker adds a 3D cuboid to the 3D scene, a side \-panel displays front and two side perspectives to help the worker adjust the cuboid tightly around the object\. Workers can annotation the 3D point cloud, the side panel and the adjustments appear in the other views in real time\. 
+ **Sensor fusion** – If you provide data for sensor fusion, workers can adjust annotations in the 3D scenes and in 2D images, and the annotations will be projected into the other view in real time\. 
+ **Auto\-merge cuboids **– Workers can automatically merge two cuboids across all frames if they determine that cuboids with different labels actually represent a single object\. 
+ **View options **– Enables workers to easily hide or view label text, a ground mesh, and additional point attributes like color or intensity\. Workers can also choose between perspective and orthogonal projections\. 

## Create a 3D Point Cloud Object Tracking Labeling Job<a name="sms-point-cloud-object-tracking-create-labeling-job"></a>

You can create a 3D point cloud labeling job using the SageMaker console or API operation, [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html)\. To create a labeling job for this task type you need the following: 
+ A sequence input manifest file\. To learn how to create this type of manifest file, see [Create a Point Cloud Sequence Input Manifest](sms-point-cloud-multi-frame-input-data.md)\. If you are a new user of Ground Truth 3D point cloud labeling modalities, we recommend that you review [Accepted Raw 3D Data Formats](sms-point-cloud-raw-data-types.md)\. 
+ A work team from a private or vendor workforce\. You cannot use Amazon Mechanical Turk for 3D point cloud labeling jobs\. To learn how to create workforces and work teams, see [Create and Manage Workforces](sms-workforce-management.md)\.
+ A label category configuration file\. For more information, see [Create a Labeling Category Configuration File with Label Category Attributes](sms-label-cat-config-attributes.md)\. 

Additionally, make sure that you have reviewed and satisfied the [Assign IAM Permissions to Use Ground Truth](sms-security-permission.md)\. 

To learn how to create a labeling job using the console or an API, see the following sections\. 

### Create a Labeling Job \(API\)<a name="sms-point-cloud-object-tracking-create-labeling-job-api"></a>

This section covers details you need to know when you create a labeling job using the SageMaker API operation `CreateLabelingJob`\. This API defines this operation for all AWS SDKs\. To see a list of language\-specific SDKs supported for this operation, review the **See Also** section of [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html)\. 

[Create a Labeling Job \(API\)](sms-create-labeling-job-api.md) provides an overview of the `CreateLabelingJob` operation\. Follow these instructions and do the following while you configure your request: 
+ You must enter an ARN for `HumanTaskUiArn`\. Use `arn:aws:sagemaker:<region>:394669845002:human-task-ui/PointCloudObjectTracking`\. Replace `<region>` with the AWS Region you are creating the labeling job in\. 

  There should not be an entry for the `UiTemplateS3Uri` parameter\. 
+ Your [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#sagemaker-CreateLabelingJob-request-LabelAttributeName](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#sagemaker-CreateLabelingJob-request-LabelAttributeName) must end in `-ref`\. For example, `ot-labels-ref`\. 
+ Your input manifest file must be a point cloud frame sequence manifest file\. For more information, see [Create a Point Cloud Sequence Input Manifest](sms-point-cloud-multi-frame-input-data.md)\. 
+ You specify your labels and worker instructions in a label category configuration file\. For more information, see [Create a Labeling Category Configuration File with Label Category Attributes](sms-label-cat-config-attributes.md) to learn how to create this file\. 
+ You need to provide pre\-defined ARNs for the pre\-annotation and post\-annotation \(ACS\) Lambda functions\. These ARNs are specific to the AWS Region you use to create your labeling job\. 
  + To find the pre\-annotation Lambda ARN, refer to [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HumanTaskConfig.html#sagemaker-Type-HumanTaskConfig-PreHumanTaskLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HumanTaskConfig.html#sagemaker-Type-HumanTaskConfig-PreHumanTaskLambdaArn)\. Use the Region you are creating your labeling job in to find the correct ARN that ends with `PRE-3DPointCloudObjectTracking`\. 
  + To find the post\-annotation Lambda ARN, refer to [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AnnotationConsolidationConfig.html#sagemaker-Type-AnnotationConsolidationConfig-AnnotationConsolidationLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AnnotationConsolidationConfig.html#sagemaker-Type-AnnotationConsolidationConfig-AnnotationConsolidationLambdaArn)\. Use the Region you are creating your labeling job in to find the correct ARN that ends with `ACS-3DPointCloudObjectTracking`\. 
+ The number of workers specified in `NumberOfHumanWorkersPerDataObject` should be `1`\. 
+ Automated data labeling is not supported for 3D point cloud labeling jobs\. You should not specify values for parameters in `[LabelingJobAlgorithmsConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#sagemaker-CreateLabelingJob-request-LabelingJobAlgorithmsConfig)`\. 
+ 3D point cloud object tracking labeling jobs can take multiple hours to complete\. You can specify a longer time limit for these labeling jobs in `TaskTimeLimitInSeconds` \(up to 7 days, or 604,800 seconds\)\. 
**Important**  
If you set your take time limit to be greater than 8 hours, you must set `MaxSessionDuration` for your IAM execution role to at least 8 hours\. To see how to update this value for your IAM role, see [Modifying a Role ](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_modify.html) in the IAM User Guide, choose your preferred method to modify the role, and then follow the steps in [Modifying a Role Maximum Session Duration](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-managingrole-editing-console.html#roles-modify_max-session-duration)\. 

#### Create an Adjustment Labeling Job \(API\)<a name="sms-point-cloud-object-tracking-adjustment-create-labeling-job-api"></a>

To create an adjustment labeling job, use the instructions in the previous section, with the following modifications: 
+ In your label category configuration file, you must include `auditLabelAttributeName`\. Use this parameter to input the [LabelAttributeName](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#sagemaker-CreateLabelingJob-request-LabelAttributeName) used in the labeling job that generated the annotations you want your worker to adjust\. 
**Important**  
When you create a labeling job in the console, if you did not specify a label category attribute name, the **Name** of your job is used as the LabelAttributeName\. 

  For example, if your label category attribute name was `point-cloud-labels` in your first labeling job, add the following to your object tracking adjustment labeling job label category configuration file\. To learn how to create this file, see [Create a Labeling Category Configuration File with Label Category Attributes](sms-label-cat-config-attributes.md)\. 

  ```
  {
      "documentVersion": "2020-03-01",
      "labels": [
          {
              "label": "Car",
              "categoryAttributes": [
                  {
                      "name":"X",
                      "description":"something",
                      "type":"string",
                      "enum": ["foo", "buz", "buzz2"]
                  },
          },
          {
              "label": "Pedestrian",
          },
          {
              "label": "Cyclist"
          }
      ],
      "instructions": {"shortInstruction":"Select the appropriate label and paint all objects in the point cloud that it applies to the same color", "fullInstruction":"<html markup>"},
      "auditLabelAttributeName": "point-cloud-labels"
  }
  ```
+ You need to provide a pre\-defined ARNs for the pre\-annotation and post\-annotation \(ACS\) Lambda functions\. These ARNs are specific to the AWS Region you use to create your labeling job\. 
  + To find the pre\-annotation Lambda ARN, refer to [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HumanTaskConfig.html#sagemaker-Type-HumanTaskConfig-PreHumanTaskLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HumanTaskConfig.html#sagemaker-Type-HumanTaskConfig-PreHumanTaskLambdaArn)\. Use the Region you are creating your labeling job in to find the correct ARN that ends with `PRE-Adjustment3DPointCloudObjectTracking`\.
  + To find the post\-annotation Lambda ARN, refer to [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AnnotationConsolidationConfig.html#sagemaker-Type-AnnotationConsolidationConfig-AnnotationConsolidationLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AnnotationConsolidationConfig.html#sagemaker-Type-AnnotationConsolidationConfig-AnnotationConsolidationLambdaArn)\. Use the Region you are creating your labeling job in to find the correct ARN that ends with `ACS-Adjustment3DPointCloudObjectTracking`\.
+ The [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#SageMaker-CreateLabelingJob-request-LabelCategoryConfigS3Uri](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#SageMaker-CreateLabelingJob-request-LabelCategoryConfigS3Uri) parameter must contain the same label categories as the previous labeling job\. Adding new label categories or adjusting label categories is not supported\.

### Create a Labeling Job \(Console\)<a name="sms-point-cloud-object-tracking-create-labeling-job-console"></a>

You can follow the instructions [Create a Labeling Job \(Console\)](sms-create-labeling-job-console.md) in order to learn how to create a 3D point cloud object tracking labeling job in the SageMaker console\. While you are creating your labeling job, be aware of the following: 
+ Your input manifest file must be a sequence manifest file\. For more information, see [Create a Point Cloud Sequence Input Manifest](sms-point-cloud-multi-frame-input-data.md)\. 
+ Optionally, you can provide label category attributes\. Workers can assign one or more of these attributes to annotations to provide more information about that object\. For example, you might want to use the attribute *occluded* to have workers identify when an object is partially obstructed\.
+ Automated data labeling and annotation consolidation are not supported for 3D point cloud labeling tasks\. 
+ 3D point cloud object tracking labeling jobs can take multiple hours to complete\. You can specify a longer time limit for these labeling jobs when you select your work team \(up to 7 days, or 604800 seconds\)\. 
**Important**  
If you set your take time limit to be greater than 8 hours, you must set `MaxSessionDuration` for your IAM execution role to at least 8 hours\. To see how to update this value for your IAM role, see [Modifying a Role ](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_modify.html) in the IAM User Guide, choose your preferred method to modify the role, and then follow the steps in [Modifying a Role Maximum Session Duration](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-managingrole-editing-console.html#roles-modify_max-session-duration)\. 

#### Create an Adjustment Labeling Job \(console\)<a name="sms-point-cloud-object-tracking-adjustment-create-labeling-job-console"></a>

You can create an adjustment labeling job in the console by *chaining* a successfully completed object tracking labeling job\. To learn more, see [Create and Start a Label Verification Job \(Console\)](sms-verification-data.md#sms-data-verify-start-console)\.

## Output Data Format<a name="sms-point-cloud-object-tracking-output-data"></a>

When you create a 3D point cloud object tracking labeling job, tasks are sent to workers\. When these workers complete their tasks, their annotations are written to the Amazon S3 bucket you specified when you created the labeling job\. The output data format determines what you see in your Amazon S3 bucket when your labeling job status \([LabelingJobStatus](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeLabelingJob.html#API_DescribeLabelingJob_ResponseSyntax)\) is `Completed`\. 

If you are a new user of Ground Truth, see [Output Data](sms-data-output.md) to learn more about the Ground Truth output data format\. To learn about the 3D point cloud object tracking output data format, see [3D Point Cloud Object Tracking Output](sms-data-output.md#sms-output-point-cloud-object-tracking)\. 