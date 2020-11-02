# Accepted Raw 3D Data Formats<a name="sms-point-cloud-raw-data-types"></a>

Ground Truth uses your 3D point cloud data to render a 3D scenes that workers annotate\. This section describes the raw data formats that are accepted for point cloud data and sensor fusion data for a point cloud frame\. For each frame, Ground Truth supports Compact Binary Pack Format \(\.bin\) and ASCII \(\.txt\) files\. These files contain information about the location \(`x`, `y`, and `z` coordinates\) of all points that make up that frame, and, optionally, information about the pixel color of each point for colored point clouds\. When you create a 3D point cloud labeling job input manifest file, you can specify the format of your raw data in the `format` parameter\. 

The following table lists elements that Ground Truth supports in point cloud frame files to describe individual points\. 


****  

| Symbol | Value | 
| --- | --- | 
|  `x`  |  The x coordinate of the point\.  | 
|  `y`  |  The y coordinate of the point\.  | 
|  `z`  |  The z coordinate of the point\.  | 
|  `i`  |  The intensity of the point\.  | 
|  `r`  |  The red color channel component\. An 8\-bit value \(0\-255\)\.  | 
|  `g`  |  The green color channel component\. An 8\-bit value \(0\-255\)  | 
|  `b`  |  The blue color channel component\. An 8\-bit value \(0\-255\)  | 

Ground Truth assumes the following about your input data:
+ All of the positional coordinates \(x, y, z\) are in meters\. 
+ All the pose headings \(qx, qy, qz, qw\) are measured in Spatial [Quaternions](https://en.wikipedia.org/wiki/Quaternions_and_spatial_rotation) \.

## Compact Binary Pack Format<a name="sms-point-cloud-raw-data-cbpf-format"></a>

The Compact Binary Pack Format represents a point cloud as an ordered set of a stream of points\. Each point in the stream is an ordered binary pack of 4\-byte float values in some variant of the form `xyzirgb`\. The `x`, `y`, and `z` elements are required and additional information about that pixel can be included in a variety of ways using `i`, `r`, `g`, and `b`\. 

To use a binary file to input point cloud frame data to a Ground Truth 3D point cloud labeling job, enter `binary/` in the `format` parameter for your input manifest file and replace `` with the order of elements in each binary pack\. For example, you may enter one of the following for the `format` parameter\. 
+ `binary/xyzi` – When you use this format, your point element stream would be in the following order: `x1y1z1i1x2y2z2i2...`
+ `binary/xyzrgb` – When you use this format, your point element stream would be in the following order: `x1y1z1r1g1b1x2y2z2r2g2b2...`
+ `binary/xyzirgb` – When you use this format, your point element stream would be in the following order: `x1y1z1i1r1g1b1x2y2z2i2r2g2b2...`

When you use a binary file for your point cloud frame data, if you do not enter a value for `format`, the default pack format `binary/xyzi` is used\. 

## ASCII Format<a name="sms-point-cloud-raw-data-ascii-format"></a>

The ASCII format uses a text file to represent a point cloud, where each line in the ASCII point cloud file represents a single point\. Each point is a line the text file and contains white space separated values, each of which is a 4\-byte float ASCII values\. The `x`, `y`, and `z` elements are required for each point and additional information about that point can be included in a variety of ways using `i`, `r`, `g`, and `b`\.

To use a text file to input point cloud frame data to a Ground Truth 3D point cloud labeling job, enter `text/` in the `format` parameter for your input manifest file and replace `` with the order of point elements on each line\. 

For example, if you enter `text/xyzi` for `format`, your text file for each point cloud frame should look similar to the following: 

```
x1 y1 z1 i1
x2 y2 z2 i2
...
...
```

If you enter `text/xyzrgb`, your text file should look similar to the following: 

```
x1 y1 z1 r1 g1 b1
x2 y2 z2 r2 g2 b1
...
...
```

When you use a text file for your point cloud frame data, if you do not enter a value for `format`, the default format `text/xyzi` will be used\. 

## Point Cloud Resolution Limits<a name="sms-point-cloud-resolution"></a>

Ground Truth does not have a resolution limit for 3D point cloud frames\. However, we recommend that you limit each point cloud frame to 500K points for optimal performance\. When Ground Truth renders the 3D point cloud visualization, it must be viewable on your workers' computers, which depends on workers' computer hardware\. Point cloud frames that are larger than 1 million points may not render on standard machines, or may take too long to load\. 