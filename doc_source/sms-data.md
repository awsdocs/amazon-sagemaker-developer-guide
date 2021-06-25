# Use Input and Output Data<a name="sms-data"></a>

The input data that you provide to Amazon SageMaker Ground Truth is sent to your workers for labeling\. You choose the data to send to your workers by creating a single manifest file that defines all of the data that requires labeling or by sending input data objects to an ongoing, streaming labeling job to be labeled in real time\. 

The output data is the result of your labeling job\. The output data file, or *augmented manifest file*, contains label data for each object you send to the labeling job and metadata about the label assigned to data objects\.

When you use image classification \(single and multi\-label\), text classification \(single and multi\-label\), object detection, and semantic segmentation built in task types to create a labeling job, you can use the resulting augmented manifest file to launch a SageMaker training job\. For a demonstration of how to use an augmented manifest to train an object detection machine learning model with Amazon SageMaker, see [object\_detection\_augmented\_manifest\_training\.ipynb](https://sagemaker-examples.readthedocs.io/en/latest/ground_truth_labeling_jobs/object_detection_augmented_manifest_training/object_detection_augmented_manifest_training.html)\. For more information, see [Provide Dataset Metadata to Training Jobs with an Augmented Manifest File](augmented-manifest.md)\.

**Topics**
+ [Input Data](sms-data-input.md)
+ [3D Point Cloud Input Data](sms-point-cloud-input-data.md)
+ [Video Frame Input Data](sms-video-frame-input-data-overview.md)
+ [Output Data](sms-data-output.md)