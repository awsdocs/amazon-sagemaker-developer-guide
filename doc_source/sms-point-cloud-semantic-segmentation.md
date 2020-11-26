# 3D Point Cloud Semantic Segmentation<a name="sms-point-cloud-semantic-segmentation"></a>

Semantic segmentation involves classifying individual points of a 3D point cloud into pre\-specified categories\. Use this task type when you want workers to create a point\-level semantic segmentation mask for 3D point clouds\. For example, if you specify the classes `car`, `pedestrian`, and `bike`, workers select one class at a time, and color all of the points that this class applies to the same color in the point cloud\. 

For this task type, the data object that workers label is a single point cloud frame\. Ground Truth generates a 3D point cloud visualization using point cloud data you provide\. You can also provide camera data to give workers more visual information about scenes in the frame, and to help workers paint objects\. When a worker paints an object in either the 2D image or the 3D point cloud, the paint shows up in the other view\. 

You can adjust annotations created in a 3D point cloud object detection labeling job using the 3D point cloud semantic segmentation adjustment task type\. 

If you are a new user of the Ground Truth 3D point cloud labeling modality, we recommend you review [3D Point Cloud Labeling Jobs Overview](sms-point-cloud-general-information.md)\. This labeling modality is different from other Ground Truth task types, and this topic provides an overview of important details you should be aware of when creating a 3D point cloud labeling job\.

**Topics**
+ [View the Worker Task Interface](#sms-point-cloud-semantic-segmentation-worker-ui)
+ [Create a 3D Point Cloud Semantic Segmentation Labeling Job](#sms-point-cloud-semantic-segmentation-create-labeling-job)
+ [Output Data Format](#sms-point-cloud-semantic-segmentation-input-data)

## View the Worker Task Interface<a name="sms-point-cloud-semantic-segmentation-worker-ui"></a>

Ground Truth provides workers with a web portal and tools to complete your 3D point cloud semantic segmentation annotation tasks\. When you create the labeling job, you provide the Amazon Resource Name \(ARN\) for a pre\-built Ground Truth UI in the `HumanTaskUiArn` parameter\. When you create a labeling job using this task type in the console, this UI is automatically used\. You can preview and interact with the worker UI when you create a labeling job in the console\. If you are a new use, it is recommended that you create a labeling job using the console to ensure your label attributes, point cloud frames, and if applicable, images, appear as expected\. 

The following is a GIF of the 3D point cloud semantic segmentation worker task interface\. If you provide camera data for sensor fusion, images are matched with scenes in the point cloud frame\. Workers can paint objects in either the 3D point cloud or the 2D image, and the paint appears in the corresponding location in the other medium\. These images appear in the worker portal as shown in the following GIF\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pointcloud/gifs/semantic_seg/ss_paint_sf.gif)

Worker can navigate in the 3D scene using their keyboard and mouse\. They can:
+ Double click on specific objects in the point cloud to zoom into them\.
+ Use a mouse\-scroller or trackpad to zoom in and out of the point cloud\.
+ Use both keyboard arrow keys and Q, E, A, and D keys to move Up, Down, Left, Right\. Use keyboard keys W and S to zoom in and out\. 

The following video demonstrates movements around the 3D point cloud\. Workers can hide and re\-expand all side views and menus\. In this GIF, the side\-views and menus have been collapsed\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pointcloud/gifs/semantic_seg/ss_nav_worker_portal.gif)

The following GIF demonstrates how a worker can label multiple objects quickly, refine painted objects using the Unpaint option and then view only points that have been painted\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pointcloud/gifs/semantic_seg/ss-view-options.gif)

Additional view options and features are available\. See the [worker instruction page](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-point-cloud-worker-instructions-semantic-segmentation.html) for a comprehensive overview of the Worker UI\. 

**Worker Tools**  
Workers can navigate through the 3D point cloud by zooming in and out, and moving in all directions around the cloud using the mouse and keyboard shortcuts\. When you create a semantic segmentation job, workers have the following tools available to them: 
+ A paint brush to paint and unpaint objects\. Workers paint objects by selecting a label category and then painting in the 3D point cloud\. Workers unpaint objects by selecting the Unpaint option from the label category menu and using the paint brush to erase paint\. 
+ A polygon tool that workers can use to select and paint an area in the point cloud\. 
+ A background paint tool, which enables workers to paint behind objects they have already annotated without altering the original annotations\. For example, workers might use this tool to paint the road after painting all of the cars on the road\. 
+ View options that enable workers to easily hide or view label text, a ground mesh, and additional point attributes like color or intensity\. Workers can also choose between perspective and orthogonal projections\. 

## Create a 3D Point Cloud Semantic Segmentation Labeling Job<a name="sms-point-cloud-semantic-segmentation-create-labeling-job"></a>

