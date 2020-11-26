# Run a Warm Start Hyperparameter Tuning Job<a name="automatic-model-tuning-warm-start"></a>

Use warm start to start a hyperparameter tuning job using one or more previous tuning jobs as a starting point\. The results of previous tuning jobs are used to inform which combinations of hyperparameters to search over in the new tuning job\. Hyperparameter tuning uses either Bayesian or random search to choose combinations of hyperparameter values from ranges that you specify\. For more information, see [How Hyperparameter Tuning Works](automatic-model-tuning-how-it-works.md)\. Using information from previous hyperparameter tuning jobs can help increase the performance of the new hyperparameter tuning job by making the search for the best combination of hyperparameters more efficient\.

**Note**  
Warm start tuning jobs typically take longer to start than standard hyperparameter tuning jobs, because the results from the parent jobs have to be loaded before the job can start\. The increased time depends on the total number of training jobs launched by the parent jobs\.

Reasons you might want to consider warm start include:
+ You want to gradually increase the number of training jobs over several tuning jobs based on the results you see after each iteration\.
+ You get new data, and want to tune a model using the new data\.
+ You want to change the ranges of hyperparameters that you used in a previous tuning job, change static hyperparameters to tunable, or change tunable hyperparameters to static values\.
+ You stopped a previous hyperparameter job early or it stopped unexpectedly\.

