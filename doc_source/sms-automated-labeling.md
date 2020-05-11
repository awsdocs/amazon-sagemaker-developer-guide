# Automate Data Labeling<a name="sms-automated-labeling"></a>

If you choose, Amazon SageMaker Ground Truth can use active learning to automate the labeling of your input data\. *Active learning* is a machine learning technique that identifies data that should be labeled by your workers\. In Ground Truth, this functionality is called automated data labeling\. Automated data labeling helps to reduce the cost and time that it takes to label your dataset compared to using only humans\. When you use automated labeling, you incur Amazon SageMaker training and inference costs\. 

We recommend using automated data labeling on large datasets because the neural networks used with active learning require a significant amount of data for every new dataset\. Typically, as more data is provided, the potential for high accuracy predictions goes up\. Data will only be auto\-labeled if the neural network used in the auto\-labeling model can achieve an acceptably high level of accuracy\. Therefore, with larger datasets, there is more potential to automatically label the data because the neural network can achieve high enough accuracy for auto\-labeling\. Automated data labeling is most appropriate when you have thousands of data objects\. The minimum number of objects allowed for automated data labeling is 1,250, but we strongly suggest providing a minimum of 5,000 objects\.

Automated data labeling is available only for the following Ground Truth built\-in task types: 
+ Image classification \(Single label\)
+ Semantic segmentation
+ Bounding box 
+ Text classification

You enable automated data labeling when you create a labeling job\. This is how it works:

1. When Ground Truth starts an automated data labeling job, it selects a random sample of input data \(objects\) and sends it to human workers\.

1. When the labeled data are returned, Ground Truth uses this data, the validation data, to validate the models trained for automated data labeling\. 

1. Ground Truth runs a batch transform job, using the validated model for inference on the validation data\. Batch inference produces a confidence score and quality metric for each object in the validation data\.

1. The auto labeling component will use these quality metrics and confidence scores to create a confidence score threshold that ensure quality labels\. 

1. Ground Truth runs a batch transform job on the unlabeled data in the dataset, using the same validated model for inference\. This will produce a confidence score for each object\. 

1. The Ground Truth auto labeling component determines if the confidence score produced in step 5 for each object meets the required threshold determined in step 4\. If the confidence score meets the threshold, the expected quality of automatically labeling exceeds the requested level of accuracy and that object will be considered auto\-labeled\. 

1. Step 6 will produce a dataset of unlabeled data with confidence scores\. Ground Truth will select data points with low confidence scores from this dataset and send them to human workers\. 

1. Ground Truth uses the existing human\-labeled data and this additional labeled data from human workers to train a new model\.

1. The process is repeated until the dataset is fully labeled or until another stopping condition is met\. For example, auto labeling will stop if your human annotation budget is reached\.

