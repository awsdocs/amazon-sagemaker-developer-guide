# Step 5: Train a Model<a name="ex1-train-model"></a>

To train, deploy, and validate a model in Amazon SageMaker, you can use either the Amazon SageMaker Python SDK or the AWS SDK for Python \(Boto3\)\. \(You can also use the console, but for this exercise, you will use the notebook instance and one of the SDKs\.\) This exercise provides code examples for each library\. 

The [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) abstracts several implementation details, and is easy to use\. If you're a first\-time SageMaker user, we recommend that you use it to train, deploy, and validate the model\. For more information, see [https://sagemaker\.readthedocs\.io/en/stable/overview\.html](https://sagemaker.readthedocs.io/en/stable/overview.html)\.

**Topics**
+ [Choose the Training Algorithm](#ex1-train-model-select-algorithm)
+ [Create and Run a Training Job \([Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\)](#ex1-train-model-sdk)
+ [Create and Run a Training Job \(AWS SDK for Python \(Boto3\)\)](#ex1-train-model-create-training-job)

## Choose the Training Algorithm<a name="ex1-train-model-select-algorithm"></a>

To choose the right algorithm for your model, you typically follow an evaluation process\. For this exercise, you use the [XGBoost Algorithm](xgboost.md) provided by SageMaker, so no evaluation is required\. For information about choosing algorithms, see [Use Amazon SageMaker built\-in algorithms](algos.md)\.

## Create and Run a Training Job \([Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\)<a name="ex1-train-model-sdk"></a>

The [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) includes the `sagemaker.estimator.Estimator` estimator\. You can use this class, in the `sagemaker.estimator` module, with any algorithm\. For more information, see [https://sagemaker\.readthedocs\.io/en/stable/estimators\.html\#sagemaker\.estimator\.Estimator](https://sagemaker.readthedocs.io/en/stable/estimators.html#sagemaker.estimator.Estimator)\. 

**To run a model training job \([Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\)**

1. Import the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) and get the XGBoost container\.

   ```
   import sagemaker
   
   from sagemaker.amazon.amazon_estimator import get_image_uri
   
   container = get_image_uri(boto3.Session().region_name, 'xgboost')
   ```

1. Download the training and validation data from the Amazon S3 location where you uploaded it in [Step 4\.3: Transform the Training Dataset and Upload It to Amazon S3](ex1-preprocess-data-transform.md), and set the location where you store the training output\.

   ```
   train_data = 's3://{}/{}/{}'.format(bucket, prefix, 'train')
   
   validation_data = 's3://{}/{}/{}'.format(bucket, prefix, 'validation')
   
   s3_output_location = 's3://{}/{}/{}'.format(bucket, prefix, 'xgboost_model_sdk')
   print(train_data)
   ```

1. Create an instance of the `sagemaker.estimator.Estimator` class\. 

   ```
   xgb_model = sagemaker.estimator.Estimator(container,
                                            role, 
                                            train_instance_count=1, 
                                            train_instance_type='ml.m4.xlarge',
                                            train_volume_size = 5,
                                            output_path=s3_output_location,
                                            sagemaker_session=sagemaker.Session())
   ```

   In the constructor, you specify the following parameters:
   + `role` – The AWS Identity and Access Management \(IAM\) role that SageMaker can assume to perform tasks on your behalf \(for example, reading training results, called model artifacts, from the S3 bucket and writing training results to Amazon S3\)\. This is the role that you got in [Step 3: Create a Jupyter Notebook](ex1-prepare.md)\.
   + `train_instance_count` and `train_instance_type` – The type and number of ML compute instances to use for model training\. For this exercise, you use only a single training instance\.
   + `train_volume_size` – The size, in GB, of the Amazon Elastic Block Store \(Amazon EBS\) storage volume to attach to the training instance\. This must be large enough to store training data if you use `File` mode \(`File` mode is the default\)\.
   + `output_path` – The  path to the S3 bucket where SageMaker stores the training results\.
   + `sagemaker_session` – The session object that manages interactions with SageMaker APIs and any other AWS service that the training job uses\.

1. Set the hyperparameter values for the XGBoost training job by calling the `set_hyperparameters` method of the estimator\. For a description of XGBoost hyperparameters, see [XGBoost Hyperparameters](xgboost_hyperparameters.md)\.

   ```
   xgb_model.set_hyperparameters(max_depth = 5,
                                 eta = .2,
                                 gamma = 4,
                                 min_child_weight = 6,
                                 silent = 0,
                                 objective = "multi:softmax",
                                 num_class = 10,
                                 num_round = 10)
   ```

1. Create the training channels to use for the training job\. For this example, we use both `train` and `validation` channels\.

   ```
   train_channel = sagemaker.session.s3_input(train_data, content_type='text/csv')
   valid_channel = sagemaker.session.s3_input(validation_data, content_type='text/csv')
   
   data_channels = {'train': train_channel, 'validation': valid_channel}
   ```

1. To start model training, call the estimator's `fit` method\. 

   ```
   xgb_model.fit(inputs=data_channels,  logs=True)
   ```

   This is a synchronous operation\. The method displays progress logs and waits until training completes before returning\. For more information about model training, see [Train a Model with Amazon SageMaker](how-it-works-training.md)\.

   Model training for this exercise can take up to 15 minutes\.

**Next Step**  
[Step 6: Deploy the Model to Amazon SageMaker](ex1-model-deployment.md)

## Create and Run a Training Job \(AWS SDK for Python \(Boto3\)\)<a name="ex1-train-model-create-training-job"></a>

To train a model, SageMaker uses the [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) API\. The AWS SDK for Python \(Boto3\) provides the corresponding `create_training_job` method\. 

When using this method, you provide the following information:
+ The training algorithm – Specify the registry path of the Docker image that contains the training code\. For the registry paths for the algorithms provided by SageMaker, see [Common parameters for built\-in algorithms](sagemaker-algo-docker-registry-paths.md)\.
+ Algorithm\-specific hyperparameters – Specify algorithm\-specific hyperparameters to influence the final quality of the model\. For information, see [XGBoost Hyperparameters](xgboost_hyperparameters.md)\.
+ The input and output configuration – Provide the S3 bucket where training data is stored and where SageMaker saves the results of model training \(the model artifacts\)\. 

**To run a model training job \(AWS SDK for Python \(Boto3\)\)**

1. Import the `get_image_url` utility function [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) and get the location of the XGBoost container\.

   ```
   import sagemaker
   
   from sagemaker.amazon.amazon_estimator import get_image_uri
   
   container = get_image_uri(boto3.Session().region_name, 'xgboost')
   ```

1. Set up the training information for the job\. You pass this information when you call `create_training_job`\. For more information about the information that you need to send to a training job, see [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html)\.

   ```
   #Ensure that the train and validation data folders generated above are reflected in the "InputDataConfig" parameter below.
   common_training_params = \
   {
       "AlgorithmSpecification": {
           "TrainingImage": container,
           "TrainingInputMode": "File"
       },
       "RoleArn": role,
       "OutputDataConfig": {
           "S3OutputPath": bucket_path + "/"+ prefix + "/xgboost"
       },
       "ResourceConfig": {
           "InstanceCount": 1,   
           "InstanceType": "ml.m4.xlarge",
           "VolumeSizeInGB": 5
       },
       "HyperParameters": {
           "max_depth":"5",
           "eta":"0.2",
           "gamma":"4",
           "min_child_weight":"6",
           "silent":"0",
           "objective": "multi:softmax",
           "num_class": "10",
           "num_round": "10"
       },
       "StoppingCondition": {
           "MaxRuntimeInSeconds": 86400
       },
       "InputDataConfig": [
           {
               "ChannelName": "train",
               "DataSource": {
                   "S3DataSource": {
                       "S3DataType": "S3Prefix",
                       "S3Uri": bucket_path + "/"+ prefix+ '/train/',
                       "S3DataDistributionType": "FullyReplicated" 
                   }
               },
               "ContentType": "text/csv",
               "CompressionType": "None"
           },
           {
               "ChannelName": "validation",
               "DataSource": {
                   "S3DataSource": {
                       "S3DataType": "S3Prefix",
                       "S3Uri": bucket_path + "/"+ prefix+ '/validation/',
                       "S3DataDistributionType": "FullyReplicated"
                   }
               },
               "ContentType": "text/csv",
               "CompressionType": "None"
           }
       ]
   }
   ```

1. Name your training job, and finish configuring the parameters that you send to it\.

   ```
   #training job params
   training_job_name = 'xgboost-mnist' + strftime("%Y-%m-%d-%H-%M-%S", gmtime())
   print("Job name is:", training_job_name)
   
   training_job_params = copy.deepcopy(common_training_params)
   training_job_params['TrainingJobName'] = training_job_name
   training_job_params['ResourceConfig']['InstanceCount'] = 1
   ```

1. Call `create_training_job` to start the training job, and wait for it to complete\. If the training job fails, print the reason that it failed\.

   ```
   %%time
   
   region = boto3.Session().region_name
   sm = boto3.Session().client('sagemaker')
   
   sm.create_training_job(**training_job_params)
   
   
   status = sm.describe_training_job(TrainingJobName=training_job_name)['TrainingJobStatus']
   print(status)
   sm.get_waiter('training_job_completed_or_stopped').wait(TrainingJobName=training_job_name)
   status = sm.describe_training_job(TrainingJobName=training_job_name)['TrainingJobStatus']
   print("Training job ended with status: " + status)
   if status == 'Failed':
       message = sm.describe_training_job(TrainingJobName=training_job_name)['FailureReason']
       print('Training failed with the following error: {}'.format(message))
       raise Exception('Training job failed')
   ```

You now have a trained model\. SageMaker stores the resulting artifacts in your S3 bucket\. 

**Next Step**  
[Step 6: Deploy the Model to Amazon SageMaker](ex1-model-deployment.md)