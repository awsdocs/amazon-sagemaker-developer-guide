# Verify and Adjust Labels<a name="sms-verification-data"></a>

When the labels on a dataset need to be validated, Amazon SageMaker Ground Truth provides functionality to have workers verify that labels are correct or to adjust previous labels\.

These types of jobs fall into two distinct categories:
+ Label verification – Workers indicate if the existing labels are correct, or rate their quality, and can add comments to explain their reasoning\.
+ Label adjustment – Workers adjust prior annotations to correct them\.

You can use Ground Truth to adjust or verify only labels that resulted from bounding box and semantic segmentation labeling jobs\. You can start a label verification and adjustment jobs using the SageMaker console or the API\.

## Create and Start a Label Verification Job \(Console\)<a name="sms-data-verify-start-console"></a>

**To start a label verification job \(console\)**

1. Open the SageMaker console: [https://console.aws.amazon.com/sagemaker/](https://console.aws.amazon.com/sagemaker/) and choose **Labeling jobs**\.

1. Start a new labeling job by [chaining](sms-reusing-data.md) a prior job or start from scratch, specifying an input manifest that contains labeled data objects\.

1. Choose the **Label verification** task type and choose **Next**\.

1. In the **Display existing labels** pane, the system shows the available label attribute names in your manifest\. Choose the label attribute name for the labeling job that you want to verify\.

1. Use the instructions areas of the tool designer to provide context about what the previous labelers were asked to do and what the current verifiers need to check\.

1. Choose **See preview** to check that the tool is displaying the prior labels correctly and presents the label verification task clearly\.

1. Select **Create**\. This will create and start your labeling job\.

## Start an Label Adjustment Job \(Console\)<a name="sms-data-adjust-start-console"></a>

Use the SageMaker console to start a label verification or adjustment job\.

**To start a label adjustment job \(console\)**

1. Open the SageMaker console: [https://console.aws.amazon.com/sagemaker/](https://console.aws.amazon.com/sagemaker/) and choose **Labeling jobs**\.

1. Start a new labeling job by [chaining](sms-reusing-data.md) a prior job or start from scratch, specifying an input manifest that contains labeled data objects\.

1. Choose the correct task type for your data and choose **Next**\.

1. After choosing the workers, expand **Display existing labels**\. If it isn't expanded, choose the arrow next to the title to expand it\.

1. Check the box next to **I want to display existing labels from the dataset for this job**\.

1. For **Label attribute name**, choose the name from your manifest that corresponds to the labels that you want to display for adjustment\. Ground Truth tries to detect and populate these values by analyzing the manifest, but you might need to set the correct value\.

1. Use the instructions areas of the tool designer to provide context about what the previous labelers were tasked with doing and what the current verifiers need to check and adjust\.

1. Choose **See preview** to check that the tool shows the prior labels correctly and presents the task clearly\.

1. Select **Create**\. This will create and start your labeling job\.

## Start a Label Verification or Adjustment Job \(API\)<a name="sms-data-verify-start-api"></a>

### <a name="sms-data-verify-start-api"></a>

Start a label verification or adjustment job by chaining a successfully completed job or starting a new job from scratch using the [ `CreateLabelingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html) operation\. To learn how to start a new labeling job by chaining a previous job, see [Start a Chained Job \(API\)](sms-reusing-data.md#sms-reusing-data-API)\. 

To create a label verification or adjustment job, use the following guidelines to specify API attributes for the `CreateLabelingJob` operation:
+ Use the [LabelAttributeName](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#SageMaker-CreateLabelingJob-request-LabelAttributeName) parameter to specify the output label name that you want to use for verified or adjusted labels\.
+ If you are chaining the job, the labels from the previous labeling job to be adjusted or verified will be specified in the custom UI template\. To learn how to create a custom template, see [Create Custom Worker Task Template](a2i-custom-templates.md)\.

  Identify the location of the UI template in the [UiTemplateS3Uri](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UiConfig.html) parameter\. SageMaker provides widgets that you can use in your custom template to display old labels\. Use the `initial-value` attribute in one of the following crowd elements to extract the labels that need verification or adjustment and include them in your task template:
  + [crowd\-semantic\-segmentation](sms-ui-template-crowd-semantic-segmentation.md)—Use this crowd element in your custom UI task template to specify semantic segmentation labels that need to be verified or adjusted\.
  + [crowd\-bounding\-box](sms-ui-template-crowd-bounding-box.md)—Use this crowd element in your custom UI task template to specify bounding box labels that need to be verified or adjusted\.
+ The [LabelCategoryConfigS3Uri](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html#SageMaker-CreateLabelingJob-request-LabelCategoryConfigS3Uri) parameter must contain the same label categories as the previous labeling job\. Adding new label categories or adjusting label categories is not supported\.

Ground Truth stores the output data from a label verification or adjustment job in the S3 bucket that you specified in the [S3OutputPath](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_LabelingJobOutputConfig.html#SageMaker-Type-LabelingJobOutputConfig-S3OutputPath) parameter of the [CreateLabelingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html) operation\. For more information about the output data from a label verification or adjustment labeling job, see [Label Verification and Adjustment Data in the Output Manifest](#sms-data-verify-manifest)\.

## Label Verification and Adjustment Data in the Output Manifest<a name="sms-data-verify-manifest"></a>

Amazon SageMaker Ground Truth writes label verification data to the output manifest within the metadata for the label\. It adds two properties to the metadata:
+ A `type` property, with a value of "`groundtruth/label-verification`\.
+ A `worker-feedback` property, with an array of `comment` values\. This property is added when the worker enters comments\. If there are no comments, the field doesn't appear\.

The following example output manifest shows how label verification data appears:

```
{
  "source-ref":"S3 bucket location", 
  "verify-bounding-box":"1",    
  "verify-bounding-box-metadata":  
  {
    "class-name": "bad", 
    "confidence": 0.93, 
    "type": "groundtruth/label-verification", 
    "job-name": "verify-bounding-boxes",
    "human-annotated": "yes",
    "creation-date": "2018-10-18T22:18:13.527256",
    "worker-feedback": [
      {"comment": "The bounding box on the bird is too wide on the right side."},
      {"comment": "The bird on the upper right is not labeled."}
    ]
  }
}
```

The worker output of adjustment tasks resembles the worker output of the original task, except that it contains the adjusted values and an `adjustment-status` property with the value of `adjusted` or `unadjusted` to indicate whether an adjustment was made\.

For more examples of the output of different tasks, see [Output Data](sms-data-output.md)\.

## Cautions and Considerations<a name="sms-data-verify-cautions"></a>

To get expected behavior when creating a label verification or adjustment job, carefully verify your input data\. 
+ If you are using image data, verify that your manifest file contains hexadecimal RGB color information\. 
+ To save money on processing costs, filter your data to ensure you are not including unwanted objects in your labeling job input manifest\.
+ Add required Amazon S3 permissions to ensure your input data is processed correctly\. 

### Color Information Requirements for Semantic Segmentation Jobs<a name="sms-data-verify-color-info"></a>

To properly reproduce color information in verification or adjustment tasks, the tool requires hexadecimal RGB color information in the manifest \(for example, \#FFFFFF for white\)\. When you set up a Semantic Segmentation verification or adjustment job, the tool examines the manifest to determine if this information is present\. If it can't find it,Amazon SageMaker Ground Truth displays an error message and the ends job setup\.

In prior iterations of the Semantic Segmentation tool, category color information wasn't output in hexadecimal RGB format to the output manifest\. That feature was introduced to the output manifest at the same time the verification and adjustment workflows were introduced\. Therefore, older output manifests aren't compatible with this new workflow\.

### Filter Your Data Before Starting the Job<a name="sms-data-verify-filter"></a>

Amazon SageMaker Ground Truth processes all objects in your input manifest\. If you have a partially labeled data set, you might want to create a custom manifest using an [Amazon S3 Select query](https://docs.aws.amazon.com/AmazonS3/latest/dev/selecting-content-from-objects.html) on your input manifest\. Unlabeled objects individually fail, but they don't cause the job to fail, and they might incur processing costs\. Filtering out objects you don't want verified reduces your costs\.

If you create a verification job using the console, you can use the filtering tools provided there\. If you create jobs using the API, make filtering your data part of your workflow where needed\.

### Security Considerations for Images<a name="sms-data-verify-cors"></a>

Due to browser security models, some image markup tasks—like keypoints, polygons, bounding boxes, and semantic segmentation—require you to add a cross\-origin resource sharing \(CORS\) specification to the Amazon Simple Storage Service \(Amazon S3\) bucket where you store the images\. You must apply the CORS specification prior to marking up the images\.

**Applying CORS to your bucket**

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Select the bucket in which you are storing your images\.

1. Select the **Permissions** tab, then **CORS configuration**\.

1. Add the following block of XML and save\.

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
   <CORSRule>
       <AllowedOrigin>*</AllowedOrigin>
       <AllowedMethod>GET</AllowedMethod>
   </CORSRule>
   </CORSConfiguration>
   ```