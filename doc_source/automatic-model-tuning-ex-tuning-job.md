# Configure and Launch a Hyperparameter Tuning Job<a name="automatic-model-tuning-ex-tuning-job"></a>

To configure and launch a hyperparameter tuning job, complete the following steps\.

**Topics**
+ [Specify the Hyperparameter Tuning Job Settings](#automatic-model-tuning-ex-low-tuning-config)
+ [Configure the Training Jobs](#automatic-model-tuning-ex-low-training-def)
+ [Launch the Hyperparameter Tuning Job](#automatic-model-tuning-ex-low-launch)
+ [Next Step](#automatic-model-tuning-ex-next-monitor)

## Specify the Hyperparameter Tuning Job Settings<a name="automatic-model-tuning-ex-low-tuning-config"></a>

To specify settings for the hyperparameter tuning job, you define a JSON object\. You pass the object as the value of the `HyperParameterTuningJobConfig` parameter to the [CreateHyperParameterTuningJob](API_CreateHyperParameterTuningJob.md) call\.

In this JSON object, you specify:
+ The ranges of hyperparameters that you want to tune\.
+ The limits of the resource that the hyperparameter tuning job can consume\.
+ The objective metric for the hyperparameter tuning job\. An *objective metric* is the metric that the hyperparameter tuning job uses to evaluate the training job that it launches\.

The hyperparameter tuning job defines ranges for the `eta`, `alpha`, `min_child_weight`, and `max_depth` hyperparameters of the [XGBoost Algorithm](xgboost.md) built\-in algorithm\. The objective metric for the hyperparameter tuning job maximizes the `validation:auc` metric that the algorithm sends to CloudWatch Logs\.

```
tuning_job_config = {
    "ParameterRanges": {
      "CategoricalParameterRanges": [],
      "ContinuousParameterRanges": [
        {
          "MaxValue": "1",
          "MinValue": "0",
          "Name": "eta"
        },
        {
          "MaxValue": "2",
          "MinValue": "0",
          "Name": "alpha"
        },
        {
          "MaxValue": "10",
          "MinValue": "1",
          "Name": "min_child_weight"
        }
      ],
      "IntegerParameterRanges": [
        {
          "MaxValue": "10",
          "MinValue": "1",
          "Name": "max_depth"
        }
      ]
    },
    "ResourceLimits": {
      "MaxNumberOfTrainingJobs": 20,
      "MaxParallelTrainingJobs": 3
    },
    "Strategy": "Bayesian",
    "HyperParameterTuningJobObjective": {
      "MetricName": "validation:auc",
      "Type": "Maximize"
    }
  }
```

## Configure the Training Jobs<a name="automatic-model-tuning-ex-low-training-def"></a>

To configure the training jobs that the tuning job launches, define a JSON object that you pass as the value of the `TrainingJobDefinition` parameter of the [CreateHyperParameterTuningJob](API_CreateHyperParameterTuningJob.md) call\.

In this JSON object, you specify:
+ Optional—Metrics that the training jobs emit\.
**Note**  
Specify metrics only when you use a custom training algorithm\. Because this example uses a built\-in algorithm, you don't specify metrics\.
+ The container image that specifies the training algorithm\.
+ The input configuration for your training and test data\.
+ The configuration for the algorithm's output\. Specify the S3 bucket where you want to store the output of the training jobs\.
+ The values of algorithm hyperparameters that are not tuned in the tuning job\.
+ The type of instance to use for the training jobs\.
+ The stopping condition for the training jobs\. This is the maximum duration for each training job\.

In this example, we set static values for the `eval_metric`, `num_round`, `objective`, `rate_drop`, and `tweedie_variance_power` parameters of the [XGBoost Algorithm](xgboost.md) built\-in algorithm\.

```
containers = {'us-west-2': '433757028032.dkr.ecr.us-west-2.amazonaws.com/xgboost:latest',
              'us-east-1': '811284229777.dkr.ecr.us-east-1.amazonaws.com/xgboost:latest',
              'us-east-2': '825641698319.dkr.ecr.us-east-2.amazonaws.com/xgboost:latest',
              'eu-west-1': '685385470294.dkr.ecr.eu-west-1.amazonaws.com/xgboost:latest'}
           
training_image = containers[region]

s3_input_train = 's3://{}/{}/train'.format(bucket, prefix)
s3_input_validation ='s3://{}/{}/validation/'.format(bucket, prefix)
     
training_job_definition = {
    "AlgorithmSpecification": {
      "TrainingImage": training_image,
      "TrainingInputMode": "File"
    },
    "InputDataConfig": [
      {
        "ChannelName": "train",
        "CompressionType": "None",
        "ContentType": "csv",
        "DataSource": {
          "S3DataSource": {
            "S3DataDistributionType": "FullyReplicated",
            "S3DataType": "S3Prefix",
            "S3Uri": s3_input_train
          }
        }
      },
      {
        "ChannelName": "validation",
        "CompressionType": "None",
        "ContentType": "csv",
        "DataSource": {
          "S3DataSource": {
            "S3DataDistributionType": "FullyReplicated",
            "S3DataType": "S3Prefix",
            "S3Uri": s3_input_validation
          }
        }
      }
    ],
    "OutputDataConfig": {
      "S3OutputPath": "s3://{}/{}/output".format(bucket,prefix)
    },
    "ResourceConfig": {
      "InstanceCount": 2,
      "InstanceType": "ml.c4.2xlarge",
      "VolumeSizeInGB": 10
    },
    "RoleArn": role,
    "StaticHyperParameters": {
      "eval_metric": "auc",
      "num_round": "100",
      "objective": "binary:logistic",
      "rate_drop": "0.3",
      "tweedie_variance_power": "1.4"
    },
    "StoppingCondition": {
      "MaxRuntimeInSeconds": 43200
    }
}
```

## Launch the Hyperparameter Tuning Job<a name="automatic-model-tuning-ex-low-launch"></a>

Now you can launch the hyperparameter tuning job by calling the [CreateHyperParameterTuningJob](API_CreateHyperParameterTuningJob.md) API\. Pass the name and JSON objects that you created in previous steps as the values of the parameters\.

```
smclient.create_hyper_parameter_tuning_job(HyperParameterTuningJobName = tuning_job_name,
                                            HyperParameterTuningJobConfig = tuning_job_config,
                                            TrainingJobDefinition = training_job_definition)
```

## Next Step<a name="automatic-model-tuning-ex-next-monitor"></a>

[Monitor the Progress of a Hyperparameter Tuning Job](automatic-model-tuning-monitor.md)