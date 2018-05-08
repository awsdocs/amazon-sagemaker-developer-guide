# Step 3\.3\.2: Create a Training Job<a name="ex1-train-model-create-training-job"></a>

To train a model, Amazon SageMaker provides the [CreateTrainingJob](API_CreateTrainingJob.md) API\. You provide the following information when making this API call:
+ The training algorithm—Specify the registry path of the Docker image that contains the training code\. For the registry paths for the algorithms provided by Amazon SageMaker, see [Algorithms Provided by Amazon SageMaker: Common Parameters ](sagemaker-algo-docker-registry-paths.md)\. In the following examples, when using the high\-level Python library, you don't need to explicitly specify this path\. The `sagemaker.amazon.kmeans.KMeans` object knows the path\.
+ Algorithm\-specific hyperparameters—Specify algorithm\-specific hyperparameters to influence the final quality of the model\. For information, see [K\-Means Hyperparameters](k-means-api-config.md)\.
+ The input and output configuration—Provide the S3 bucket where training data is stored and where Amazon SageMaker saves the results of model training \(the model artifacts\)\. 

The low\-level AWS SDK for Python provides the corresponding `create_training_job` method and the high\-level Python library provide the `fit` method\. 

To train the model, choose one of the following options\. 
+ **Use the high\-level Python library provided by Amazon SageMaker**\.

  This Python library provides the `KMeans` estimator, which is a class in the `sagemaker.amazon.kmeans.KMeans` module\. To start model training, call the `fit` method\. 

  1. Create an instance of the `sagemaker.amazon.kmeans.KMeans` class\. 

     ```
     from sagemaker import KMeans
     
     data_location = 's3://{}/kmeans_highlevel_example/data'.format(bucket)
     output_location = 's3://{}/kmeans_example/output'.format(bucket)
     
     print('training data will be uploaded to: {}'.format(data_location))
     print('training artifacts will be uploaded to: {}'.format(output_location))
     
     kmeans = KMeans(role=role,
                     train_instance_count=2,
                     train_instance_type='ml.c4.8xlarge',
                     output_path=output_location,
                     k=10,
                     data_location=data_location)
     ```

     In the constructor, you specify the following parameters:
     + `role`— The IAM role that Amazon SageMaker can assume to perform tasks on your behalf \(for example, reading training results, called model artifacts, from the S3 bucket and writing training results to Amazon S3\)\.
     + `output_path`—The S3 location where Amazon SageMaker stores the training results\.
     + `train_instance_count` and `train_instance_type`—The type and number of ML compute instances to use for model training\.
     + `k`—The number of clusters to create\. For more information, see [K\-Means Hyperparameters](k-means-api-config.md)\.
     + `data_location`—The S3 location where the high\-level library uploads the transformed training data\. 

  1. To start model training, call the `KMeans` estimator's `fit` method\. 

     ```
     %%time
     
     kmeans.fit(kmeans.record_set(train_set[0]))
     ```

     This is a synchronous operation\. The method displays progress logs and waits until training completes before returning\. For more information about model training, see [Training a Model with Amazon SageMaker ](how-it-works-training.md)\.

     The model training in this example takes about 15 minutes\.
+ **Use the SDK for Python**\.

  This low\-level SDK for Python provides the `create_training_job` method, which maps to the [CreateTrainingJob](API_CreateTrainingJob.md) Amazon SageMaker API\. 

  ```
  %%time
  import boto3
  from time import gmtime, strftime
  
  job_name = 'kmeans-lowlevel-' + strftime("%Y-%m-%d-%H-%M-%S", gmtime())
  print("Training job", job_name)
  
  images = {'us-west-2': '174872318107.dkr.ecr.us-west-2.amazonaws.com/kmeans:latest',
            'us-east-1': '382416733822.dkr.ecr.us-east-1.amazonaws.com/kmeans:latest',
            'us-east-2': '404615174143.dkr.ecr.us-east-2.amazonaws.com/kmeans:latest',
            'eu-west-1': '438346466558.dkr.ecr.eu-west-1.amazonaws.com/kmeans:latest'}
  image = images[boto3.Session().region_name]
  
  output_location = 's3://{}/kmeans_example/output'.format(bucket)
  print('training artifacts will be uploaded to: {}'.format(output_location))
  
  create_training_params = \
  {
      "AlgorithmSpecification": {
          "TrainingImage": image,
          "TrainingInputMode": "File"
      },
      "RoleArn": role,
      "OutputDataConfig": {
          "S3OutputPath": output_location
      },
      "ResourceConfig": {
          "InstanceCount": 2,
          "InstanceType": "ml.c4.8xlarge",
          "VolumeSizeInGB": 50
      },
      "TrainingJobName": job_name,
      "HyperParameters": {
          "k": "10",
          "feature_dim": "784",
          "mini_batch_size": "500"
      },
      "StoppingCondition": {
          "MaxRuntimeInSeconds": 60 * 60
      },
      "InputDataConfig": [
          {
              "ChannelName": "train",
              "DataSource": {
                  "S3DataSource": {
                      "S3DataType": "S3Prefix",
                      "S3Uri": data_location,
                      "S3DataDistributionType": "FullyReplicated"
                  }
              },
              "CompressionType": "None",
              "RecordWrapperType": "None"
          }
      ]
  }
  
  
  sagemaker = boto3.client('sagemaker')
  
  sagemaker.create_training_job(**create_training_params)
  
  status = sagemaker.describe_training_job(TrainingJobName=job_name)['TrainingJobStatus']
  print(status)
  
  try:
      sagemaker.get_waiter('training_job_completed_or_stopped').wait(TrainingJobName=job_name)
  finally:
      status = sagemaker.describe_training_job(TrainingJobName=job_name)['TrainingJobStatus']
      print("Training job ended with status: " + status)
      if status == 'Failed':
          message = sagemaker.describe_training_job(TrainingJobName=job_name)['FailureReason']
          print('Training failed with the following error: {}'.format(message))
          raise Exception('Training job failed')
  ```

  The code uses a `Waiter` to wait until training is complete before returning\. 

You now have trained a model\. The resulting artifacts are stored in your S3 bucket\. 

**Next Step**  
[Step 3\.4: Deploy the Model to Amazon SageMaker Hosting Services ](ex1-deploy-model.md)