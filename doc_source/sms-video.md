# Label Videos and Video Frames<a name="sms-video"></a>

You can use Ground Truth to classify videos and annotate video frames \(still images extracted from videos\) using one of the three built\-in video task types\. These task types streamline the process of creating video and video frame labeling jobs using the Amazon SageMaker console, API, and language\-specific SDKs\. 
+ Video clip classification – Enable workers to classify videos into categories you specify\. For example, you can use this task type to have workers categorize videos into topics like sports, comedy, music, and education\. To learn more, see [Video Classification](sms-video-classification.md)\.
+ Video frame labeling jobs – Enable workers to annotate video frames extracted from a video using bounding boxes\. Ground Truth offers two built\-in task types to label video frames:
  + Enable workers to identify and locate objects in video frames using *video frame object detection*\. 
  + Enable workers to track the movement of objects across video frames using *video frame object tracking*\.

  If you have video files, you can use the Ground Truth automatic frame extraction tool to extract video frames from your videos\. 

  To learn more, see [Label Video Frames](sms-video-task-types.md)\.

**Topics**
+ [Video Classification](sms-video-classification.md)
+ [Label Video Frames](sms-video-task-types.md)
+ [Worker Instructions](sms-video-worker-instructions.md)