Input data quotas apply for automated data labeling jobs\. See [Input Data Quotas](sms-data-input.md#input-data-limits) for information about dataset size, input data size and resolution limits for automated Semantic Segmentation, Object Detection, and Image Classification\. 

**Note**  
Before you use an the automated\-labeling model in production, you need to fine\-tune or test the it, or both \. You might fine\-tune the model \(or create and tune another supervised model of your choice\) on the dataset produced by your labeling job to optimize the model’s architecture and hyperparameters\. If you decide to use the model for inference without fine\-tuning it, we strongly recommend making sure that you evaluate its accuracy on a representative \(for example, randomly selected\) subset of the dataset labeled with Ground Truth and that it matches your expectations\.

## Create an Automated Data Labeling Job \(Console\)<a name="sms-create-automated-labeling-console"></a>

Automated data labeling is available only for Ground Truth built\-in algorithms\. 

To use the Amazon SageMaker console to create a labeling job that uses automated data labeling you need to know how to create a labeling job using the Amazon SageMaker console\. To learn how to create a labeling job in the console using Ground Truth, see [Getting started](sms-getting-started.md)\. 

**To create an automated data labeling job \(console\)**

1. Open the Ground Truth **Labeling jobs** section of the Amazon SageMaker console: [https://console.aws.amazon.com/sagemaker/groundtruth](https://console.aws.amazon.com/sagemaker/groundtruth)\.

1. Using [Getting started](sms-getting-started.md) as a guide, complete the **Job overview** and **Task type** sections\. Note that auto labeling is not support for custom task types\.

1. Under **Workers**, choose your workforce type\. 

1. In the same section, choose **Enable automated data labeling**\. 

1. Using [Step 4: Configure the Bounding Box Tool](sms-getting-started-step4.md) as a guide, create worker instructions in the section ***Task Type* labeling tool**\. For example, if you chose **Semantic segmentation** as your labeling job type, this section will be called **Semantic segmentation labeling tool**\.

1. To preview your worker instructions and dashboard, choose **Preview**\.

1. Choose **Create**\. This will create and start your labeling job and the auto labeling process\. 

You will see your labeling job appear in the **Labeling jobs** section of the Amazon SageMaker console\. Your output data will appear in the Amazon S3 bucket that you specified when creating the labeling job\. For more information about the format and file structure of your labeling job output data, see [Output Data](sms-data-output.md)\.

## Create an Automated Data Labeling Job \(API\)<a name="sms-create-automated-labeling-api"></a>

To create an automated data labeling job using the the Amazon SageMaker API, use the [ `LabelingJobAlgorithmsConfig`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_LabelingJobAlgorithmsConfig.html) parameter of the [ `CreateLabelingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html) operation\. 

Specify the Amazon Resource Name \(ARN\) of the algorithm that you are using for automated data labeling in the [LabelingJobAlgorithmSpecificationArn](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_LabelingJobAlgorithmsConfig.html#SageMaker-Type-LabelingJobAlgorithmsConfig-LabelingJobAlgorithmSpecificationArn) parameter\. Choose from one of the four Ground Truth built\-in algorithms that are supported with automated labeling:
+ Image classification
+ Semantic segmentation
+ Object detection \(bounding box\) 
+ Text classification

When an automated data labeling job finishes, Ground Truth returns the ARN of the model it used for the automated data labeling job\. Use this model as the starting model for similar auto\-labeling job types by providing the ARN, in string format, in the [InitialActiveLearningModelArn](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_LabelingJobAlgorithmsConfig.html#SageMaker-Type-LabelingJobAlgorithmsConfig-InitialActiveLearningModelArn) parameter\. To retrieve the model's ARN, use an AWS Command Line Interface \(AWS CLI\) command similar to the following\. 

```
# Fetch the mARN of the model trained in the final iteration of the previous labeling job.Ground Truth
pretrained_model_arn = sagemaker_client.describe_labeling_job(LabelingJobName=job_name)['LabelingJobOutput']['FinalActiveLearningModelArn']
```

To encrypt data on the storage volume attached to the ML compute instance\(s\) that are used in automated labeling, include an AWS Key Management Service \(AWS KMS\) key in the `VolumeKmsKeyId` parameter\. For information about AWS KMS keys see [What is AWS Key Management Service?](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html)*AWS Key Management Service Developer Guide*\.

For an example that uses the [ `CreateLabelingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html) operation to create an automated data labeling job, see the **object\_detection\_tutorial** example in the **SageMaker Examples**, **Ground Truth Labeling Jobs** section of a Amazon SageMaker notebook instance\. To learn how to create and open a notebook instance, see [Create a Notebook Instance](howitworks-create-ws.md)\. To learn how to access Amazon SageMaker example notebooks, see [Use Example Notebooks](howitworks-nbexamples.md)\.

## Amazon EC2 Instances Required for Automated Data Labeling<a name="sms-auto-labeling-ec2"></a>

The following table lists the Amazon Elastic Compute Cloud \(Amazon EC2\) instances that you need to run automated data labeling for training and batch inference jobs\.


| Automated Data Labeling Job Type | Training Instance Type | Inference Instance Type | 
| --- | --- | --- | 
|  Image classification  |  ml\.p3\.2xlarge\*  |  ml\.c5\.xlarge  | 
|  Object detection \(bounding box\)  |  ml\.p3\.2xlarge\*  |  ml\.c5\.4large  | 
|  Text classification  |  ml\.c5\.2xlarge  |  ml\.m4\.xlarge  | 
|  Semantic segmentation  |  ml\.p3\.2xlarge\*  |  ml\.p3\.2xlarge\*  | 

\* In the Asia Pacific \(Mumbai\) Region \(ap\-south\-1\) use ml\.p2\.8xlarge instead\.

****  
Automated data labeling incurs two separate charges: the per\-item charge \(for information, see [ pricing](http://aws.amazon.com/sagemaker/groundtruth/pricing/)\), and the charge for the Amazon EC2 instance required to run the model \(see [Amazon EC2 pricing](http://aws.amazon.com/ec2/pricing/on-demand/)\)\.

 Ground Truth manages the instances that you use for automated data labeling jobs\. It creates, configures, and terminates the instances as needed to perform your job\. These instances don't appear in your Amazon EC2 instance dashboard\.