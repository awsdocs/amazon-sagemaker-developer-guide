# Choose Video Files or Video Frames for Input Data<a name="sms-point-cloud-video-input-data"></a>

When you create a video frame object detection or object tracking labeling job, you can provide a sequence of video frames \(images\) or you can use the Amazon SageMaker console to have Ground Truth automatically extract video frames from your video files\. Use the following sections to learn more about these options\. 

## Provide Video Frames<a name="sms-video-provide-frames"></a>

Video frames are sequences of images extracted from a video file\. You can create a Ground Truth labeling job to have workers label multiple sequences of video frames\. Each sequence is made up of images extracted from a single video\. 

To create a labeling job using video frame sequences, you must store each sequence using a unique [key name prefix](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingMetadata.html#object-keys) in Amazon S3\. In the Amazon S3 console, key name prefixes are folders\. So in the Amazon S3 console, each sequence of video frames must be located in its own folder in Amazon S3\.

For example, if you have two sequences of video frames, you might use the key name prefixes `sequence1/` and `sequence2/` to identify your sequences\. In this example, your sequences may be located in `s3://DOC-EXAMPLE-BUCKET/video-frames/sequence1/` and `s3://DOC-EXAMPLE-BUCKET/video-frames/sequence2/`\.

If you are using the Ground Truth console to create an input manifest file, all of the sequence key name prefixes should be in the same location in Amazon S3\. For example, in the Amazon S3 console, each sequence could be in a folder in `s3://DOC-EXAMPLE-BUCKET/video-frames/`\. In this example, your first sequence of video frames \(images\) may be located in `s3://DOC-EXAMPLE-BUCKET/video-frames/sequence1/` and your second sequence may be located in `s3://DOC-EXAMPLE-BUCKET/video-frames/sequence2/`\. 

**Important**  
Even if you only have a single sequence of video frames that you want workers to label, that sequence must have a key name prefix in Amazon S3\. If you are using the Amazon S3 console, this means that your sequence is located in a folder\. It cannot be located in the root of your S3 bucket\. 

When creating worker tasks using sequences of video frames, Ground Truth uses one sequence per task\. In each task, Ground Truth orders your video frames using [UTF\-8](https://en.wikipedia.org/wiki/UTF-8) binary order\. 

For example, video frames might be in the following order in Amazon S3: 

```
[0001.jpg, 0002.jpg, 0003.jpg, ..., 0011.jpg]
```

They are arranged in the same order in the worker’s task: `0001.jpg, 0002.jpg, 0003.jpg, ..., 0011.jpg`\.

Frames might also be ordered using a naming convention like the following:

```
[frame1.jpg, frame2.jpg, ..., frame11.jpg]
```

In this case, `frame10.jpg` and `frame11.jpg` come before `frame2.jpg` in the worker task\. Your worker sees your video frames in the following order: `frame1.jpg, frame10.jpg, frame11.jpg, frame2.jpg, ..., frame9.jpg`\. 

## Provide Video Files<a name="sms-point-cloud-video-frame-extraction"></a>

You can use the Ground Truth frame splitting feature when creating a new labeling job in the console to extract video frames from video files \(MP4 files\)\. A series of video frames extracted from a single video file is referred to as a *sequence of video frames*\. 

You can either have Ground Truth automatically extract all frames, up to 2,000, from the video, or you can specify a frequency for frame extraction\. For example, you can have Ground Truth extract every 10th frame from your videos\.

To use the video frame extraction tool, see [Automated Video Frame Input Data Setup](sms-video-automated-data-setup.md)\. 

When all of your video frames have been successfully extracted from your videos, you will see the following in your S3 input dataset location:
+ A key name prefix \(a folder in the Amazon S3 console\) named after each video\. Each of these prefixes leads to:
  + A sequence of video frames extracted from the video used to name that prefix\.
  + A sequence file used to identify all of the images that make up that sequence\. 
+ An input manifest file with a \.manifest extension\. This identifies all of the sequence files that will be used to create your labeling job\. 

All of the frames extracted from a single video file are used for a labeling task\. If you extract video frames from multiple video files, multiple tasks are created for your labeling job, one for each sequence of video frames\. 

 Ground Truth stores each sequence of video frames that it extracts in your Amazon S3 location for input datasets using a unique [key name prefix](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingMetadata.html#object-keys)\. In the Amazon S3 console, key name prefixes are folders\.