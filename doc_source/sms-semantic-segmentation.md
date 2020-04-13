# Semantic Segmentation<a name="sms-semantic-segmentation"></a>

To identify the contents of an image at the pixel level, use an Amazon SageMaker Ground Truth semantic segmentation labeling task\. When assigned a semantic segmentation labeling job, workers classify pixels in the image into a set of predefined labels or classes\. Ground Truth supports single and multi\-class semantic segmentation labeling jobs\.

You create a semantic segmentation labeling job using the Ground Truth section of the Amazon SageMaker console or the [ `CreateLabelingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html) operation\. Ground Truth provides a worker console similar to the following for labeling tasks\. When you create the labeling job with the console, you can modify the images and content that are shown\. If you create a labeling job using the API, you must supply a custom\-built template\. To learn how to create a custom template, see [Creating Custom Labeling Workflows](sms-custom-templates.md)\. To see examples of custom templates that can be used for semantic segmentation labeling job types, see this [Github Repository](https://github.com/aws-samples/amazon-sagemaker-ground-truth-task-uis/tree/master/images)\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/semantic_segmentation_sample.gif)

Images that contain large numbers of objects that need to be segmented require more time\. To help workers label these objects in less time and with greater accuracy, Ground Truth provides an AI\-assisted auto\-segmentation tool\. For information, see [Auto\-Segmentation Tool](sms-auto-segmentation.md)\.

**Note**  
The auto\-segmentation tool is available in all segmentation tasks that are sent to a private workforce or vendor workforce\. It isn't available for tasks sent to the public workforce \(Amazon Mechanical Turk\)\. 

You can create a semantic segmentation labeling job using the Amazon SageMaker console or API\. To learn how to start a semantic segmentation labeling job on the console, see [Getting started](sms-getting-started.md)\. To use the API, see [ `CreateLabelingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html)\. 