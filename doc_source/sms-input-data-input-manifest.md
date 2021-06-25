# Use an Input Manifest File<a name="sms-input-data-input-manifest"></a>

Each line in an input manifest file is an entry containing an object, or a reference to an object, to label\. An entry can also contain labels from previous jobs and for some task types, additional information\. 

Input data and the manifest file must be stored in Amazon Simple Storage Service \(Amazon S3\)\. Each has specific storage and access requirements, as follows:
+ The Amazon S3 bucket that contains the input data must be in the same AWS Region in which you are running Amazon SageMaker Ground Truth\. You must give Amazon SageMaker access to the data stored in the Amazon S3 bucket so that it can read it\. For more information about Amazon S3 buckets, see [ Working with Amazon S3 buckets](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingBucket.html)\. 
+ The manifest file must be in the same AWS Region as the data files, but it doesn't need to be in the same location as the data files\. It can be stored in any Amazon S3 bucket that is accessible to the AWS Identity and Access Management \(IAM\) role that you assigned to Ground Truth when you created the labeling job\.

**Note**  
3D point cloud and video frame [ task types](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-task-types.html) have different input manifest requirements and attributes\.   
For [3D point cloud task types](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-point-cloud.html), refer to [Create an Input Manifest File for a 3D Point Cloud Labeling Job](sms-point-cloud-input-manifest.md)\.  
For [video frame task types](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-video-task-types.html), refer to [Create a Video Frame Input Manifest File](sms-video-manual-data-setup.md#sms-video-create-manifest)\.

The manifest is a UTF\-8 encoded file in which each line is a complete and valid JSON object\. Each line is delimited by a standard line break, \\n or \\r\\n\. Because each line must be a valid JSON object, you can't have unescaped line break characters\. For more information about data format, see [JSON Lines](http://jsonlines.org/)\.

Each JSON object in the manifest file can be no larger than 100,000 characters\. No single attribute within an object can be larger than 20,000 characters\. Attribute names can't begin with `$` \(dollar sign\)\.

Each JSON object in the manifest file must contain one of the following keys: `source-ref` or `source`\. The value of the keys are interpreted as follows:
+ `source-ref` – The source of the object is the Amazon S3 object specified in the value\. Use this value when the object is a binary object, such as an image\.
+ `source` – The source of the object is the value\. Use this value when the object is a text value\.



The following is an example of a manifest file for files stored in an Amazon S3 bucket:

```
{"source-ref": "S3 bucket location 1"}
{"source-ref": "S3 bucket location 2"}
   ...
{"source-ref": "S3 bucket location n"}
```

Use the `source-ref` key for image files for bounding box, image classification \(single and multi\-label\), semantic segmentation, and video clips for video classification labeling jobs\. 3D point cloud and video frame labeling jobs also use the `source-ref` key but these labeling jobs require additional information in the input manifest file\. For more information see [3D Point Cloud Input Data](sms-point-cloud-input-data.md) and [Video Frame Input Data](sms-video-frame-input-data-overview.md)\.

The following is an example of a manifest file with the input data stored in the manifest:

```
{"source": "Lorem ipsum dolor sit amet"}
{"source": "consectetur adipiscing elit"}
   ...
{"source": "mollit anim id est laborum"}
```

Use the `source` key for single and multi\-label text classification and named entity recognition labeling jobs\. 

You can include other key\-value pairs in the manifest file\. These pairs are passed to the output file unchanged\. This is useful when you want to pass information between your applications\. For more information, see [Output Data](sms-data-output.md)\.