# Use Input and Output Data<a name="sms-data"></a>

The input data that you provide to Amazon SageMaker Ground Truth is sent to your workers for labeling\. You choose the data to send to your workers by creating a manifest file that defines the data that requires labeling\.

The output data is the result of your labeling job\. The output data file contains one entry for each object in the input dataset that specifies the label\.

**Topics**
+ [Input Data](sms-data-input.md)
+ [3D Point Cloud Input Data](sms-point-cloud-input-data.md)
+ [Video Frame Input Data](sms-video-frame-input-data-overview.md)
+ [Output Data](sms-data-output.md)

After you create an augmented manifest file, you can use it in a training job\. For a demonstration of how to use an augmented manifest to train an object detection machine learning model with Amazon SageMaker, see [object\_detection\_augmented\_manifest\_training\.ipynb](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/ground_truth_labeling_jobs/object_detection_augmented_manifest_training/object_detection_augmented_manifest_training.ipynb)\. For more information, see [Provide Dataset Metadata to Training Jobs with an Augmented Manifest File](augmented-manifest.md)\.