# Supported Data Formats<a name="sms-supported-data-formats"></a>

When you create an input manifest file for a [ built\-in task types](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-task-types.html) manually, your input data must be in one of the following support file formats for the respective input data type\. To learn about automated data setup, see [Automated Data Setup](sms-input-data-input-manifest.md#sms-console-create-manifest-file)\.

**Tip**  
When you use the automated data setup, additional data formats can be used to generate an input manifest file for video frame and text based task types\.


****  

| Task Types | Input Data Type | Support Formats | Example Input Manifest Line | 
| --- | --- | --- | --- | 
|  Bounding Box, Semantic Segmentation, Image Classification \(Single Label and Multi\-label\), Verify and Adjust Labels  |  Image  |  \.jpg, \.jpeg, \.png  |  <pre>{"source-ref": "s3://DOC-EXAMPLE-BUCKET1/example-image.png"}</pre>  | 
|  Named Entity Recognition, Text Classification \(Single and Multi\-Label\)  | Text | Raw text |  <pre>{"source": "Lorem ipsum dolor sit amet"}</pre>  | 
|  Video Classification  | Video clips | \.mp4, \.ogg, and \.webm |  <pre>{"source-ref": "s3:///example-video.mp4"}</pre>  | 
| Video Frame Object Detection, Video Frame Object Tracking \(bounding boxes, polylines, polygons or keypoint\) | Video frames and video frame sequence files \(for Object Tracking\) |  Video frames \- \.jpg, \.jpeg, \.png Sequence files \- \.json  | Refer to [Create a Video Frame Input Manifest File](sms-video-manual-data-setup.md#sms-video-create-manifest)\. | 
|  3D Point Cloud Semantic Segmentation, 3D Point Cloud Object Detection, 3D Point Cloud Object Tracking  | Point clouds and point cloud sequence files \(for Object Tracking\) |  Point clouds \- Binary pack format and ASCII\. For more information see [Accepted Raw 3D Data Formats](sms-point-cloud-raw-data-types.md)\. Sequence files \- \.json  | Refer to [Create an Input Manifest File for a 3D Point Cloud Labeling Job](sms-point-cloud-input-manifest.md)\. | 