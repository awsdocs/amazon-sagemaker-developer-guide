# Automated Video Frame Input Data Setup<a name="sms-video-automated-data-setup"></a>

Use these instructions to automatically set up your input dataset connect with Ground Truth\. To use the automated data setup option, your video files or sequences of video frames must be stored in Amazon S3\. This procedure will create an input manifest file in your Amazon S3 bucket\. 

**Automatically connect your data in Amazon S3 with Ground Truth**

1. Navigate to the **Create labeling job** page in the Amazon SageMaker console: [https://console.aws.amazon.com/sagemaker/groundtruth](https://console.aws.amazon.com/sagemaker/groundtruth)\. 

   Your input and output S3 buckets must be located in the same AWS Region that you create your labeling job\. This link puts you in the North Virginia \(us\-east\-1\) AWS Region\. If your input data is in an Amazon S3 bucket in another Region, switch to that Region\. To change your AWS Region, on the [navigation bar](https://docs.aws.amazon.com/awsconsolehelpdocs/latest/gsg/getting-started.html#select-region), choose the name of the currently displayed Region\.

1. Select **Create labeling job**\.

1. Enter a **Job name**\. 

1. In the section **Input data setup**, select **Automated data setup**\.

1. Enter an Amazon S3 URI for **S3 location for input datasets**\. 

   If your input dataset is made up of sequences of *sequences of video frames*, this should be the Amazon S3 location where your sequences are stored\. Each sequence should have a unique prefix\. For example, if you have two sequences stored in `s3://DOC-EXAMPLE-BUCKET/vid-frames/sequences/sequence1`, `s3://DOC-EXAMPLE-BUCKET/vid-frames/sequences/sequence2`, enter `s3://DOC-EXAMPLE-BUCKET/vid-frames/sequences/` here\. 

   If your input dataset is made up of *video files* such as MP4 files, this should be the Amazon S3 location in which your video files are stored\. 

1. Specify your **S3 location for output datasets**\. This is where your output data is stored\. 

1. Choose **Video \- individual frames** for your **Data type** using the dropdown list\. Do *not* choose Video \- full clip\. 

1. If your input dataset is made up of *sequences of video frames*, choose **Use existing video frames in S3**\. 

   If your input dataset is made up of *video files* choose **Extract frames from video**\. Select one of the following options for **Frame extraction**:
   + When you choose **Use all frames extracted from the video to create a labeling task**, Ground Truth extracts all frames from each video in your **S3 location for input datasets**, up to 2,000 frames\. If a video in your input dataset contains more than 2,000 frames, the first 2,000 are extracted and used for that labeling task\. 
   + When you choose **Use every *x* frame from a video to create a labeling task**, Ground Truth extracts every *x*th frame from each video in your **S3 location for input datasets**\. 

     For example, if your video is 2 seconds long, and has a [frame rate](https://en.wikipedia.org/wiki/Frame_rate) of 30 frames per second, there are 60 frames in your video\. If you specify 10 here, Ground Truth extracts every 10th frame from your video\. This means the 1st, 10th, 20th, 30th, 40th, 50th, and 60th frames are extracted\. 

1. Choose or create an IAM execution role\. 

1. Select **Complete data setup**\.

This creates an input manifest in the Amazon S3 location for input datasets that you specified in step 5\. If you are creating a labeling job using the SageMaker API or, AWS CLI, or an AWS SDK, use the Amazon S3 URI for this input manifest file as input to the parameter `ManifestS3Uri`\. 