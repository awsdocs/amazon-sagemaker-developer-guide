# Label Video Frames<a name="sms-video-task-types"></a>

You can use Ground Truth built\-in video frame task types to have workers annotate video frames using bounding boxes, polylines, polygons or keypoints\. A *video frame* is a sequence of images that have been extracted from a video\.

If you do not have video frames, you can provide video files \(MP4 files\) and use the Ground Truth automated frame extraction tool to extract video frames\. To learn more, see [Provide Video Files](sms-point-cloud-video-input-data.md#sms-point-cloud-video-frame-extraction)\.

You can use the following built\-in video task types to create video frame labeling jobs using the Amazon SageMaker console, API, and language\-specific SDKs\.
+ **Video frame object detection** – Use this task type when you want workers to identify and locate objects in sequences of video frames\. You provide a list of categories, and workers can select one category at a time and annotate objects which the category applies to in all frames\. For example, you can use this task to ask workers to identify and localize various objects in a scene, such as cars, bikes, and pedestrians\.
+ **Video frame object tracking** – Use this task type when you want workers to track the movement of instances of objects across sequences of video frames\. When a worker adds an annotation to a single frame, that annotation is associated with a unique instance ID\. The worker adds annotations associated with the same ID in all other frames to identify the same object or person\. For example, a worker can track the movement of a vehicle across a sequences of video frames by drawing bounding boxes associated with the same ID around the vehicle in each frame that it appears\. 

Use the following topics to learn more about these built\-in task types and to how to create a labeling job using each task type\. See [Task Types](sms-video-overview.md#sms-video-frame-tools) to learn more about the annotations tools \(bounding boxes, polylines, polygons and keypoints\) available for these task types\.

Before you create a labeling job, we recommend that you review [Video Frame Labeling Job Overview](sms-video-overview.md)\.

**Topics**
+ [Video Frame Object Detection](sms-video-object-detection.md)
+ [Video Frame Object Tracking](sms-video-object-tracking.md)
+ [Video Frame Labeling Job Overview](sms-video-overview.md)