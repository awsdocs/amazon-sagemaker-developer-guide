# Step 3\.4: Deploy the Model to Amazon SageMaker Hosting Services<a name="ex1-deploy-model"></a>

Deploying a model in Amazon SageMaker is a 3\-step process: 

1. Create a model in Amazon SageMaker— Send a [CreateModel](API_CreateModel.md) request to provide information such as the location of the S3 bucket that contains your model artifacts and the registry path of the image that contains inference code\. In the next step, you provide the model when you create an endpoint configuration\.

1. Create an endpoint configuration— Send a [CreateEndpointConfig](API_CreateEndpointConfig.md) request to provide the resource configuration for hosting\. This includes the type and number of ML compute instances to launch for deploying the model\. In the next step, you create an endpoint with the [CreateEndpoint](API_CreateEndpoint.md) API using this endpoint configuration\.

1. Create an endpoint— Send a [CreateEndpoint](API_CreateEndpoint.md) request to create an endpoint\. Amazon SageMaker launches the ML compute instances and deploys the model\. In the response, Amazon SageMaker returns an endpoint\. Applications can send requests to this endpoint to get inferences from the model\.

The low\-level AWS SDK for Python provides corresponding methods\. However, the high\-level Python library provides the `deploy` method that does all these tasks for you\. 

To deploy the model, choose one of the following options\. 

+ **Use the high\-level Python library provided by Amazon SageMaker**\.

  The `sagemaker.amazon.kmeans.KMeans` class provides the `deploy` method for deploying a model\. It performs all three steps of the model deployment process\. 

  ```
  %%time
  
  kmeans_predictor = kmeans.deploy(initial_instance_count=1,
                                   instance_type='ml.c4.xlarge')
  ```

  The `sagemaker.amazon.kmeans.KMeans` instance knows the registry path of the image that contains the k\-means inference code, so you don't need to provide it\. 

  This is a synchronous operation\. The method waits until the deployment completes before returning\. It returns a `kmeans_predictor`\. 

+ **Use the SDK for Python**\. 

  The low\-level SDK for Python provides methods that map to the underlying Amazon SageMaker API\. To deploy the model, you make three calls\.

  1. Create an Amazon SageMaker model by identifying the location of model artifacts and the Docker image that contains inference code\. 

     ```
     %%time
     import boto3
     from time import gmtime, strftime
     
     
     model_name=job_name
     print(model_name)
     
     info = sagemaker.describe_training_job(TrainingJobName=job_name)
     model_data = info['ModelArtifacts']['S3ModelArtifacts']
     
     primary_container = {
         'Image': image,
         'ModelDataUrl': model_data
     }
     
     create_model_response = sagemaker.create_model(
         ModelName = model_name,
         ExecutionRoleArn = role,
         PrimaryContainer = primary_container)
     
     print(create_model_response['ModelArn'])
     ```

  1. Create an Amazon SageMaker endpoint configuration by specifying the ML compute instances that you want to deploy your model to\.

     ```
     from time import gmtime, strftime
     
     endpoint_config_name = 'KMeansEndpointConfig-' + strftime("%Y-%m-%d-%H-%M-%S", gmtime())
     print(endpoint_config_name)
     create_endpoint_config_response = sagemaker.create_endpoint_config(
         EndpointConfigName = endpoint_config_name,
         ProductionVariants=[{
             'InstanceType':'ml.c4.xlarge',
             'InitialInstanceCount':3,
             'ModelName':model_name,
             'VariantName':'AllTraffic'}])
     
     print("Endpoint Config Arn: " + create_endpoint_config_response['EndpointConfigArn'])
     ```

  1. Create an Amazon SageMaker endpoint\. This code uses a `Waiter` to wait until the deployment is complete before returning\.

     ```
     %%time
     import time
     
     endpoint_name = 'KMeansEndpoint-' + strftime("%Y-%m-%d-%H-%M-%S", gmtime())
     print(endpoint_name)
     create_endpoint_response = sagemaker.create_endpoint(
         EndpointName=endpoint_name,
         EndpointConfigName=endpoint_config_name)
     print(create_endpoint_response['EndpointArn'])
     
     resp = sagemaker.describe_endpoint(EndpointName=endpoint_name)
     status = resp['EndpointStatus']
     print("Status: " + status)
     
     try:
         sagemaker.get_waiter('Endpoint_Created').wait(EndpointName=endpoint_name)
     finally:
         resp = sagemaker.describe_endpoint(EndpointName=endpoint_name)
         status = resp['EndpointStatus']
         print("Arn: " + resp['EndpointArn'])
         print("Create endpoint ended with status: " + status)
     
         if status != 'InService':
             message = sagemaker.describe_endpoint(EndpointName=endpoint_name)['FailureReason']
             print('Training failed with the following error: {}'.format(message))
             raise Exception('Endpoint creation did not succeed')
     ```

**Next Step**  
[Step 3\.5: Validate the Model](ex1-test-model.md)