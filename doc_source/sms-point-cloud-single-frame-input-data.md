# Create a Point Cloud Frame Input Manifest File<a name="sms-point-cloud-single-frame-input-data"></a>

In the single\-frame input manifest file, each line in the manifest contains data for a single point cloud frame\. The point cloud frame data can either be stored in binary or ASCII format \(see [Accepted Raw 3D Data Formats](sms-point-cloud-raw-data-types.md)\)\. This is the manifest file formatting required for 3D point cloud object detection and semantic segmentation\. Optionally, you can also provide camera sensor fusion data for each point cloud frame\. 

Ground Truth supports point cloud and video camera sensor fusion in the [world coordinate system](sms-point-cloud-sensor-fusion-details.md#sms-point-cloud-world-coordinate-system) for all modalities\. If you can obtain your 3D sensor extrinsic \(like a LiDAR extrinsic\), we recommend that you transform 3D point cloud frames into the world coordinate system using the extrinsic\. For more information, see [Sensor Fusion](sms-point-cloud-sensor-fusion-details.md#sms-point-cloud-sensor-fusion)\. 

However, if you cannot obtain a point cloud in world coordinate system, you can provide coordinates in the original coordinate system that the data was captured in\. If you are providing camera data for sensor fusion, it is recommended that you provide LiDAR sensor and camera pose in the world coordinate system\. 

To create a single\-frame input manifest file, you will identify the location of each point cloud frame that you want workers to label using the `source-ref` key\. Additionally, you must use the `source-ref-metadata` key to identify the format of your dataset, a timestamp for that frame, and, optionally, sensor fusion data and video camera images\.

The following example demonstrates the syntax used for an input manifest file for a single\-frame point cloud labeling job\. The example includes two point cloud frames\. For details about each parameter, see the table following this example\. 

```
{
    "source-ref": "s3://awsexamplebucket/examplefolder/frame1.bin",
    "source-ref-metadata":{
        "format": "binary/xyzi",
        "unix-timestamp": 1566861644.759115,
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
        "prefix": "s3://awsexamplebucket/lidar_singleframe_dataset/someprefix/",
        "images": [
        {
            "image-path": "images/frame300.bin_camera0.jpg",
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
    }
}
{
    "source-ref": "s3://awsexamplebucket/examplefolder/frame2.bin",
    "source-ref-metadata":{
        "format": "binary/xyzi",
        "unix-timestamp": 1566861632.759133,
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
        "prefix": "s3://awsexamplebucket/lidar_singleframe_dataset/someprefix/",
        "images": [
        {
            "image-path": "images/frame300.bin_camera0.jpg",
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
    }
}
```

The following table shows the parameters you can include in your input manifest file:


****  

|  Parameter  |  Required  |  Accepted Values  |  Description  | 
| --- | --- | --- | --- | 
|  `source-ref`  |  Yes  |  String **Accepted string value format**:  `s3://<bucket-name>/<folder-name>/point-cloud-frame-file`  |  The Amazon S3 location of a single point cloud frame\.  | 
|  `source-ref-metadata`  |  Yes  |  JSON object **Accepted parameters**:  `format`, `unix-timestamp`, `ego-vehicle-pose`, `position`, `prefix`, `images`  |  Use this parameter to include additional information about the point cloud in `source-ref`, and to provide camera data for sensor fusion\.   | 
|  `format`  |  No  |  String **Accepted string values**: `"binary/xyz"`, `"binary/xyzi"`, `"binary/xyzrgb"`, `"binary/xyzirgb"`, `"text/xyz"`, `"text/xyzi"`, `"text/xyzrgb"`, `"text/xyzirgb"` **Default Values**:  When the file identified in `source-ref` has a \.bin extension, `binary/xyzi` When the file identified in `source-ref` has a \.txt extension, `text/xyzi`  |  Use this parameter to specify the format of your point cloud data\. For more information, see [Accepted Raw 3D Data Formats](sms-point-cloud-raw-data-types.md)\.  | 
|  `unix-timestamp`  |  Yes  |  Number A unix timestamp\.   |  The unix timestamp is the number of seconds since January 1st, 1970 until the UTC time that the data was collected by a sensor\.   | 
|  `ego-vehicle-pose`  |  No  |  JSON object  |  The pose of the device used to collect the point cloud data\. For more information about this parameter, see [Include Vehicle Pose Information in Your Input Manifest](#sms-point-cloud-single-frame-ego-vehicle-input)\.  | 
|  `prefix`  |  No  |  String **Accepted string value format**:  `s3://<bucket-name>/<folder-name>/`  |  The location in Amazon S3 where your metadata, such as camera images, is stored for this frame\.  The prefix must end with a forward slash: `/`\.  | 
|  `images`  |  No  |  List  |  A list of parameters describing color camera images used for sensor fusion\. You can include up to 8 images in this list\. For more information about the parameters required for each image, see [Include Camera Data in Your Input Manifest](#sms-point-cloud-single-frame-image-input)\.   | 

## Include Vehicle Pose Information in Your Input Manifest<a name="sms-point-cloud-single-frame-ego-vehicle-input"></a>

Use the ego\-vehicle location to provide information about the location of the vehicle used to capture point cloud data\. Ground Truth use this information to compute LiDAR extrinsic matrix\. 

Ground Truth uses extrinsic matrices to project labels to and from the 3D scene and 2D images\. For more information, see [Sensor Fusion](sms-point-cloud-sensor-fusion-details.md#sms-point-cloud-sensor-fusion)\.

The following table provides more information about the `position` and orientation \(`heading`\) parameters that are required when you provide ego\-vehicle information\. 


****  

|  Parameter  |  Required  |  Accepted Values  |  Description  | 
| --- | --- | --- | --- | 
|  `position`  |  Yes  |  JSON object **Required Parameters**: `x`, `y`, and `z`\. Enter numbers for these parameters\.   |  The translation vector of the ego vehicle in the world coordinate system\.   | 
|  `heading`  |  Yes  |  JSON Object **Required Parameters**: `qx`, `qy`, `qz`, and `qw`\. Enter numbers for these parameters\.   |  The orientation of the frame of reference of the device or sensor mounted on the vehicle sensing the surrounding, measured in [quaternions](https://en.wikipedia.org/wiki/Quaternion), \(`qx`, `qy`, `qz`, `qw`\) in the a coordinate system\.  | 

## Include Camera Data in Your Input Manifest<a name="sms-point-cloud-single-frame-image-input"></a>

If you want to include video camera data with a frame, use the following parameters to provide information about each image\. The **Required** column below applies when the `images` parameter is included in the input manifest file under `source-ref-metadata`\. You are not required to include images in your input manifest file\. 

If you include camera images, you must include information about the camera `position` and `heading` used the capture the images in the world coordinate system\.

If your images are distorted, Ground Truth can automatically undistort them using information you provide about the image in your input manifest file, including distortion coefficients \(`k1`, `k2`, `k3`, `k4`, `p1`, `p1`\), the camera model and the camera intrinsic matrix\. The intrinsic matrix is made up of focal length \(`fx`, `fy`\), and the principal point \(`cx`, `cy)`\. See [Intrinsic Matrix ](sms-point-cloud-sensor-fusion-details.md#sms-point-cloud-intrinsic) to learn how Ground Truth uses the camera intrinsic\. If distortion coefficients are not included, Ground Truth will not undistort an image\. 


****  

|  Parameter  |  Required  |  Accepted Values  |  Description  | 
| --- | --- | --- | --- | 
|  `image-path`  |  Yes  |  String **Example of format**:  `<folder-name>/<imagefile.png>`  |  The relative location, in Amazon S3 of your image file\. This relative path will be appended to the path you specify in `prefix`\.   | 
|  `unix-timestamp`  |  Yes  |  Number  |  The unix timestamp is the number of seconds since January 1st, 1970 until the UTC time that the data was collected by a camera\.   | 
|  `camera-model`  |  No  |  String: **Accepted Values**: `"pinhole"`, `"fisheye"` **Default**: `"pinhole"`  |  The model of the camera used to capture the image\. This information is used to undistort camera images\.   | 
|  `fx, fy`  |  Yes  |  Numbers  |  The focal length of the camera, in the x \(`fx`\) and y \(`fy`\) directions\.  | 
|  `cx, cy`  |  Yes  | Numbers |  The x \(`cx`\) and y \(`cy`\) coordinates of the principal point\.   | 
|  `k1, k2, k3, k4`  |  No  |  Number  |  Radial distortion coefficients\. Supported for both **fisheye** and **pinhole** camera models\.   | 
|  `p1, p2`  |  No  |  Number  |  Tangential distortion coefficients\. Supported for **pinhole** camera models\.  | 
|  `skew`  |  No  |  Number  |  A parameter to measure the skew of an image\.   | 
|  `position`  |  Yes  |  JSON object **Required Parameters**: `x`, `y`, and `z`\. Enter numbers for these parameters\.   | The location or origin of the frame of reference of the camera mounted on the vehicle capturing images\. | 
|  `heading`  |  Yes  |  JSON Object **Required Parameters**: `qx`, `qy`, `qz`, and `qw`\. Enter numbers for these parameters\.   |  The orientation of the frame of reference of the camera mounted on the vehicle capturing images, measured using [quaternions](https://en.wikipedia.org/wiki/Quaternion), \(`qx`, `qy`, `qz`, `qw`\), in the world coordinate system\.   | 

## Point Cloud Frame Limits<a name="sms-point-cloud-single-frame-limits"></a>

You can include up to 100,000 point cloud frames in your input manifest file\. 3D point cloud labeling job have longer pre\-processing times than other Ground Truth task types\. For more information, see [Job Pre\-processing Time](sms-point-cloud-general-information.md#sms-point-cloud-job-creation-time)\.