You can create a 3D point cloud labeling job using the SageMaker console or API operation, [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html)\. To create a labeling job for this task type you need the following: 
+ A single\-frame input manifest file\. To learn how to create this type of manifest file, see [Create a Point Cloud Frame Input Manifest File](sms-point-cloud-single-frame-input-data.md)\. If you are a new user of Ground Truth 3D point cloud labeling modalities, we recommend that you review [Accepted Raw 3D Data Formats](sms-point-cloud-raw-data-types.md)\. 
+ A work team from a private or vendor workforce\. You cannot use Amazon Mechanical Turk workers for 3D point cloud labeling jobs\. To learn how to create workforces and work teams, see [Create and Manage Workforces](sms-workforce-management.md)\.
+ A label category configuration file\. For more information, see [Create a Labeling Category Configuration File with Label Category Attributes](sms-label-cat-config-attributes.md)\. 

Additionally, make sure that you have reviewed and satisfied the [Assign IAM Permissions to Use Ground Truth](sms-security-permission.md)\. 

Use one of the following sections to learn how to create a labeling job using the console or an API\. 

### Create a Labeling Job \(API\)<a name="sms-point-cloud-semantic-segmentation-api"></a>

This section covers details you need to know when you create a labeling job using the SageMaker API operation `CreateLabelingJob`\. This API defines this operation for all AWS SDKs\. To see a list of language\-specific SDKs supported for this operation, review the **See Also** section of [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html)\. 

The page, [Create a Labeling Job \(API\)](sms-create-labeling-job-api.md), provides an overview of the `CreateLabelingJob` operation\. Follow these instructions and do the following while you configure your request: 
+ You must enter an ARN for `HumanTaskUiArn`\. Use `arn:aws:sagemaker:<region>:394669845002:human-task-ui/PointCloudSemanticSegmentation`\. Replace `<region>` with the AWS Region you are creating the labeling job in\. 

  There should not be an entry for the `UiTemplateS3Uri` parameter\. 
