# Automated Video Frame Input Data Setup<a name="sms-video-automated-data-setup"></a>

You can use the Ground Truth automated data setup to automatically detect video files in your Amazon S3 bucket and extract video frames from those files\. To learn how, see [Provide Video Files](sms-point-cloud-video-input-data.md#sms-point-cloud-video-frame-extraction)\.

If you already have video frames in Amazon S3, you can use the automated data setup to use these video frames in your labeling job\. For this option, all video frames from a single video must be stored using a unique prefix\. To learn about the requirements to use this option, see [Provide Video Frames](sms-point-cloud-video-input-data.md#sms-video-provide-frames)\.

Select one of the following sections to learn how to set up your automatic input dataset connection with Ground Truth\.

## Provide Video Files and Extract Frames<a name="sms-video-provide-files-auto-setup-console"></a>

Use the following procedure to connect your video files with Ground Truth and automatically extract video frames from those files for video frame object detection and object tracking labeling jobs\.

**Note**  
If you use the automated data setup console tool to extract video frames from more than 10 video files, you will need to modify the manifest file the tool generates or create a new one to include 10 video frame sequence files or less\. To learn more, see [Provide Video Files](sms-point-cloud-video-input-data.md#sms-point-cloud-video-frame-extraction)\.

Make sure your video files are stored in an Amazon S3 bucket in the same AWS Region that you perform the automated data setup in\. 

**Automatically connect your video files in Amazon S3 with Ground Truth and extract video frames:**

1. Navigate to the **Create labeling job** page in the Amazon SageMaker console: [https://console\.aws\.amazon\.com/sagemaker/groundtruth](https://console.aws.amazon.com/sagemaker/groundtruth)\. 

   Your input and output S3 buckets must be located in the same AWS Region that you create your labeling job in\. This link puts you in the North Virginia \(us\-east\-1\) AWS Region\. If your input data is in an Amazon S3 bucket in another Region, switch to that Region\. To change your AWS Region, on the [navigation bar](https://docs.aws.amazon.com/awsconsolehelpdocs/latest/gsg/getting-started.html#select-region), choose the name of the currently displayed Region\.

1. Select **Create labeling job**\.

1. Enter a **Job name**\. 

1. In the section **Input data setup**, select **Automated data setup**\.

1. Enter an Amazon S3 URI for **S3 location for input datasets**\. An S3 URI looks like the following: `s3://DOC-EXAMPLE-BUCKET/path-to-files/`\. This URI should point to the Amazon S3 location where your video files are stored\.

1. Specify your **S3 location for output datasets**\. This is where your output data is stored\. You can choose to store your output data in the **Same location as input dataset** or **Specify a new location** and entering the S3 URI of the location that you want to store your output data\.

1. Choose **Video Files** for your **Data type** using the dropdown list\.

1. Choose **Yes, extract frames for object tracking and detection tasks**\. 

1. Choose a method of **Frame extraction**\.
   + When you choose **Use all frames extracted from the video to create a labeling task**, Ground Truth extracts all frames from each video in your **S3 location for input datasets**, up to 2,000 frames\. If a video in your input dataset contains more than 2,000 frames, the first 2,000 are extracted and used for that labeling task\. 
   + When you choose **Use every *x* frame from a video to create a labeling task**, Ground Truth extracts every *x*th frame from each video in your **S3 location for input datasets**\. 

     For example, if your video is 2 seconds long, and has a [frame rate](https://en.wikipedia.org/wiki/Frame_rate) of 30 frames per second, there are 60 frames in your video\. If you specify 10 here, Ground Truth extracts every 10th frame from your video\. This means the 1st, 10th, 20th, 30th, 40th, 50th, and 60th frames are extracted\. 

1. Choose or create an IAM execution role\. Make sure that this role has permission to access your Amazon S3 locations for input and output data specified in steps 5 and 6\. 

1. Select **Complete data setup**\.

## Provide Video Frames<a name="sms-video-provide-frames-auto-setup-console"></a>

Use the following procedure to connect your sequences of video frames with Ground Truth for video frame object detection and object tracking labeling jobs\. 

Make sure your video frames are stored in an Amazon S3 bucket in the same AWS Region that you perform the automated data setup in\. Each sequence of video frames should have a unique prefix\. For example, if you have two sequences stored in `s3://DOC-EXAMPLE-BUCKET/video-frames/sequences/`, each should have a unique prefix like `sequence1` and `sequence2` and should both be located directly under the `/sequences/` prefix\. In the example above, the locations of these two sequences is: `s3://DOC-EXAMPLE-BUCKET/video-frames/sequences/sequence1/` and `s3://DOC-EXAMPLE-BUCKET/video-frames/sequences/sequence2/`\. 

**Automatically connect your video frame in Amazon S3 with Ground Truth:**

1. Navigate to the **Create labeling job** page in the Amazon SageMaker console: [https://console\.aws\.amazon\.com/sagemaker/groundtruth](https://console.aws.amazon.com/sagemaker/groundtruth)\. 

   Your input and output S3 buckets must be located in the same AWS Region that you create your labeling job in\. This link puts you in the North Virginia \(us\-east\-1\) AWS Region\. If your input data is in an Amazon S3 bucket in another Region, switch to that Region\. To change your AWS Region, on the [navigation bar](https://docs.aws.amazon.com/awsconsolehelpdocs/latest/gsg/getting-started.html#select-region), choose the name of the currently displayed Region\.

1. Select **Create labeling job**\.

1. Enter a **Job name**\. 

1. In the section **Input data setup**, select **Automated data setup**\.

1. Enter an Amazon S3 URI for **S3 location for input datasets**\. 

   This should be the Amazon S3 location where your sequences are stored\. For example, if you have two sequences stored in `s3://DOC-EXAMPLE-BUCKET/video-frames/sequences/sequence1/`, `s3://DOC-EXAMPLE-BUCKET/video-frames/sequences/sequence2/`, enter `s3://DOC-EXAMPLE-BUCKET/video-frames/sequences/` here\.

1. Specify your **S3 location for output datasets**\. This is where your output data is stored\. You can choose to store your output data in the **Same location as input dataset** or **Specify a new location** and entering the S3 URI of the location that you want to store your output data\.

1. Choose **Video frames** for your **Data type** using the dropdown list\. 

1. Choose or create an IAM execution role\. Make sure that this role has permission to access your Amazon S3 locations for input and output data specified in steps 5 and 6\. 

1. Select **Complete data setup**\.

These procedures will create an input manifest in the Amazon S3 location for input datasets that you specified in step 5\. If you are creating a labeling job using the SageMaker API or, AWS CLI, or an AWS SDK, use the Amazon S3 URI for this input manifest file as input to the parameter `ManifestS3Uri`\.