**Topics**
+ [Types of Warm Start Tuning Jobs](#tuning-warm-start-types)
+ [Warm Start Tuning Restrictions](#warm-start-tuning-restrictions)
+ [Warm Start Tuning Sample Notebook](#warm-start-tuning-sample-notebooks)
+ [Create a Warm Start Tuning Job](#warm-start-tuning-example)

## Types of Warm Start Tuning Jobs<a name="tuning-warm-start-types"></a>

There are two different types of warm start tuning jobs:

`IDENTICAL_DATA_AND_ALGORITHM`  
The new hyperparameter tuning job uses the same input data and training image as the parent tuning jobs\. You can change the hyperparameter ranges to search and the maximum number of training jobs that the hyperparameter tuning job launches\. You can also change hyperparameters from tunable to static, and from static to tunable, but the total number of static plus tunable hyperparameters must remain the same as it is in all parent jobs\. You cannot use a new version of the training algorithm, unless the changes in the new version do not affect the algorithm itself\. For example, changes that improve logging or adding support for a different data format are allowed\.  
Use identical data and algorithm when you use the same training data as you used in a previous hyperparameter tuning job, but you want to increase the total number of training jobs or change ranges or values of hyperparameters\.  
When you run an warm start tuning job of type `IDENTICAL_DATA_AND_ALGORITHM`, there is an additional field in the response to [ `DescribeHyperParameterTuningJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeHyperParameterTuningJob.html) named `OverallBestTrainingJob`\. The value of this field is the [TrainingJobSummary](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_TrainingJobSummary.html) for the training job with the best objective metric value of all training jobs launched by this tuning job and all parent jobs specified for the warm start tuning job\.

`TRANSFER_LEARNING`  
The new hyperparameter tuning job can include input data, hyperparameter ranges, maximum number of concurrent training jobs, and maximum number of training jobs that are different than those of its parent hyperparameter tuning jobs\. You can also change hyperparameters from tunable to static, and from static to tunable, but the total number of static plus tunable hyperparameters must remain the same as it is in all parent jobs\. The training algorithm image can also be a different version from the version used in the parent hyperparameter tuning job\. When you use transfer learning, changes in the dataset or the algorithm that significantly affect the value of the objective metric might reduce the usefulness of using warm start tuning\.

## Warm Start Tuning Restrictions<a name="warm-start-tuning-restrictions"></a>

The following restrictions apply to all warm start tuning jobs:
+ A tuning job can have a maximum of 5 parent jobs, and all parent jobs must be in a terminal state \(`Completed`, `Stopped`, or `Failed`\) before you start the new tuning job\.
+ The objective metric used in the new tuning job must be the same as the objective metric used in the parent jobs\.
+ The total number of static plus tunable hyperparameters must remain the same between parent jobs and the new tuning job\. Because of this, if you think you might want to use a hyperparameter as tunable in a future warm start tuning job, you should add it as a static hyperparameter when you create a tuning job\.
+ The type of each hyperparameter \(continuous, integer, categorical\) must not change between parent jobs and the new tuning job\.
+ The number of total changes from tunable hyperparameters in the parent jobs to static hyperparameters in the new tuning job, plus the number of changes in the values of static hyperparameters cannot be more than 10\. Each value in a categorical hyperparameter counts against this limit\. For example, if the parent job has a tunable categorical hyperparameter with the possible values `red` and `blue`, you change that hyperparameter to static in the new tuning job, that counts as 2 changes against the allowed total of 10\. If the same hyperparameter had a static value of `red` in the parent job, and you change the static value to `blue` in the new tuning job, it also counts as 2 changes\.
+ Warm start tuning is not recursive\. For example, if you create `MyTuningJob3` as a warm start tuning job with `MyTuningJob2` as a parent job, and `MyTuningJob2` is itself an warm start tuning job with a parent job `MyTuningJob1`, the information that was learned when running `MyTuningJob1` is not used for `MyTuningJob3`\. If you want to use the information from `MyTuningJob1`, you must explicitly add it as a parent for `MyTuningJob3`\.
+ The training jobs launched by every parent job in a warm start tuning job count against the 500 maximum training jobs for a tuning job\.
+ Hyperparameter tuning jobs created before October 1, 2018 cannot be used as parent jobs for warm start tuning jobs\.

## Warm Start Tuning Sample Notebook<a name="warm-start-tuning-sample-notebooks"></a>

For a sample notebook that shows how to use warm start tuning, see [https://github\.com/awslabs/amazon\-sagemaker\-examples/blob/master/hyperparameter\_tuning/image\_classification\_warmstart/hpo\_image\_classification\_warmstart\.ipynb](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/hyperparameter_tuning/image_classification_warmstart/hpo_image_classification_warmstart.ipynb)\. For instructions how to create and access Jupyter notebook instances that you can use to run the example in SageMaker, see [Example Notebooks](howitworks-nbexamples.md)\. Once you have created a notebook instance and opened it, select the **SageMaker Examples** tab to see a list of all the SageMaker samples\. The warm start tuning example notebook is located in the **Hyperparameter tuning** section, and is named `hpo_image_classification_warmstart.ipynb`\. To open a notebook, click on its **Use** tab and select **Create copy**\.

## Create a Warm Start Tuning Job<a name="warm-start-tuning-example"></a>

You can use either the low\-level AWS SDK for Python \(Boto 3\) or the high\-level SageMaker Python SDK to create a warm start tuning job\.

**Topics**
+ [Create a Warm Start Tuning Job \( Low\-level SageMaker API for Python \(Boto 3\)\)](#warm-start-tuning-example-boto)
+ [Create a Warm Start Tuning Job \(SageMaker Python SDK\)](#warm-start-tuning-example-sdk)

### Create a Warm Start Tuning Job \( Low\-level SageMaker API for Python \(Boto 3\)\)<a name="warm-start-tuning-example-boto"></a>

To use warm start tuning, you specify the values of a [ `HyperParameterTuningJobWarmStartConfig`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HyperParameterTuningJobWarmStartConfig.html) object, and pass that as the `WarmStartConfig` field in a call to [ `CreateHyperParameterTuningJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateHyperParameterTuningJob.html)\.

The following code shows how to create a [ `HyperParameterTuningJobWarmStartConfig`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HyperParameterTuningJobWarmStartConfig.html) object and pass it to [ `CreateHyperParameterTuningJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateHyperParameterTuningJob.html) job by using the low\-level SageMaker API for Python \(Boto 3\)\.

Create the `HyperParameterTuningJobWarmStartConfig` object:

```
warm_start_config = {
          "ParentHyperParameterTuningJobs" : [
          {"HyperParameterTuningJobName" : 'MyParentTuningJob'}
          ],
          "WarmStartType" : "IdenticalDataAndAlgorithm"
}
```

Create the warm start tuning job:

```
smclient = boto3.Session().client('sagemaker')
smclient.create_hyper_parameter_tuning_job(HyperParameterTuningJobName = 'MyWarmStartTuningJob',
   HyperParameterTuningJobConfig = tuning_job_config, # See notebook for tuning configuration
   TrainingJobDefinition = training_job_definition, # See notebook for job definition
   WarmStartConfig = warm_start_config)
```

### Create a Warm Start Tuning Job \(SageMaker Python SDK\)<a name="warm-start-tuning-example-sdk"></a>

To use the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) to run a warm start tuning job, you:
+ Specify the parent jobs and the warm start type by using a `WarmStartConfig` object\.
+ Pass the `WarmStartConfig` object as the value of the `warm_start_config` argument of a [HyperparameterTuner](https://sagemaker.readthedocs.io/en/latest/tuner.html) object\.
+ Call the `fit` method of the `HyperparameterTuner` object\.

For more information about using the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) for hyperparameter tuning, see [https://github\.com/aws/sagemaker\-python\-sdk\#sagemaker\-automatic\-model\-tuning](https://github.com/aws/sagemaker-python-sdk#sagemaker-automatic-model-tuning)\.

This example uses an estimator that uses the [Image Classification Algorithm](image-classification.md) algorithm for training\. The following code sets the hyperparameter ranges that the warm start tuning job searches within to find the best combination of values\. For information about setting hyperparameter ranges, see [Define Hyperparameter Ranges](automatic-model-tuning-define-ranges.md)\.

```
hyperparameter_ranges = {'learning_rate': ContinuousParameter(0.0, 0.1),
                         'momentum': ContinuousParameter(0.0, 0.99)}
```

The following code configures the warm start tuning job by creating a `WarmStartConfig` object\.

```
from sagemaker.tuner import WarmStartConfig,
          WarmStartTypes

parent_tuning_job_name = "MyParentTuningJob"
warm_start_config = WarmStartConfig(type=WarmStartTypes.IDENTICAL_DATA_AND_ALGORITHM, parents={parent_tuning_job_name})
```

Now set the values for static hyperparameters, which are hyperparameters that keep the same value for every training job that the warm start tuning job launches\. In the following code, `imageclassification` is an estimator that was created previously\.

```
imageclassification.set_hyperparameters(num_layers=18,
                                        image_shape='3,224,224',
                                        num_classes=257,
                                        num_training_samples=15420,
                                        mini_batch_size=128,
                                        epochs=30,
                                        optimizer='sgd',
                                        top_k='2',
                                        precision_dtype='float32',
                                        augmentation_type='crop')
```

Now create the `HyperparameterTuner` object and pass the `WarmStartConfig` object that you previously created as the `warm_start_config` argument\.

```
tuner_warm_start = HyperparameterTuner(imageclassification,
                            'validation:accuracy',
                            hyperparameter_ranges,
                            objective_type='Maximize',
                            max_jobs=10,
                            max_parallel_jobs=2,
                            base_tuning_job_name='warmstart',
                            warm_start_config=warm_start_config)
```

Finally, call the `fit` method of the `HyperparameterTuner` object to launch the warm start tuning job\.

```
tuner_warm_start.fit(
        {'train': s3_input_train, 'validation': s3_input_validation},
        include_cls_metadata=False)
```