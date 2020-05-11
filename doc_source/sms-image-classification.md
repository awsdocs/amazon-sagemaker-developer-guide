# Image Classification<a name="sms-image-classification"></a>

Use an Amazon SageMaker Ground Truth image classification labeling task when you need workers to classify images using predefined labels that you specify\. Workers are shown images and are asked to choose one label for each image\. 

You create an image classification labeling job using the Ground Truth section of the Amazon SageMaker console or the [CreateLabelingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html) operation\. Ground Truth provides a worker console similar to the following for labeling tasks\. When you create the labeling job with the console, you can modify the images and content that are shown\. If you create a labeling job using the API, you must supply a custom\-built template\. To learn how to create a custom template, see [Creating Custom Labeling Workflows](sms-custom-templates.md)\. To see examples of custom templates that can be used for image classification job types, see this [Github Repository](https://github.com/aws-samples/amazon-sagemaker-ground-truth-task-uis/tree/master/images)\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/image-classification-example.png)

You can create an image classification labeling job using the Amazon SageMaker console or API\. To learn how to start an image classification labeling job using on the console, see [Getting started](sms-getting-started.md)\. To use the API, see [ `CreateLabelingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html)\.