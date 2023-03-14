# Use an Algorithm to Run a Hyperparameter Tuning Job<a name="sagemaker-mkt-algo-tune"></a>

A hyperparameter tuning job finds the best version of a model by running many training jobs on your dataset using the algorithm and ranges of hyperparameters that you specify\. It then chooses the hyperparameter values that result in a model that performs the best, as measured by a metric that you choose\. For more information, see [Perform Automatic Model Tuning with SageMaker](automatic-model-tuning.md)\.

You can create use an algorithm resource to create a hyperparameter tuning job by using the Amazon SageMaker console, the low\-level Amazon SageMaker API, or the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\.

**Topics**
+ [Use an Algorithm to Run a Hyperparameter Tuning Job \(Console\)](#sagemaker-mkt-algo-tune-console)
+ [Use an Algorithm to Run a Hyperparameter Tuning Job \(API\)](#sagemaker-mkt-algo-tune-api)
+ [Use an Algorithm to Run a Hyperparameter Tuning Job \([Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\)](#sagemaker-mkt-algo-tune-sdk)

## Use an Algorithm to Run a Hyperparameter Tuning Job \(Console\)<a name="sagemaker-mkt-algo-tune-console"></a>

**To use an algorithm to run a hyperparameter tuning job \(console\)**

1. Open the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Algorithms**\.

1. Choose an algorithm that you created from the list on the **My algorithms** tab or choose an algorithm that you subscribed to on the **AWS Marketplace subscriptions** tab\.

1. Choose **Create hyperparameter tuning job**\.

   The algorithm you chose will automatically be selected\.

1. On the **Create hyperparameter tuning job** page, provide the following information:

   1. For **Warm start**, choose **Enable warm start** to use the information from previous hyperparameter tuning jobs as a starting point for this hyperparameter tuning job\. For more information, see [Run a Warm Start Hyperparameter Tuning Job](automatic-model-tuning-warm-start.md)\.

      1. Choose **Identical data and algorithm** if your input data is the same as the input data for the parent jobs of this hyperparameter tuning job, or choose **Transfer learning** to use additional or different input data for this hyperparameter tuning job\.

      1. For **Parent hyperparameter tuning job\(s\)**, choose up to 5 hyperparameter tuning jobs to use as parents to this hyperparameter tuning job\.

   1. For **Hyperparameter tuning job name**, type a name for the tuning job\.

   1. For **IAM role**, choose an IAM role that has the required permissions to run hyperparameter tuning jobs in SageMaker, or choose **Create a new role** to allow SageMaker to create a role that has the `AmazonSageMakerFullAccess` managed policy attached\. For information, see [SageMaker Roles](sagemaker-roles.md)\.

   1. For **VPC**, choose a Amazon VPC that you want to allow the training jobs that the tuning job launches to access\. For more information, see [Give SageMaker Training Jobs Access to Resources in Your Amazon VPC](train-vpc.md)\.

   1. Choose **Next**\.

   1. For **Objective metric**, choose the metric that the hyperparameter tuning job uses to determine the best combination of hyperparameters, and choose whether to minimize or maximize this metric\. For more information, see [View the Best Training Job](automatic-model-tuning-ex-tuning-job.md#automatic-model-tuning-best-training-job)\.

   1. For **Hyperparameter configuration**, choose ranges for the tunable hyperparameters that you want the tuning job to search, and set static values for hyperparameters that you want to remain constant in all training jobs that the hyperparameter tuning job launches\. For more information, see [Define Hyperparameter Ranges](automatic-model-tuning-define-ranges.md)\.

   1. Choose **Next**\.

   1. For **Input data configuration**, specify the following values for each channel of input data to use for the hyperparameter tuning job\. You can see what channels the algorithm you're using for hyperparameter tuning supports, and the content type, supported compression type, and supported input modes for each channel, under **Channel specification** section of the **Algorithm summary** page for the algorithm\.

      1. For **Channel name**, type the name of the input channel\.

      1. For **Content type**, type the content type of the data that the algorithm expects for the channel\.

      1. For **Compression type**, choose the data compression type to use, if any\.

      1. For **Record wrapper**, choose `RecordIO` if the algorithm expects data in the `RecordIO` format\.

      1. For **S3 data type**, **S3 data distribution type**, and **S3 location**, specify the appropriate values\. For information about what these values mean, see [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_S3DataSource.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_S3DataSource.html)\.

      1. For **Input mode**, choose **File** to download the data from to the provisioned ML storage volume, and mount the directory to a Docker volume\. Choose **Pipe**To stream data directly from Amazon S3 to the container\.

      1. To add another input channel, choose **Add channel**\. If you are finished adding input channels, choose **Done**\.

   1. For **Output** location, specify the following values:

      1. For **S3 output path**, choose the S3 location where the training jobs that this hyperparameter tuning job launches store output, such as model artifacts\.
**Note**  
You use the model artifacts stored at this location to create a model or model package from this hyperparameter tuning job\.

      1. For **Encryption key**, if you want SageMaker to use a AWS KMS key to encrypt output data at rest in the S3 location\.

   1. For **Resource configuration**, provide the following information:

      1. For **Instance type**, choose the instance type to use for each training job that the hyperparameter tuning job launches\.

      1. For **Instance count**, type the number of ML instances to use for each training job that the hyperparameter tuning job launches\.

      1. For **Additional volume per instance \(GB\)**, type the size of the ML storage volume that you want to provision each training job that the hyperparameter tuning job launches\. ML storage volumes store model artifacts and incremental states\.

      1. For **Encryption key**, if you want Amazon SageMaker to use an AWS Key Management Service key to encrypt data in the ML storage volume attached to the training instances, specify the key\.

   1. For **Resource limits**, provide the following information:

      1. For **Maximum training jobs**, specify the maximum number of training jobs that you want the hyperparameter tuning job to launch\. A hyperparameter tuning job can launch a maximum of 500 training jobs\.

      1. For **Maximum parallel training jobs**, specify the maximum number of concurrent training jobs that the hyperparameter tuning job can launch\. A hyperparameter tuning job can launch a maximum of 10 concurrent training jobs\.

      1. For **Stopping condition**, specify the maximum amount of time in seconds, minutes, hours, or days, that you want each training job that the hyperparameter tuning job launches to run\.

   1. For **Tags**, specify one or more tags to manage the hyperparameter tuning job\. Each tag consists of a key and an optional value\. Tag keys must be unique per resource\.

   1. Choose **Create jobs** to run the hyperparameter tuning job\.

## Use an Algorithm to Run a Hyperparameter Tuning Job \(API\)<a name="sagemaker-mkt-algo-tune-api"></a>

To use an algorithm to run a hyperparameter tuning job by using the SageMaker API, specify either the name or the Amazon Resource Name \(ARN\) of the algorithm as the `AlgorithmName` field of the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AlgorithmSpecification.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AlgorithmSpecification.html) object that you pass to [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateHyperParameterTuningJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateHyperParameterTuningJob.html)\. For information about hyperparameter tuning in SageMaker, see [Perform Automatic Model Tuning with SageMaker](automatic-model-tuning.md)\.

## Use an Algorithm to Run a Hyperparameter Tuning Job \([Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\)<a name="sagemaker-mkt-algo-tune-sdk"></a>

Use an algorithm that you created or subscribed to on AWS Marketplace to create a hyperparameter tuning job, create an `AlgorithmEstimator` object and specify either the Amazon Resource Name \(ARN\) or the name of the algorithm as the value of the `algorithm_arn` argument\. Then initialize a `HyperparameterTuner` object with the `AlgorithmEstimator` you created as the value of the `estimator` argument\. Finally, call the `fit` method of the `AlgorithmEstimator`\. For example:

```
from sagemaker import AlgorithmEstimator
from sagemaker.tuner import HyperparameterTuner

data_path = os.path.join(DATA_DIR, 'marketplace', 'training')

algo = AlgorithmEstimator(
            algorithm_arn='arn:aws:sagemaker:us-east-2:764419575721:algorithm/scikit-decision-trees-1542410022',
            role='SageMakerRole',
            instance_count=1,
            instance_type='ml.c4.xlarge',
            sagemaker_session=sagemaker_session,
            base_job_name='test-marketplace')

train_input = algo.sagemaker_session.upload_data(
    path=data_path, key_prefix='integ-test-data/marketplace/train')

algo.set_hyperparameters(max_leaf_nodes=10)
tuner = HyperparameterTuner(estimator=algo, base_tuning_job_name='some-name',
                                objective_metric_name='validation:accuracy',
                                hyperparameter_ranges=hyperparameter_ranges,
                                max_jobs=2, max_parallel_jobs=2)

tuner.fit({'training': train_input}, include_cls_metadata=False)
tuner.wait()
```