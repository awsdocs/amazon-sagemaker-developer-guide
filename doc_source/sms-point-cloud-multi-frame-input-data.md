# Create a Point Cloud Sequence Input Manifest<a name="sms-point-cloud-multi-frame-input-data"></a>

In the point cloud sequence input manifest file, each line in the manifest contains a sequence of point cloud frames\. The point cloud data for each frame in the sequence can either be stored in binary or ASCII format\. For more information, see [Accepted Raw 3D Data Formats](sms-point-cloud-raw-data-types.md)\. This is the manifest file formatting required for 3D point cloud object tracking\. Optionally, you can also provide point attribute and camera sensor fusion data for each point cloud frame\. When you create a sequence input manifest file, you must provide LiDAR and video camera sensor fusion data in a [world coordinate system](sms-point-cloud-sensor-fusion-details.md#sms-point-cloud-world-coordinate-system)\. 

The following example demonstrates the syntax used for an input manifest file when each line in the manifest is a sequence file\. 

```
{"source-ref": "s3://awsexamplebucket/example-folder/seq1.json"}
{"source-ref": "s3://awsexamplebucket/example-folder/seq2.json"}
```

The data for each sequence of point cloud frames needs to be stored in a JSON data object\. The following is an example of the format you use for a sequence file\. Information about each frame is included as a JSON object and is listed in the `frames` list\. In this example, information is given for a single frame, and *\.\.\.* is used to indicated where you should include information for additional frames\. 

```
{
  "seq-no": 1,
  "prefix": "s3://awsexamplebucket/example_lidar_sequence_dataset/seq1/",
  "number-of-frames": 100,
  "frames":[
    {
        "frame-no": 300, 
        "unix-timestamp": 1566861644.759115, 
        "frame": "example_lidar_frames/frame300.bin", 
        "format": "binary/xyzi", 
        "ego-vehicle-pose":{
            "position": {
                "x": -2.7161461413869947,
                "y": 116.25822288149078,
                "z": 1.8348751887989483
            },
            "heading": {
                "qx": -0.02111296123795955,
                "qy": -0.006495469416730261,
                "qz": -0.008024565904865688,
                "qw": 0.9997181192298087
            }
        }, 
        "images": [
        {
            "image-path": "example_images/frame300.bin_camera0.jpg",
            "unix-timestamp": 1566861644.759115,
            "fx": 847.7962624528487,
            "fy": 850.0340893791985,
            "cx": 576.2129134707038,
            "cy": 317.2423573573745,
            "k1": 0,
            "k2": 0,
            "k3": 0,
            "k4": 0,
            "p1": 0,
            "p2": 0,
            "skew": 0,
            "position": {
                "x": -2.2722515189268138,
                "y": 116.86003310568965,
                "z": 1.454614668542299
            },
            "heading": {
                "qx": 0.7594754093069037,
                "qy": 0.02181790885672969,
                "qz": -0.02461725233103356,
                "qw": -0.6496916273040025
            },
            "camera-model": "pinhole"
        }]
    },
    {
        "frame-no": 303, 
        "unix-timestamp": 1566861644.759115, 
        "frame": "example_lidar_frames/frame303.bin", 
        "format": "text/xyzi", 
        "ego-vehicle-pose":{...}, 
        "images":[{...}]
    },
     ...
  ]
}
```

The following table provides details about the top\-level parameters shown in the this code example\. For detailed information about the parameters required for individual frames in the sequence file, see [Parameters for Individual Point Cloud Frames](#sms-point-cloud-multi-frame-input-single-frame)\.


****  

|  Parameter  |  Required  |  Accepted Values  |  Description  | 
| --- | --- | --- | --- | 
|  `seq-no`  |  Yes  |  Integer  |  The ordered number of the sequence\.   | 
|  `prefix`  |  Yes  |  String **Accepted Values**: `s3://<bucket-name>/<prefix>/`  |  The Amazon S3 location where the sequence files are located\.  The prefix must end with a forward slash: `/`\.  | 
|  `number-of-frames`  |  Yes  |  Integer  |  The total number of frames included in the sequence file\. This number must match the total number of frames listed in the `frames` parameter in the next row\.  | 
|  `frames`  |  Yes  |  List of JSON objects  |  A list of frame data\. The length of the list must equal `number-of-frames`\. In the worker UI, frames in a sequence will be the same as the order of frames in this array\.  For details about the format of each frame, see [Parameters for Individual Point Cloud Frames](#sms-point-cloud-multi-frame-input-single-frame)\.   | 

## Parameters for Individual Point Cloud Frames<a name="sms-point-cloud-multi-frame-input-single-frame"></a>

The following table shows the parameters you can include in your input manifest file\.


****  

|  Parameter  |  Required  |  Accepted Values  |  Description  | 
| --- | --- | --- | --- | 
|  `frame-no`  |  No  |  Integer  |  A frame number\.   | 
|  `unix-timestamp`  |  Yes  |  Number  |  The unix timestamp is the number of seconds since January 1st, 1970 until the UTC time that the data was collected by a sensor\.   | 
|  `frame`  |  Yes  |  String **Example of format** `<folder-name>/<sequence-file.json>`  |  The relative location, in Amazon S3 of your sequence file\. This relative path will be appended to the path you specify in `prefix`\.  | 
|  `format`  |  No  |  String **Accepted string values**: `"binary/xyz"`, `"binary/xyzi"`, `"binary/xyzrgb"`, `"binary/xyzirgb"`, `"text/xyz"`, `"text/xyzi"`, `"text/xyzrgb"`, `"text/xyzirgb"` **Default Values**:  When the file identified in `source-ref` has a \.bin extension, `binary/xyzi` When the file identified in `source-ref` has a \.txt extension, `text/xyzi`  |  Use this parameter to specify the format of your point cloud data\. For more information, see [Accepted Raw 3D Data Formats](sms-point-cloud-raw-data-types.md)\.  | 
|  `ego-vehicle-pose`  |  No  |  JSON object  |  The pose of the device used to collect the point cloud data\. For more information about this parameter, see [Include Vehicle Pose Information in Your Input Manifest](#sms-point-cloud-multi-frame-ego-vehicle-input)\.  | 
|  `prefix`  |  No  |  String **Accepted string value format**:  `s3://<bucket-name>/<folder-name>/`  |  The location in Amazon S3 where your metadata, such as camera images, is stored for this frame\.  The prefix must end with a forward slash: `/`\.  | 
|  `images`  |  No  |  List  |  A list parameters describing color camera images used for sensor fusion\. You can include up to 8 images in this list\. For more information about the parameters required for each image, see [Include Camera Data in Your Input Manifest](#sms-point-cloud-multi-frame-image-input)\.   | 

## Include Vehicle Pose Information in Your Input Manifest<a name="sms-point-cloud-multi-frame-ego-vehicle-input"></a>

Use the ego\-vehicle location to provide information about the pose of the vehicle used to capture point cloud data\. Ground Truth use this information to compute LiDAR extrinsic matrices\. 

Ground Truth uses extrinsic matrices to project labels to and from the 3D scene and 2D images\. For more information, see [Sensor Fusion](sms-point-cloud-sensor-fusion-details.md#sms-point-cloud-sensor-fusion)\.

The following table provides more information about the `position` and orientation \(`heading`\) parameters that are required when you provide ego\-vehicle information\. 


****  

|  Parameter  |  Required  |  Accepted Values  |  Description  | 
| --- | --- | --- | --- | 
|  `position`  |  Yes  |  JSON object **Required Parameters**: `x`, `y`, and `z`\. Enter numbers for these parameters\.   |  The translation vector of the ego vehicle in the world coordinate system\.   | 
|  `heading`  |  Yes  |  JSON Object **Required Parameters**: `qx`, `qy`, `qz`, and `qw`\. Enter numbers for these parameters\.   |  The orientation of the frame of reference of the device or sensor mounted on the vehicle sensing the surrounding, measured in [quaternions](https://en.wikipedia.org/wiki/Quaternion), \(`qx`, `qy`, `qz`, `qw`\) in the a coordinate system\.  | 

## Include Camera Data in Your Input Manifest<a name="sms-point-cloud-multi-frame-image-input"></a>

If you want to include color camera data with a frame, use the following parameters to provide information about each image\. The **Required** column in the following table applies when the `images` parameter is included in the input manifest file\. You are not required to include images in your input manifest file\. 

If you include camera images, you must include information about the `position` and orientation \(`heading`\) of the camera used the capture the images\. 

If your images are distorted, Ground Truth can automatically undistort them using information you provide about the image in your input manifest file, including distortion coefficients \(`k1`, `k2`, `k3`, `k4`, `p1`, `p1`\), camera model and focal length \(`fx`, `fy`\), and the principal point \(`cx`, `cy)`\. To learn more about these coefficients and undistorting images, see [Camera calibration With OpenCV](https://docs.opencv.org/2.4.13.7/doc/tutorials/calib3d/camera_calibration/camera_calibration.html)\. If distortion coefficients are not included, Ground Truth will not undistort an image\. 


****  

|  Parameter  |  Required  |  Accepted Values  |  Description  | 
| --- | --- | --- | --- | 
|  `image-path`  |  Yes  |  String **Example of format**:  `<folder-name>/<imagefile.png>`  |  The relative location, in Amazon S3 of your image file\. This relative path will be appended to the path you specify in `prefix`\.   | 
|  `unix-timestamp`  |  Yes  |  Number  |  The timestamp of the image\.   | 
|  `camera-model`  |  No  |  String: **Accepted Values**: `"pinhole"`, `"fisheye"` **Default**: `"pinhole"`  |  The model of the camera used to capture the image\. This information is used to undistort camera images\.   | 
|  `fx, fy`  |  Yes  |  Numbers  |  The focal length of the camera, in the x \(`fx`\) and y \(`fy`\) directions\.  | 
|  `cx, cy`  |  Yes  | Numbers |  The x \(`cx`\) and y \(`cy`\) coordinates of the principal point\.   | 
|  `k1, k2, k3, k4`  |  No  |  Number  |  Radial distortion coefficients\. Supported for both **fisheye** and **pinhole** camera models\.   | 
|  `p1, p2`  |  No  |  Number  |  Tangential distortion coefficients\. Supported for **pinhole** camera models\.  | 
|  `skew`  |  No  |  Number  |  A parameter to measure any known skew in the image\.  | 
|  `position`  |  Yes  |  JSON object **Required Parameters**: `x`, `y`, and `z`\. Enter numbers for these parameters\.   |  The location or origin of the frame of reference of the camera mounted on the vehicle capturing images\.  | 
|  `heading`  |  Yes  |  JSON Object **Required Parameters**: `qx`, `qy`, `qz`, and `qw`\. Enter numbers for these parameters\.   |  The orientation of the frame of reference of the camera mounted on the vehicle capturing images, measured using [quaternions](https://en.wikipedia.org/wiki/Quaternion), \(`qx`, `qy`, `qz`, `qw`\)\.   | 

## Sequence File and Point Cloud Frame Limits<a name="sms-point-cloud-multi-frame-limits"></a>

You can include up to 100,000 point cloud frame sequences in your input manifest file\. You can include up to 500 point cloud frames in each sequence file\. 

Keep in mind that 3D point cloud labeling job have longer pre\-processing times than other Ground Truth task types\. For more information, see [Job Pre\-processing Time](sms-point-cloud-general-information.md#sms-point-cloud-job-creation-time)\.