+ Your [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#sagemaker-CreateLabelingJob-request-LabelAttributeName](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#sagemaker-CreateLabelingJob-request-LabelAttributeName) must end in `-ref`\. For example, `ss-labels-ref`\. 
+ Your input manifest file must be a single\-frame manifest file\. For more information, see [Create a Point Cloud Frame Input Manifest File](sms-point-cloud-single-frame-input-data.md)\. 
+ You specify your labels and worker instructions in a label category configuration file\. See [Create a Labeling Category Configuration File with Label Category Attributes](sms-label-cat-config-attributes.md) to learn how to create this file\. 
+ You need to provide a pre\-defined ARNs for the pre\-annotation and post\-annotation \(ACS\) Lambda functions\. These ARNs are specific to the AWS Region you use to create your labeling job\. 
  + To find the pre\-annotation Lambda ARN, refer to [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HumanTaskConfig.html#sagemaker-Type-HumanTaskConfig-PreHumanTaskLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HumanTaskConfig.html#sagemaker-Type-HumanTaskConfig-PreHumanTaskLambdaArn)\. Use the Region you are creating your labeling job in to find the correct ARN\. For example, if you are creating your labeling job in us\-east\-1, the ARN will be `arn:aws:lambda:us-east-1:432418664414:function:PRE-3DPointCloudSemanticSegmentation`\. 
  + To find the post\-annotation Lambda ARN, refer to [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AnnotationConsolidationConfig.html#sagemaker-Type-AnnotationConsolidationConfig-AnnotationConsolidationLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AnnotationConsolidationConfig.html#sagemaker-Type-AnnotationConsolidationConfig-AnnotationConsolidationLambdaArn)\. Use the Region you are creating your labeling job in to find the correct ARN\. For example, if you are creating your labeling job in us\-east\-1, the ARN will be `arn:aws:lambda:us-east-1:432418664414:function:ACS-3DPointCloudSemanticSegmentation`\. 
+ The number of workers specified in `NumberOfHumanWorkersPerDataObject` should be `1`\. 
+ Automated data labeling is not supported for 3D point cloud labeling jobs\. You should not specify values for parameters in `[LabelingJobAlgorithmsConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#sagemaker-CreateLabelingJob-request-LabelingJobAlgorithmsConfig)`\. 
+ 3D point cloud semantic segmentation labeling jobs can take multiple hours to complete\. You can specify a longer time limit for these labeling jobs in `TaskTimeLimitInSeconds` \(up to 7 days, or 604800 seconds\)\. 
**Important**  
If you set your take time limit to be greater than 8 hours, you must set `MaxSessionDuration` for your IAM execution role to at least 8 hours\. To see how to update this value for your IAM role, see [Modifying a Role ](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_modify.html) in the IAM User Guide, choose your preferred method to modify the role, and then follow the steps in [Modifying a Role Maximum Session Duration](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-managingrole-editing-console.html#roles-modify_max-session-duration)\. 

#### Create an Adjustment Labeling Job \(API\)<a name="sms-point-cloud-semantic-segmentation-adjustment-api"></a>

To create an adjustment labeling job, use the instructions in the previous section, with the following modifications: 
+ In your label category configuration file, you must include `auditLabelAttributeName`\. Use this parameter to input the [LabelAttributeName](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#sagemaker-CreateLabelingJob-request-LabelAttributeName) used in the labeling job that generated the annotations you want your worker to adjust\. 
**Important**  
When you create a labeling job in the console, if you did not specify a label category attribute name, the **Name** of your job is used as the LabelAttributeName\. 

  For example, if your label category attribute name was `point-cloud-labels` in your first labeling job, add the following to your semantic segmentation adjustment labeling job label category configuration file\. To learn how to create this file, see [Create a Labeling Category Configuration File with Label Category Attributes](sms-label-cat-config-attributes.md)\. 

  ```
  {
      "documentVersion": "2020-03-01",
      "labels": [
          {
              "label": "Car"
          },
          {
              "label": "Pedestrian"
          },
          {
              "label": "Cyclist"
          }
      ],
      "instructions": {"shortInstruction":"Select the appropriate label and paint all objects in the point cloud that it applies to the same color", "fullInstruction":"<html markup>"},
      // include auditLabelAttributeName for label adjustment jobs
      "auditLabelAttributeName": "point-cloud-label"
  }
  ```
+ You need to provide a pre\-defined ARNs for the pre\-annotation and post\-annotation \(ACS\) Lambda functions\. These ARNs are specific to the AWS Region you use to create your labeling job\. 
  + To find the pre\-annotation Lambda ARN, refer to [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HumanTaskConfig.html#sagemaker-Type-HumanTaskConfig-PreHumanTaskLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HumanTaskConfig.html#sagemaker-Type-HumanTaskConfig-PreHumanTaskLambdaArn)\. Use the Region you are creating your labeling job in to find the correct ARN that ends with `PRE-Adjustment3DPointCloudSemanticSegmentation`\.
  + To find the post\-annotation Lambda ARN, refer to [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AnnotationConsolidationConfig.html#sagemaker-Type-AnnotationConsolidationConfig-AnnotationConsolidationLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AnnotationConsolidationConfig.html#sagemaker-Type-AnnotationConsolidationConfig-AnnotationConsolidationLambdaArn)\. Use the Region you are creating your labeling job in to find the correct ARN that ends with `ACS-Adjustment3DPointCloudSemanticSegmentation`\.
+ The [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#SageMaker-CreateLabelingJob-request-LabelCategoryConfigS3Uri](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#SageMaker-CreateLabelingJob-request-LabelCategoryConfigS3Uri) parameter must contain the same label categories as the previous labeling job\. Adding new label categories or adjusting label categories is not supported\.

### Create a Labeling Job \(Console\)<a name="sms-point-cloud-semantic-segmentation-console"></a>

You can follow the instructions [Create a Labeling Job \(Console\)](sms-create-labeling-job-console.md) in order to learn how to create a 3D point cloud semantic segmentation labeling job in the SageMaker console\. While you are creating your labeling job, be aware of the following: 
+ Your input manifest file must be a single\-frame manifest file\. For more information, see [Create a Point Cloud Frame Input Manifest File](sms-point-cloud-single-frame-input-data.md)\. 
+ Automated data labeling and annotation consolidation are not supported for 3D point cloud labeling tasks\. 
+ 3D point cloud semantic segmentation labeling jobs can take multiple hours to complete\. You can specify a longer time limit for these labeling jobs when you select your work team \(up to 7 days, or 604800 seconds\)\. 
**Important**  
If you set your take time limit to be greater than 8 hours, you must set `MaxSessionDuration` for your IAM execution role to at least 8 hours\. To see how to update this value for your IAM role, see [Modifying a Role ](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_modify.html) in the IAM User Guide, choose your preferred method to modify the role, and then follow the steps in [Modifying a Role Maximum Session Duration](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-managingrole-editing-console.html#roles-modify_max-session-duration)\. 

#### Create an Adjustment Labeling Job \(console\)<a name="sms-point-cloud-semantic-seg-adjustment-console"></a>

You can create an adjustment labeling job in the console by *chaining* a successfully completed 3D point cloud semantic segmentation labeling job\. To learn more, see [Create and Start a Label Verification Job \(Console\)](sms-verification-data.md#sms-data-verify-start-console)\.

## Output Data Format<a name="sms-point-cloud-semantic-segmentation-input-data"></a>

When you create a 3D point cloud semantic segmentation labeling job, tasks are sent to workers\. When these workers complete their tasks, their annotations are written to the Amazon S3 bucket you specified when you created the labeling job\. The output data format determines what you see in your Amazon S3 bucket when your labeling job status \([LabelingJobStatus](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeLabelingJob.html#API_DescribeLabelingJob_ResponseSyntax)\) is `Completed`\. 

If you are a new user of Ground Truth, see [Output Data](sms-data-output.md) to learn more about the Ground Truth output data format\. To learn about the 3D point cloud object detection output data format, see [3D Point Cloud Semantic Segmentation Output](sms-data-output.md#sms-output-point-cloud-segmentation)\. 