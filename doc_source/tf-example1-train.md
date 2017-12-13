# Step 2: Train a Model on Amazon SageMaker Using TensorFlow Custom Code<a name="tf-example1-train"></a>

The high\-level Python library provides the `TensorFlow` class, which has two methods: `fit` \(for training a model\) and `deploy` \(for deploying a model\)\. 

**To train a model**

1. Create an instance of the `sagemaker.tensorflow.TensorFlow` class by copying, pasting, and running the following code: 

   ```
   from sagemaker.tensorflow import TensorFlow
   
   iris_estimator = TensorFlow(entry_point='/home/ec2-user/sample-notebooks/sagemaker-python-sdk/tensorflow_iris_dnn_classifier_using_estimators/iris_dnn_classifier.py',
                               role=role,
                               output_path=model_artifacts_location,
                               code_location=custom_code_upload_location,
                               train_instance_count=1,
                               train_instance_type='ml.c4.xlarge',
                               training_steps=1000,
                               evaluation_steps=100)
   ```

   Some of these constructor parameters are sent in the `fit` method call for model training in the next step\. 

   Details:

   + `entry_point`—The example uses only one source file \(`iris_dnn_classifier.py`\) and it is already provided for you on your notebook instance\. If your custom training code is stored in a single file, specify only the `entry_point` parameter\. If it's stored in multiple files, also add the `source_dir` parameter\. 
**Note**  
Specify only the source file that contains your custom code\. The `sagemaker.tensorflow.TensorFlow` object determines which Docker image to use for model training when you call the `fit` method in the next step\.

   + `output_path`—Identifies the S3 location where you want to save the result of model training \(model artifacts\)\. 

   + `code_location`—S3 location where you want the `fit` method \(in the next step\) to upload the tar archive of your custom TensorFlow code\.

   + `role`—Identifies the IAM role that Amazon SageMaker assumes when performing tasks on your behalf, such as downloading training data from an S3 bucket for model training and uploading training results to an S3 bucket\.

   + `hyperparameters` \- Any hyperparameters that you specify to influence the final quality of the model\. Your custom training code uses these parameters\.

   + `train_instance_type` and `train_instance_count`—Identify the type and number of ML Compute instances to launch for model training\.

1. Start model training by copying, pasting, and running the following code: 

   ```
   %%time
   import boto3
   
   region = boto3.Session().region_name
   train_data_location = 's3://sagemaker-sample-data-{}/tensorflow/iris'.format(region)
   
   iris_estimator.fit(train_data_location)
   ```

The `fit` method parameter identifies the S3 location where the training data is stored\. The `fit` method sends a [CreateTrainingJob](API_CreateTrainingJob.md) request to Amazon SageMaker\. 

You can get the training job information by calling the [DescribeTrainingJob](API_DescribeTrainingJob.md) or viewing it in the console\. The following is an example response:

```
{
    "InputDataConfig": [
        {
            "ChannelName": "training", 
            "DataSource": {
                "S3DataSource": {
                    "S3DataType": "S3Prefix", 
                    "S3DataDistributionType": "FullyReplicated", 
                    "S3Uri": "s3://sagemaker-sample-data-us-west-2/tensorflow/iris"
                }
            }
        }
    ], 
    "OutputDataConfig": {
        "S3OutputPath": "s3://sagemakermv/artifacts"
    }, 
    "StoppingCondition": {
        "MaxRuntimeInSeconds": 86400
    }, 
    "TrainingJobName": "sagemaker-tensorflow-py2-cpu-2017-11-18-03-11-11-686", 
    "AlgorithmSpecification": {
        "TrainingInputMode": "File", 
        "TrainingImage": "142577830533.dkr.ecr.us-west-2.amazonaws.com/sagemaker-tensorflow-py2-cpu:1.0.5"
    }, 
    "HyperParameters": {
        "sagemaker_program": "\"iris_dnn_classifier.py\"", 
        "checkpoint_path": "\"s3://sagemakermv/artifacts/checkpoints\"", 
        "sagemaker_job_name": "\"sagemaker-tensorflow-py2-cpu-2017-11-18-03-11-11-686\"", 
        "sagemaker_submit_directory": "\"s3://sagemakermv/customcode/tensorflow_iris/sagemaker-tensorflow-py2-cpu-2017-11-18-03-11-11-686/sourcedir.tar.gz\"", 
        "sagemaker_region": "\"us-west-2\"", 
        "training_steps": "100", 
        "sagemaker_container_log_level": "20"
    }, 
    "ResourceConfig": {
        "VolumeSizeInGB": 30, 
        "InstanceCount": 1, 
        "InstanceType": "ml.c4.xlarge"
    }, 
    "RoleArn": "arn:aws:iam::account-id:role/SageMakerRole"
}
```

Details:

+ `TrainingImage`—Amazon SageMaker runs this image to create a container for model training\. You don't explicitly identify this image in your request\. The `fit` method dynamically chooses the correct image by inspecting the Python version in the interpreter and the GPU capability of the ML compute instance type that you specified when creating the TensorFlow object\. 

+ `Hyperparameters`—The request includes the hyperparameters that you specified when you created the `sagemaker.tensorflow.TensorFlow` object\. It also includes the following additional hyperparameters, which have the prefix `sagemaker`\. Amazon SageMaker uses these hyperparameters to set up the training environment\. 

  + `sagemaker_submit_directory`—Identifies the S3 location of the custom training code\. 

    The high\-level Python library does several things for you\. In this case, the `fit` method creates a gzipped tar archive from the custom code file\(s\), and uploads the archive to an S3 bucket\. You specify this archive in this hyperparameter\. 

  + `sagemaker_program`—Identifies the primary module that your training functions will be loaded from\. This is the `entry_point` parameter that you specified when you created the `sagemaker.tensorflow.TensorFlow` object\.

  + `sagemaker_container_log_level`—Sets the Python logging level\. 

  + `sagemaker_job_name`—Amazon SageMaker uses the job name to publish CloudWatch metrics in your account\.

  + `sagemaker_checkpoint_path`—In distributed training, TensorFlow uses this S3 location as a shared file system for the ML compute instances running the training\.

+ `InputDataConfig`—Specifies one channel\. A channel is a named input source that the training code consumes\. 

+ `OutputDataConfig`—Identifies the S3 location where you want to save training results \(model artifacts\)\. 

By default, the training job runs synchronously \(you see the output in the notebook\)\. If you want it to run asynchronously, set the `wait` value to `false` in the call to the `fit` method or when you create the `sagemaker.tensorflow.TensorFlow` instance\. 

## Next Step<a name="tf-example1-train-nexttopic"></a>

 [Step 3: Deploy the Trained Model ](tf-example1-deploy.md) 