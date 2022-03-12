# Use an Algorithm to Run a Training Job<a name="sagemaker-mkt-algo-train"></a>

You can create use an algorithm resource to create a training job by using the Amazon SageMaker console, the low\-level Amazon SageMaker API, or the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\.

**Topics**
+ [Use an Algorithm to Run a Training Job \(Console\)](#sagemaker-mkt-algo-train-console)
+ [Use an Algorithm to Run a Training Job \(API\)](#sagemaker-mkt-algo-train-api)
+ [Use an Algorithm to Run a Training Job \([Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\)](#sagemaker-mkt-algo-train-sdk)

## Use an Algorithm to Run a Training Job \(Console\)<a name="sagemaker-mkt-algo-train-console"></a>

**To use an algorithm to run a training job \(console\)**

1. Open the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Algorithms**\.

1. Choose an algorithm that you created from the list on the **My algorithms** tab or choose an algorithm that you subscribed to on the **AWS Marketplace subscriptions** tab\.

1. Choose **Create training job**\.

   The algorithm you chose will automatically be selected\.

1. On the **Create training job** page, provide the following information:

   1. For **Job name**, type a name for the training job\.

   1. For **IAM role**, choose an IAM role that has the required permissions to run training jobs in SageMaker, or choose **Create a new role** to allow SageMaker to create a role that has the `AmazonSageMakerFullAccess` managed policy attached\. For information, see [SageMaker Roles](sagemaker-roles.md)\.

   1. For **Resource configuration**, provide the following information:

      1. For **Instance type**, choose the instance type to use for training\.

      1. For **Instance count**, type the number of ML instances to use for the training job\.

      1. For **Additional volume per instance \(GB\)**, type the size of the ML storage volume that you want to provision\. ML storage volumes store model artifacts and incremental states\.

      1. For **Encryption key**, if you want Amazon SageMaker to use an AWS Key Management Service key to encrypt data in the ML storage volume attached to the training instance, specify the key\.

      1. For **Stopping condition**, specify the maximum amount of time in seconds, minutes, hours, or days, that you want the training job to run\.

   1. For **VPC**, choose a Amazon VPC that you want to allow your training container to access\. For more information, see [Give SageMaker Training Jobs Access to Resources in Your Amazon VPC](train-vpc.md)\.

   1. For **Hyperparameters**, specify the values of the hyperparameters to use for the training job\.

   1. For **Input data configuration**, specify the following values for each channel of input data to use for the training job\. You can see what channels the algorithm you're using for training support, and the content type, supported compression type, and supported input modes for each channel, under **Channel specification** section of the **Algorithm summary** page for the algorithm\.

      1. For **Channel name**, type the name of the input channel\.

      1. For **Content type**, type the content type of the data that the algorithm expects for the channel\.

      1. For **Compression type**, choose the data compression type to use, if any\.

      1. For **Record wrapper**, choose `RecordIO` if the algorithm expects data in the `RecordIO` format\.

      1. For **S3 data type**, **S3 data distribution type**, and **S3 location**, specify the appropriate values\. For information about what these values mean, see [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_S3DataSource.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_S3DataSource.html)\.

      1. For **Input mode**, choose **File** to download the data from to the provisioned ML storage volume, and mount the directory to a Docker volume\. Choose **Pipe**To stream data directly from Amazon S3 to the container\.

      1. To add another input channel, choose **Add channel**\. If you are finished adding input channels, choose **Done**\.

   1. For **Output** location, specify the following values:

      1. For **S3 output path**, choose the S3 location where the training job stores output, such as model artifacts\.
**Note**  
You use the model artifacts stored at this location to create a model or model package from this training job\.

      1. For **Encryption key**, if you want SageMaker to use a AWS KMS key to encrypt output data at rest in the S3 location\.

   1. For **Tags**, specify one or more tags to manage the training job\. Each tag consists of a key and an optional value\. Tag keys must be unique per resource\.

   1. Choose **Create training job** to run the training job\.

## Use an Algorithm to Run a Training Job \(API\)<a name="sagemaker-mkt-algo-train-api"></a>

To use an algorithm to run a training job by using the SageMaker API, specify either the name or the Amazon Resource Name \(ARN\) as the `AlgorithmName` field of the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AlgorithmSpecification.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AlgorithmSpecification.html) object that you pass to [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html)\. For information about training models in SageMaker, see [Train a Model with Amazon SageMaker](how-it-works-training.md)\.

## Use an Algorithm to Run a Training Job \([Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\)<a name="sagemaker-mkt-algo-train-sdk"></a>

Use an algorithm that you created or subscribed to on AWS Marketplace to create a training job, create an `AlgorithmEstimator` object and specify either the Amazon Resource Name \(ARN\) or the name of the algorithm as the value of the `algorithm_arn` argument\. Then call the `fit` method of the estimator\. For example:

```
from sagemaker import AlgorithmEstimator
data_path = os.path.join(DATA_DIR, 'marketplace', 'training')

algo = AlgorithmEstimator(
algorithm_arn='arn:aws:sagemaker:us-east-2:012345678901:algorithm/my-algorithm',
        role='SageMakerRole',
        instance_count=1,
        instance_type='ml.c4.xlarge',
        sagemaker_session=sagemaker_session,
        base_job_name='test-marketplace')

train_input = algo.sagemaker_session.upload_data(
path=data_path, key_prefix='integ-test-data/marketplace/train')

algo.fit({'training': train_input})
```