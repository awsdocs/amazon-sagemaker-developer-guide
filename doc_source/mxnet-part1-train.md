# Step 2: Train a Model on Amazon SageMaker Using Apache MXNet Custom Code<a name="mxnet-part1-train"></a>

The high\-level Python library provides the `MXNet` class with two methods: `fit` \(for training a model\) and `deploy` \(for deploying a model\)\. 

**To train the model:**

1. Create an instance of the `sagemaker.mxnet.MXNet` class by copying, pasting, and running the following code:

   ```
   from sagemaker.mxnet import MXNet
   
   mnist_estimator = MXNet(entry_point='/home/ec2-user/sample-notebooks/sagemaker-python-sdk/mxnet_mnist/mnist.py',
                           role=role,
                           output_path=model_artifacts_location,
                           code_location=custom_code_upload_location,
                           train_instance_count=1, 
                           train_instance_type='ml.m4.xlarge',
                           hyperparameters={'learning_rate': 0.1})
   ```

   Some of these constructor parameters are sent in the `fit` method call for model training in the next step\. 

   Details:
   + `entry_point` — The example uses only one source file \(`mnist.py`\) and it is already provided for you on your notebook instance\. If your custom code is in one file, you specify only the `entry_point` parameter If your training code consists of multiple files, you also add the `source_dir` parameter\. 
**Note**  
You specify the source of only your custom code\. The `sagemaker.mxnet.MXNet` object determines which Docker image to use for model training\.
   + `role` —The IAM role that Amazon SageMaker assumes when performing tasks on your behalf, such as downloading training data from an S3 bucket for model training and uploading training results to an S3 bucket\.
   + `code_location`—S3 location where you want the `fit` method \(in the next step\) to upload the tar archive of your custom Apache MXNet code\.
   + `output_path` — Identifies S3 location where you want to the result of model training \(model artifacts\) saved\. 
   + `train_instance_count` and `train_instance_type` —You specify the number and type of instances to use for model training\.

     You can also train your model on your local computer by specifying `local` as the value for `train_instance_type` and `1` as the value for `train_instance_count`\. For more information about local mode, see [https://github.com/aws/sagemaker-python-sdk#local-mode](https://github.com/aws/sagemaker-python-sdk#local-mode) in the *Amazon SageMaker Python SDK*\.
   + `Hyperparameters` — Any hyperparameters that you specify to influence the final quality of the model\. Your custom training code uses these parameters\.

1. Start model training by copying, pasting, and running the following code: 

   ```
   %%time
   import boto3
   
   region = boto3.Session().region_name
   train_data_location = 's3://sagemaker-sample-data-{}/mxnet/mnist/train'.format(region)
   test_data_location = 's3://sagemaker-sample-data-{}/mxnet/mnist/test'.format(region)
   
   mnist_estimator.fit({'train': train_data_location, 'test': test_data_location})
   ```

In the `fit` call, you specify the S3 URI strings that identify where the training and test dataset are stored\. The `fit` method sends a [CreateTrainingJob](API_CreateTrainingJob.md) request to Amazon SageMaker\. 

You can get the training job information by calling the [DescribeTrainingJob](API_DescribeTrainingJob.md) or viewing it in the console\. The following is an example response:

```
{
    "AlgorithmSpecification": {
        "TrainingImage": "520713654638.dkr.ecr.us-west-2.amazonaws.com/sagemaker-mxnet-py2-cpu:1.0",
        "TrainingInputMode": "File"
    },
    "HyperParameters": {
        "learning_rate": "0.11",
        "sagemaker_submit_directory": "\"s3://sagemakermv/customcode/mxnet_mnist/sagemaker-mxnet-py2-cpu-2017-11-18-02-02-18-586/sourcedir.tar.gz\"",
        "sagemaker_program": "\"mnist.py\"",
        "sagemaker_container_log_level": "20",
        "sagemaker_job_name": "\"sagemaker-mxnet-py2-cpu-2017-11-18-02-02-18-586\"",
        "sagemaker_region": "\"us-west-2\""
    },
    "InputDataConfig": [
        {
            "DataSource": {
                "S3DataSource": {
                    "S3DataDistributionType": "FullyReplicated",
                    "S3DataType": "S3Prefix",
                    "S3Uri": "s3://sagemaker-sample-data-us-west-2/mxnet/mnist/train"
                }
            },
            "ChannelName": "train"
        },
        {
            "DataSource": {
                "S3DataSource": {
                    "S3DataDistributionType": "FullyReplicated",
                    "S3DataType": "S3Prefix",
                    "S3Uri": "s3://sagemaker-sample-data-us-west-2/mxnet/mnist/test"
                }
            },
            "ChannelName": "test"
        }
    ],
    "OutputDataConfig": {
        "S3OutputPath": "s3://sagemakermv/artifacts"
    },
    "TrainingJobName": "sagemaker-mxnet-py2-cpu-2017-11-18-02-02-18-586",
    "StoppingCondition": {
        "MaxRuntimeInSeconds": 86400
    },
    "ResourceConfig": {
        "InstanceCount": 1,
        "InstanceType": "ml.m4.xlarge",
        "VolumeSizeInGB": 30
    },
    "RoleArn": "arn:aws:iam::account-id:role/SageMakerRole"
}
```

Details:
+ `TrainingImage`— Amazon SageMaker runs this image to create a container for model training\. You don't explicitly identify this image in your request\. Instead, the  method dynamically chooses the appropriate image\. It inspects the Python version in the interpreter and the GPU capability of the ML compute instance type that you specified when creating the Apache MXNet object\. 
+ `Hyperparameters`—The request includes the hyperparameters that you specified when creating the `sagemaker.mxnet.MXNet` object\. In addition, the request includes the following additional hyperparameters \(all beginning with the prefix `sagemaker`\)\. The hyperparameters starting with prefix "sagemaker" are used by Amazon SageMaker to set up the training environment\. 
  + `sagemaker_submit_directory`—Identifies the custom training code in S3 location to use for model training\. 

    The high\-level Python library does several things for you\. In this case, the `fit` method first it creates gzipped tar archive from the custom code file\(s\), and uploads the archive to S3\. This archive is specified in this hyperparameter\. 
  + `sagemaker_program`—Identifies the primary module from which your training functions will be loaded \(it is the `entry_point` parameter you specified when creating the `sagemaker.mxnet.MXNet` object\.
  + `sagemaker_container_log_level` — Sets the Python logging level\.
+ `InputDataConfig`—Specifies two channels \(train and test\)\. Each channel is a named input source the training code consumes\.
+ `OutputDataConfig`—Identifies the S3 location where you want Amazon SageMaker to save training results \(model artifacts\)\. 

By default, the training job runs synchronously \(you see the output in the notebook\)\. 

## Next Step<a name="mxnet-part1-train-nexttopic"></a>

 [Step 3 : Deploy the Trained Model ](mxnet-example1-deploy.md) 