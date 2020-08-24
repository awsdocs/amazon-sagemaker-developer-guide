# Label Video Frames<a name="sms-video-task-types"></a>

You can use Ground Truth built\-in video frame task types to have workers annotate video frames using bounding boxes\. A *video frame* is a sequence of images that have been extracted from a video\. A *bounding box* is a box that a worker draws around an object to identify the pixel location and label of that object in the frame\. 

If you do not have video frames, you can provide video files \(MP4 files\) and use the Ground Truth automated frame extraction tool to extract video frames\. To learn more, see [Provide Video Files](sms-point-cloud-video-input-data.md#sms-point-cloud-video-frame-extraction)\.

You can use the following built\-in video task types to create video frame labeling jobs using the Amazon SageMaker console, API, and language\-specific SDKs\.
+ **Video frame object detection** – Use this task type when you want workers to identify and locate objects in sequences of video frames using bounding boxes\. You provide a list of categories, and workers can select one category at a time and draw bounding boxes around objects to which the category applies in all frames\. For example, you can use this task to ask workers to identify and localize various objects in a scene, such as cars, bikes, and pedestrians\.
+ **Video frame object tracking** – Use this task type when you want workers to use bounding boxes to track the movement of instances of objects across sequences of video frames\. When a worker draws a new bounding box around an object or person in a single frame, that bounding box is associated with a unique instance ID\. The worker draws bounding boxes associated with the same ID in all other frames to identify the same object or person\. For example, a worker can track the movement of a vehicle across a sequences of video frames by drawing bounding boxes associated with the same ID around the vehicle in each frame that it appears\. 

Use the following topics to learn more about these built\-in task types and to how to create a labeling job using each task type\. Before you create a labeling job, we recommend that you review [Video Frame Labeling Job Overview](sms-video-overview.md)\.

**Topics**
+ [Video Frame Object Detection](sms-video-object-detection.md)
+ [Video Frame Object Tracking](sms-video-object-tracking.md)
+ [Video Frame Labeling Job Overview](sms-video-overview.md)