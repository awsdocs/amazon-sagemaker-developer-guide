# CORS Permission Requirement<a name="sms-cors-update"></a>

Earlier in 2020, widely used browsers like Chrome and Firefox changed their default behavior for rotating images based on image metadata, referred to as [EXIF data](https://en.wikipedia.org/wiki/Exif)\. Previously, browsers would always display images in exactly the manner in which they are stored on disk, which is typically unrotated\. After the change, images now rotate according to a piece of image metadata called *orientation value*\. This has important implications for the entire machine learning \(ML\) community\. For example, if applications that annotate images do not consider the EXIF orientation, they may display images in unexpected orientations, resulting in incorrect labels\. 

Starting with Chrome 89, AWS can no longer automatically prevent the rotation of images because the web standards group W3C has decided that the ability to control rotation of images violates the web’s Same\-origin Policy\. Therefore, to ensure human workers annotate your input images in a predictable orientation when you submit requests to create a labeling job, you must add a CORS header policy to the Amazon S3 buckets that contain your input images\.

**Important**  
If you do not add a CORS configuration to the Amazon S3 buckets that contain your input data, labeling tasks for those input data objects will fail\.

If you create a job through the Ground Truth console, CORS is enabled by default\. If all of your input data is *not* located in the same Amazon S3 bucket as your input manifest file, you must add a CORS configuration to all Amazon S3 buckets that contain input data using the following instructions\.

If you are using the `CreateLabelingJob` API to create a Ground Truth labeling job, you can add a CORS policy to an Amazon S3 bucket that contains input data in the S3 console\. To set the required CORS headers on the Amazon S3 bucket that contain your input images in the Amazon S3 console, follow the directions detailed in [How do I add cross\-domain resource sharing with CORS?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/add-cors-configuration.html)\. Use the following CORS configuration code for the buckets that host your images\. If you use the Amazon S3 console to add the policy to your bucket, you must use the JSON format\.

**Important**  
If you create a 3D point cloud or video frame labeling job, you must add additional rules to your CORS configuration\. To learn more, see [3D Point Cloud Labeling Job Permission Requirements](sms-point-cloud-general-information.md#sms-security-permission-3d-point-cloud) and [Video Frame Job Permission Requirements](sms-video-overview.md#sms-security-permission-video-frame) respectively\. 

**JSON**

```
[{
   "AllowedHeaders": [],
   "AllowedMethods": ["GET"],
   "AllowedOrigins": ["*"],
   "ExposeHeaders": []
}]
```

**XML**

```
<CORSConfiguration>
 <CORSRule>
   <AllowedOrigin>*</AllowedOrigin>
   <AllowedMethod>GET</AllowedMethod>
 </CORSRule>
</CORSConfiguration>
```

The following GIF demonstrates the instructions found in the Amazon S3 documentation to add a CORS header policy using the Amazon S3 console\. For written instructions, see **Using the Amazon S3 console** on the documentation page [How do I add cross\-domain resource sharing with CORS?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/add-cors-configuration.html) in the Amazon Simple Storage Service User Guide\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/gifs/cors-config.gif)