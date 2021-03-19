# Automated Data Setup<a name="sms-console-create-manifest-file"></a>

You can used the automated data setup to create manifest files for your labeling jobs in the Ground Truth console using images, videos, video frames, text \(\.txt\) files, and comma\-separated value \(\.csv\) files stored in Amazon S3\.

**Important**  
Ground Truth does not support automated data setup using AWS KMS encrypted buckets or objects in S3\.

Before using the following procedure, ensure that your input images or files are correctly formatted:
+ Image files – Image files must comply with the size and resolution limits listed in the tables found in [Input File Size Quota](input-data-limits.md#input-file-size-limit)\. 
+ Text files – Text data can be stored in one or more \.txt files\. Each item that you want labeled must be separated by a standard line break\. 
+ CSV files – Text data can be stored in one or more \.csv files\. Each item that you want labeled must be in a separate row\.
+ Videos – Video files can be any of the following formats: \.mp4, \.ogg, and \.webm\. If you want to extract video frames from your video files for object detection or object tracking, see [Provide Video Files](sms-point-cloud-video-input-data.md#sms-point-cloud-video-frame-extraction)\.
+ Video frames – Video frames are images extracted from a videos\. All images extracted from a single video are referred to as a *sequence of video frames*\. Each sequence of video frames must have unique prefix keys in Amazon S3\. See [Provide Video Frames](sms-point-cloud-video-input-data.md#sms-video-provide-frames)\. For this data type, see [Automated Video Frame Input Data Setup](sms-video-automated-data-setup.md)

**Important**  
For video frame object detection and video frame object tracking labeling jobs, see [Automated Video Frame Input Data Setup](sms-video-automated-data-setup.md) to learn how to use the automated data setup\. 

Use these instructions to automatically set up your input dataset connection with Ground Truth\.

**Automatically connect your data in Amazon S3 with Ground Truth**

1. Navigate to the **Create labeling job** page in the Amazon SageMaker console: [https://console.aws.amazon.com/sagemaker/groundtruth](https://console.aws.amazon.com/sagemaker/groundtruth)\. 

   This link puts you in the North Virginia \(us\-east\-1\) AWS Region\. If your input data is in an Amazon S3 bucket in another Region, switch to that Region\. To change your AWS Region, on the [navigation bar](https://docs.aws.amazon.com/awsconsolehelpdocs/latest/gsg/getting-started.html#select-region), choose the name of the currently displayed Region\.

1. Select **Create labeling job**\.

1. Enter a **Job name**\. 

1. In the section **Input data setup**, select **Automated data setup**\.

1. Enter an Amazon S3 URI for **S3 location for input datasets**\. 

1. Specify your **S3 location for output datasets**\. This is where your output data is stored\. 

1. Choose your **Data type** using the dropdown list\.

1. Use the drop down menu under **IAM Role** to select an execution role\. If you select **Create a new role**, specify the S3 buckets that you want grant this role permission to access\. This role must have permission to access the S3 buckets you specified in Steps 5 and 6\.

1. Select **Complete data setup**\.

This creates an input manifest in the Amazon S3 location for input datasets that you specified in step 5\. If you are creating a labeling job using the SageMaker API or, AWS CLI, or an AWS SDK, use the Amazon S3 URI for this input manifest file as input to the parameter `ManifestS3Uri`\. 

The following GIF demonstrates how to use the automated data setup for image data\. This example will create a file, `dataset-YYMMDDTHHMMSS.manifest` in the S3 bucket `example-groundtruth-images` where `YYMMDDTHHmmSS` indicates the year \(`YY`\), month \(`MM`\), day \(`DD`\) and time in hours \(`HH`\), minutes \(`mm`\) and seconds \(`ss`\), that the input manifest file was created\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/gifs/automated-data-setup.gif)