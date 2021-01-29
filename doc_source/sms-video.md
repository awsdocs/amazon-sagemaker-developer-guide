# Label Videos and Video Frames<a name="sms-video"></a>

You can use Ground Truth to classify videos and annotate video frames \(still images extracted from videos\) using one of the three built\-in video task types\. These task types streamline the process of creating video and video frame labeling jobs using the Amazon SageMaker console, API, and language\-specific SDKs\. 
+ Video clip classification – Enable workers to classify videos into categories you specify\. For example, you can use this task type to have workers categorize videos into topics like sports, comedy, music, and education\. To learn more, see [Video Classification](sms-video-classification.md)\.
+ Video frame labeling jobs – Enable workers to annotate video frames extracted from a video using bounding boxes, polylines, polygons or keypoint annotation tools\. Ground Truth offers two built\-in task types to label video frames:
  + *Video frame object detection*: Enable workers to identify and locate objects in video frames\. 
  + *Video frame object tracking*: Enable workers to track the movement of objects across video frames\.
  + *Video frame adjustment jobs*: Have works adjust labels, label category attributes, and frame attributes from a previous video frame object detection or object tracking labeling job\.
  + *Video frame verification jobs*: Have workers verify labels, label category attributes, and frame attributes from a previous video frame object detection or object tracking labeling job\.

  If you have video files, you can use the Ground Truth automatic frame extraction tool to extract video frames from your videos\.

  To learn more, see [Label Video Frames](sms-video-task-types.md)\.

**Topics**
+ [Video Classification](sms-video-classification.md)
+ [Label Video Frames](sms-video-task-types.md)
+ [Worker Instructions](sms-video-worker-instructions.md)