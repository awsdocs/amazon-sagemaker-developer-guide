# Video Frame Input Data<a name="sms-video-frame-input-data-overview"></a>

When you create a video frame object detection or object tracking labeling job, you can choose video files \(MP4 files\) or video frames for input data\. All worker tasks are created using video frames, so if you choose video files, use the Ground Truth frame extraction tool to extract video frames \(images\) from your video files\. 

For both of these options, you can use the **Automated data setup** option in the Ground Truth section of the Amazon SageMaker console to set up a connection between Ground Truth and your input data in Amazon S3 so that Ground Truth knows where to look for your input data when creating your labeling tasks\. This creates and stores an input manifest file in your Amazon S3 input dataset location\. To learn more, see [Automated Video Frame Input Data Setup](sms-video-automated-data-setup.md)\.

Alternatively, you can manually create sequence files for each sequence of video frames that you want labeled and provide the Amazon S3 location of an input manifest file that references each of these sequences files using the `source-ref` key\. To learn more, see [Create a Video Frame Input Manifest File](sms-video-manual-data-setup.md#sms-video-create-manifest)\. 

**Topics**
+ [Choose Video Files or Video Frames for Input Data](sms-point-cloud-video-input-data.md)
+ [Input Data Setup](sms-video-data-setup.md)