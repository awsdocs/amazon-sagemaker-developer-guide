# Manual Input Data Setup<a name="sms-video-manual-data-setup"></a>

Choose the manual data setup option if you have created sequence files for each of your video frame sequences, and a manifest file listing references to those sequences files\.

## Create a Video Frame Input Manifest File<a name="sms-video-create-manifest"></a>

 Ground Truth uses the input manifest file to identify the location of your input dataset when creating labeling tasks\. For video frame object detection and object tracking labeling jobs, each line in the input manifest file identifies the location of a video frame sequence file\. Each sequence file identifies the images included in a single sequence of video frames\.

Use this page to learn how to create a video frame sequence file and an input manifest file for video frame object tracking and object detection labeling jobs\.

If you want Ground Truth to automatically generate your sequence files and input manifest file, see [Automated Video Frame Input Data Setup](sms-video-automated-data-setup.md)\. 

### Create a Video Frame Sequence Input Manifest<a name="sms-video-create-input-manifest-file"></a>

In the video frame sequence input manifest file, each line in the manifest references a sequence file that identifies the location of a sequence of video frames\. This is the manifest file formatting required for all video frame labeling jobs\. 

The following example demonstrates the syntax used for an input manifest file:

```
{"source-ref": "s3://DOC-EXAMPLE-BUCKET/example-folder/seq1.json"}
{"source-ref": "s3://DOC-EXAMPLE-BUCKET/example-folder/seq2.json"}
```

### Create a Video Frame Sequence File<a name="sms-video-create-sequence-file"></a>

The data for each sequence of video frames needs to be stored in a JSON data object\. The following is an example of the format you use for a sequence file\. Information about each frame is included as a JSON object and is listed in the `frames` list\.

```
{
 "seq-no": 1,
 "prefix": "s3://mybucket/prefix/video1",
 "number-of-frames": 3,
 "frames":[
   {"frame-no": 1, "unix-timestamp": 1566861644, "frame": "frame0001.jpg" },
   {"frame-no": 2, "unix-timestamp": 1566861644, "frame": "frame0002.jpg" }, 
   {"frame-no": 3, "unix-timestamp": 1566861644, "frame": "frame0003.jpg" }   
 ]
}
```

The following table provides details about the parameters shown in the this code example\. 


****  

|  Parameter  |  Required  |  Accepted Values  |  Description  | 
| --- | --- | --- | --- | 
|  `seq-no`  |  Yes  |  Integer  |  The ordered number of the sequence\.   | 
|  `prefix`  |  Yes  |  String **Accepted Values**: `s3://<bucket-name>/<prefix>/`  |  The Amazon S3 location where the sequence files are located\.  The prefix must end with a forward slash: `/`\.  | 
|  `number-of-frames`  |  Yes  |  Integer  |  The total number of frames included in the sequence file\. This number must match the total number of frames listed in the `frames` parameter in the next row\.  | 
|  `frames`  |  Yes  |  List of JSON objects **Required**: `frame-no`, `frame` **Optional**: `unix-timestamp`  |  A list of frame data\. The length of the list must equal `number-of-frames`\. In the worker UI, frames in a sequence are ordered in [UTF\-8](https://en.wikipedia.org/wiki/UTF-8) binary order\. To learn more about this ordering, see [Provide Video Frames](sms-point-cloud-video-input-data.md#sms-video-provide-frames)\.  | 
| frame\-no |  Yes  |  String  |  The frame order number\. This will determine the order of a frame in the sequence\.   | 
|  `unix-timestamp`  |  No  |  Integer  |  The unix timestamp of a frame\. The number of seconds since January 1st, 1970 until the UTC time when the frame was captured\.   | 
| frame |  Yes  |  String  |  The name of a video frame image file\.   | 