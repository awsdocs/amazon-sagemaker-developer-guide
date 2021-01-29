# CORS Permission Requirement<a name="sms-cors-update"></a>

Earlier in 2020, widely used browsers like Chrome and Firefox changed their default behavior for rotating images based on image metadata, referred to as [EXIF data](https://en.wikipedia.org/wiki/Exif)\. Previously, images would always display in browsers exactly how they are stored on disk, which is typically unrotated\. After the change, images now rotate according to a piece of image metadata called *orientation value*\. This has important implications for the entire machine learning \(ML\) community\. For example, if the EXIF orientation is not considered, applications that are used to annotate images may display images in unexpected orientations and result in incorrect labels\. 

It is estimated that, starting with Chrome 89 on March 2nd, 2021, AWS can no longer automatically prevent the rotation of images because the web standards group W3C has decided that the ability to control rotation of images violates the web’s Same Origin Policy\. Therefore, to ensure human workers annotate your input images in a predictable orientation when you submit requests to create a labeling job, you must add a CORS header policy to the S3 buckets that contain your input images by February 10th, 2021\.

**Important**  
If you do not add a CORS configuration to the S3 buckets that contains your input data by February 10th, 2021, labeling tasks for those input data objects will fail\.

If you create your job through the Ground Truth console, under **Enable enhanced image access**, a check box is select to enable CORS configuration on the S3 bucket that contains your input manifest file\. Keep this check box selected\. If all of your input data is *not* located in the same S3 bucket as your input manifest file, you must add a CORS configuration to all S3 buckets that contain input data using the following instructions\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/cors-checkbox.png)

If you are using the `CreateLabelingJob` API to create a Ground Truth labeling job, you can add a CORS policy to an S3 bucket that contains input data in the S3 console\. To set the required CORS headers on the S3 bucket that contain your input images in the S3 console, follow the directions detailed in [How do I add cross\-domain resource sharing with CORS?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/add-cors-configuration.html)\. Use the following CORS configuration code for the buckets that hosts your images \(Choose JSON or XML based on your preference; they both accomplish the same configuration\)\.

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

The following GIF demonstrates the instructions found in the Amazon S3 documentation to add a CORS header policy using the Amazon S3 console\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/gifs/cors-config.gif)