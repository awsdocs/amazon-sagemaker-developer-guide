# 3D\-2D Point Cloud Object Tracking<a name="sms-point-cloud-3d-2d-object-tracking"></a>

Use this task type when you want workers to link 3D point cloud annotations with 2D images annotations and also link 2D image annotations among various cameras\. Currently, Ground Truth supports cuboids for annotation in a 3D point cloud and bounding boxes for annotation in 2D videos\. For example, you can use this task type to ask workers to link the movement of a vehicle in 3D point cloud with its 2D video\. Using 3D\-2D linking, you can easily correlate point cloud data \(like the distance of a cuboid\) to video data \(bounding box\) for up to 8 cameras\.

 Ground Truth provides workers with tools to annotate cuboids in a 3D point cloud and bounding boxes in up to 8 cameras using the same annotation UI\. Workers can also link various bounding boxes for the same object across different cameras\. For example, a bounding box in camera1 can be linked to a bounding box in camera2\. This lets you to correlate an object across multiple cameras using a unique ID\. 

**Note**  
Currently, SageMaker does not support creating a 3D\-2D linking job using the console\. To create a 3D\-2D linking job using the SageMaker API, see [Create a Labeling Job \(API\)](#sms-point-cloud-3d-2d-object-tracking-create-labeling-job-api)\. 

**Topics**
+ [View the Worker Task Interface](#sms-point-cloud-3d-2d-object-tracking-worker-ui)
+ [Input Data Format](#sms-point-cloud-3d-2d-object-tracking-input-data)
+ [Create a 3D\-2D Point Cloud Object Tracking Labeling Job](#sms-3d-2d-point-cloud-object-tracking-create-labeling-job)
+ [Output Data](#sms-point-cloud-3d-2d-object-tracking-output-data)

## View the Worker Task Interface<a name="sms-point-cloud-3d-2d-object-tracking-worker-ui"></a>

Ground Truth provides workers with a web portal and tools to complete your 3D\-2D object tracking annotation tasks\. When you create the labeling job, you provide the Amazon Resource Name \(ARN\) for a pre\-built Ground Truth UI in the `HumanTaskUiArn` parameter\. To use the UI when you create a labeling job for this task type using the API, you need to provide the `HumanTaskUiArn`\. You can preview and interact with the worker UI when you create a labeling job through the API\. The annotating tools are a part of the worker task interface\. They are not available for the preview interface\. The following image demonstrates the worker task interface used for the 3D\-2D point cloud object tracking annotation task\.

![\[The worker task interface used for the 3D-2D point cloud object tracking annotation task.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms-sensor-fusion.png)

Once workers add a single cuboid, that cuboid is replicated in all frames of the sequence with the same ID\. Once workers adjust the cuboid in another frame, Ground Truth interpolates the movement of that object and adjust all cuboids between the manually adjusted frames\. The accuracy of the cuboid to image projection is based on accuracy of calibrations captured in the extrinsics and intrinsics data\.

If you provide camera data for sensor fusion, images are matched up with scenes in point cloud frames\. Note that the camera data should be closely synchronized with the point cloud data\. The manifest file holds the extrinsics and intrinsics and pose to allow the cuboid projection on the camera image to be shown\.

Worker can navigate in the 3D scene using their keyboard and mouse\. They can:
+ Double click on specific objects in the point cloud to zoom into them\.
+ Use a mouse\-scroller or trackpad to zoom in and out of the point cloud\.
+ Use both keyboard arrow keys and Q, E, A, and D keys to move Up, Down, Left, Right\. Use keyboard keys W and S to zoom in and out\. 

Once a worker places a cuboids in the 3D scene, a side\-view appears with the three projected side views: top, side, and front\. These side\-views show points in and around the placed cuboid and help workers refine cuboid boundaries in that area\. Workers can zoom in and out of each of those side\-views using their mouse\.

The worker should first select the cuboid to draw a corresponding bounding box on any of the camera views\. This links the cuboid and the bounding box with a common name and unique ID\.

The worker can also first draw a bounding box, select it and draw the corresponding cuboid to link them\.

Additional view options and features are available\. See the [worker instruction page](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-point-cloud-worker-instructions-object-tracking.html) for a comprehensive overview of the Worker UI\. 

### Worker Tools<a name="sms-point-cloud-object-tracking-worker-tools"></a>

Workers can navigate through the 3D point cloud by zooming in and out, and moving in all directions around the cloud using the mouse and keyboard shortcuts\. If workers click on a point in the point cloud, the UI automatically zooms into that area\. Workers can use various tools to draw 3D cuboid around objects\. For more information, see **Assistive Labeling Tools** in the following discussion\. 

After workers have placed a 3D cuboid in the point cloud, they can adjust these cuboids to fit tightly around cars using a variety of views: directly in the 3D point cloud, in a side\-view featuring three zoomed\-in perspectives of the point cloud around the box, and if you include images for sensor fusion, directly in the 2D image\. 

Additional view options enable workers to easily hide or view label text, a ground mesh, and additional point attributes\. Workers can also choose between perspective and orthogonal projections\. 

**Assistive Labeling Tools**  
Ground Truth helps workers annotate 3D point clouds faster and more accurately using UX, machine learning and computer vision powered assistive labeling tools for 3D point cloud object tracking tasks\. The following assistive labeling tools are available for this task type:
+ **Label autofill** – When a worker adds a cuboid to a frame, a cuboid with the same dimensions, orientation and xyz position is automatically added to all frames in the sequence\. 
+ **Label interpolation** – After a worker has labeled a single object in two frames, Ground Truth uses those annotations to interpolate the movement of that object between all the frames\. Label interpolation can be turned on and off\. It is on by default\. For example, if a worker working with 5 frames adds a cuboid in frame 2, it is copied to all the 5 frames\. If the worker then makes adjustments in frame 4, frame 2 and 4 now act as two points, through which a line is fit\. The cuboid is then interpolated in frames 1,3 and 5\.
+ **Bulk label and attribute management** – Workers can add, delete, and rename annotations, label category attributes, and frame attributes in bulk\.
  + Workers can manually delete annotations for a given object before and after a frame, or in all frames\. For example, a worker can delete all labels for an object after frame 10 if that object is no longer located in the scene after that frame\. 
  + If a worker accidentally bulk deletes all annotations for a object, they can add them back\. For example, if a worker deletes all annotations for an object before frame 100, they can bulk add them to those frames\. 
  + Workers can rename a label in one frame and all 3D cuboids assigned that label are updated with the new name across all frames\. 
  + Workers can use bulk editing to add or edit label category attributes and frame attributes in multiple frames\.
+ **Snapping** – Workers can add a cuboid around an object and use a keyboard shortcut or menu option to have Ground Truth's autofit tool snap the cuboid tightly around the object's boundaries\. 
+ **Fit to ground** – After a worker adds a cuboid to the 3D scene, the worker can automatically snap the cuboid to the ground\. For example, the worker can use this feature to snap a cuboid to the road or sidewalk in the scene\. 
+ **Multi\-view labeling** – After a worker adds a 3D cuboid to the 3D scene, a side\-panel displays front and two side perspectives to help the worker adjust the cuboid tightly around the object\. Workers can annotation the 3D point cloud, the side panel and the adjustments appear in the other views in real time\. 
+ **Sensor fusion** – If you provide data for sensor fusion, workers can adjust annotations in the 3D scenes and in 2D images, and the annotations are projected into the other view in real time\. To learn more about the data for sensor fusion, see [Understand Coordinate Systems and Sensor Fusion](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-point-cloud-sensor-fusion-details.html#sms-point-cloud-sensor-fusion)\.
+ **Auto\-merge cuboids **– Workers can automatically merge two cuboids across all frames if they determine that cuboids with different labels actually represent a single object\. 
+ **View options **– Enables workers to easily hide or view label text, a ground mesh, and additional point attributes like color or intensity\. Workers can also choose between perspective and orthogonal projections\. 

## Input Data Format<a name="sms-point-cloud-3d-2d-object-tracking-input-data"></a>

You can create a 3D\-2D object tracking job using the SageMaker API operation, [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html)\. To create a labeling job for this task type you need the following:
+ A sequence input manifest file\. To learn how to create this type of manifest file, see [Create a Point Cloud Sequence Input Manifest](sms-point-cloud-multi-frame-input-data.md)\. If you are a new user of Ground Truth 3D point cloud labeling modalities, we recommend that you review [Accepted Raw 3D Data Formats](sms-point-cloud-raw-data-types.md)\. 
+ You specify your labels, label category and frame attributes, and worker instructions in a label category configuration file\. For more information, see [Create a Labeling Category Configuration File with Label Category and Frame Attributes](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-label-cat-config-attributes.html) to learn how to create this file\. The following is an example showing a label category configuration file for creating a 3D\-2D object tracking job\.

  ```
  {
      "document-version": "2020-03-01",
      "categoryGlobalAttributes": [
          {
              "name": "Occlusion",
              "description": "global attribute that applies to all label categories",
              "type": "string",
              "enum":[
                  "Partial",
                  "Full"
              ]
          }
      ],
      "labels":[
          {
              "label": "Car",
              "attributes": [
                  {
                      "name": "Type",
                      "type": "string",
                      "enum": [
                          "SUV",
                          "Sedan"
                      ]
                  } 
              ]
          },
          {
              "label": "Bus",
              "attributes": [
                  {
                      "name": "Size",
                      "type": "string",
                      "enum": [
                          "Large",
                          "Medium",
                          "Small"
                      ]
                  }
              ]
          }
      ],
      "instructions": {
          "shortIntroduction": "Draw a tight cuboid around objects after you select a category.",
          "fullIntroduction": "<p>Use this area to add more detailed worker instructions.</p>"
      },
      "annotationType": [
          {
              "type": "BoundingBox"
          },
          {
              "type": "Cuboid"
          }
      ]
  }
  ```
**Note**  
You need to provide `BoundingBox` and `Cuboid` as annotationType in the label category configuration file to create a 3D\-2D object tracking job\. 

## Create a 3D\-2D Point Cloud Object Tracking Labeling Job<a name="sms-3d-2d-point-cloud-object-tracking-create-labeling-job"></a>

You can create a 3D\-2D point cloud labeling job using the SageMaker API operation, [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html)\. To create a labeling job for this task type you need the following: 
+ A work team from a private or vendor workforce\. You cannot use Amazon Mechanical Turk for 3D point cloud labeling jobs\. To learn how to create workforces and work teams, see [Create and Manage Workforces](sms-workforce-management.md)\.
+ Add a CORS policy to an S3 bucket that contains input data in the Amazon S3 console\. To set the required CORS headers on the S3 bucket that contains your input images in the S3 console, follow the directions detailed in [CORS Permission Requirement](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-cors-update.html)\.
+ Additionally, make sure that you have reviewed and satisfied the [Assign IAM Permissions to Use Ground Truth](sms-security-permission.md)\. 

To learn how to create a labeling job using the API, see the following sections\. 

### Create a Labeling Job \(API\)<a name="sms-point-cloud-3d-2d-object-tracking-create-labeling-job-api"></a>

This section covers details you need to know when you create a 3D\-2D object tracking labeling job using the SageMaker API operation `CreateLabelingJob`\. This API defines this operation for all AWS SDKs\. To see a list of language\-specific SDKs supported for this operation, review the **See Also** section of [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html)\. 

[Create a Labeling Job \(API\)](sms-create-labeling-job-api.md) provides an overview of the `CreateLabelingJob` operation\. Follow these instructions and do the following while you configure your request: 
+ You must enter an ARN for `HumanTaskUiArn`\. Use `arn:aws:sagemaker:<region>:394669845002:human-task-ui/PointCloudObjectTracking`\. Replace `<region>` with the AWS Region you are creating the labeling job in\. 

  There should not be an entry for the `UiTemplateS3Uri` parameter\. 
+ Your [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#sagemaker-CreateLabelingJob-request-LabelAttributeName](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#sagemaker-CreateLabelingJob-request-LabelAttributeName) must end in `-ref`\. For example, `ot-labels-ref`\. 
+ Your input manifest file must be a point cloud frame sequence manifest file\. For more information, see [Create a Point Cloud Sequence Input Manifest](sms-point-cloud-multi-frame-input-data.md)\. You also need to provide a label category configuration file as mentioned above\.
+ You need to provide pre\-defined ARNs for the pre\-annotation and post\-annotation \(ACS\) Lambda functions\. These ARNs are specific to the AWS Region you use to create your labeling job\. 
  + To find the pre\-annotation Lambda ARN, refer to [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HumanTaskConfig.html#sagemaker-Type-HumanTaskConfig-PreHumanTaskLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HumanTaskConfig.html#sagemaker-Type-HumanTaskConfig-PreHumanTaskLambdaArn)\. Use the Region you are creating your labeling job in to find the correct ARN that ends with `PRE-3DPointCloudObjectTracking`\. 
  + To find the post\-annotation Lambda ARN, refer to [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AnnotationConsolidationConfig.html#sagemaker-Type-AnnotationConsolidationConfig-AnnotationConsolidationLambdaArn](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AnnotationConsolidationConfig.html#sagemaker-Type-AnnotationConsolidationConfig-AnnotationConsolidationLambdaArn)\. Use the Region you are creating your labeling job in to find the correct ARN that ends with `ACS-3DPointCloudObjectTracking`\. 
+ The number of workers specified in `NumberOfHumanWorkersPerDataObject` should be `1`\. 
+ Automated data labeling is not supported for 3D point cloud labeling jobs\. You should not specify values for parameters in `[LabelingJobAlgorithmsConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#sagemaker-CreateLabelingJob-request-LabelingJobAlgorithmsConfig)`\. 
+ 3D\-2D object tracking labeling jobs can take multiple hours to complete\. You can specify a longer time limit for these labeling jobs in `TaskTimeLimitInSeconds` \(up to 7 days, or 604,800 seconds\)\. 

**Note**  
After you have successfully created a 3D\-2D object tracking job, it shows up on the console under labeling jobs\. The task type for the job is displayed as **Point Cloud Object Tracking**\.

## Output Data<a name="sms-point-cloud-3d-2d-object-tracking-output-data"></a>

When you create a 3D\-2D object tracking labeling job, tasks are sent to workers\. When these workers complete their tasks, their annotations are written to the Amazon S3 bucket you specified when you created the labeling job\. The output data format determines what you see in your Amazon S3 bucket when your labeling job status \([LabelingJobStatus](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeLabelingJob.html#API_DescribeLabelingJob_ResponseSyntax)\) is `Completed`\. 

If you are a new user of Ground Truth, see [Output Data](sms-data-output.md) to learn more about the Ground Truth output data format\. To learn about the 3D\-2D point cloud object tracking output data format, see [3D\-2D Object Tracking Point Cloud Object Tracking Output](sms-data-output.md#sms-output-3d-2d-point-cloud-object-tracking)\. 