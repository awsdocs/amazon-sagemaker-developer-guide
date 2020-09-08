# Step 6\.1: Deploy the Model to SageMaker Hosting Services<a name="ex1-deploy-model"></a>

To deploy a model in SageMaker, hosting services, you can use either the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) or the AWS SDK for Python \(Boto3\)\. This exercise provides code examples for both libraries\. 

The [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) abstracts several implementation details, and is easy to use\. If you're a first\-time SageMaker user, we recommend that you use it\. For more information, see [https://sagemaker\.readthedocs\.io/en/stable/overview\.html](https://sagemaker.readthedocs.io/en/stable/overview.html)\.

**Topics**
+ [Deploy the Model to SageMaker Hosting Services \([Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\)](#ex1-deploy-model-sdk)
+ [Deploy the Model to SageMaker Hosting Services \(AWS SDK for Python \(Boto3\)\)\.](#ex1-deploy-model-boto)

## Deploy the Model to SageMaker Hosting Services \([Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\)<a name="ex1-deploy-model-sdk"></a>

Deploy the model that you trained in [Create and Run a Training Job \([Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\)](ex1-train-model.md#ex1-train-model-sdk) by calling the `deploy` method of the `sagemaker.estimator.Estimator` object\. This is the same object that you used to train the model\. When you call the `deploy` method, specify the number and type of ML instances that you want to use to host the endpoint\.

```
xgb_predictor = xgb_model.deploy(initial_instance_count=1,
                                content_type='text/csv',
                                instance_type='ml.t2.medium'
                                )
```

The `deploy` method creates the deployable model, configures the SageMaker hosting services endpoint, and launches the endpoint to host the model\. For more information, see [https://sagemaker\.readthedocs\.io/en/stable/estimators\.html\#sagemaker\.estimator\.Estimator\.deploy](https://sagemaker.readthedocs.io/en/stable/estimators.html#sagemaker.estimator.Estimator.deploy)\.

It also returns a `sagemaker.predictor.RealTimePredictor` object, which you can use to get inferences from the model\. For information, see [https://sagemaker\.readthedocs\.io/en/stable/predictors\.html\#sagemaker\.predictor\.RealTimePredictor](https://sagemaker.readthedocs.io/en/stable/predictors.html#sagemaker.predictor.RealTimePredictor)\.

**Next Step**  
[Step 7: Validate the Model](ex1-test-model.md)

## Deploy the Model to SageMaker Hosting Services \(AWS SDK for Python \(Boto3\)\)\.<a name="ex1-deploy-model-boto"></a>

Deploying a model using the AWS SDK for Python \(Boto 3\) is a three\-step process: 

1. Create a model in SageMaker – Send a [ `CreateModel`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html) request to provide information such as the location of the S3 bucket that contains your model artifacts and the registry path of the image that contains inference code\.

1. Create an endpoint configuration – Send a [ `CreateEndpointConfig`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpointConfig.html) request to provide the resource configuration for hosting\. This includes the type and number of ML compute instances to launch to deploy the model\. 

1. Create an endpoint – Send a [ `CreateEndpoint`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html) request to create an endpoint\. SageMaker launches the ML compute instances and deploys the model\. SageMaker returns an endpoint\. Applications can send requests for inference to this endpoint\.

**To deploy the model \(AWS SDK for Python \(Boto 3\)\)**

For each of the following steps, paste the code in a cell in the Jupyter notebook you created in [Step 3: Create a Jupyter Notebook](ex1-prepare.md) and run the cell\.

1. Create a deployable model by identifying the location of model artifacts and the Docker image that contains the inference code\. 

   ```
   model_name = training_job_name + '-mod'
   
   info = sm.describe_training_job(TrainingJobName=training_job_name)
   model_data = info['ModelArtifacts']['S3ModelArtifacts']
   print(model_data)
   
   primary_container = {
       'Image': container,
       'ModelDataUrl': model_data
   }
   
   create_model_response = sm.create_model(
       ModelName = model_name,
       ExecutionRoleArn = role,
       PrimaryContainer = primary_container)
   
   print(create_model_response['ModelArn'])
   ```

1. Create a SageMaker endpoint configuration by specifying the ML compute instances that you want to deploy your model to\.

   ```
   endpoint_config_name = 'DEMO-XGBoostEndpointConfig-' + strftime("%Y-%m-%d-%H-%M-%S", gmtime())
   print(endpoint_config_name)
   create_endpoint_config_response = sm.create_endpoint_config(
       EndpointConfigName = endpoint_config_name,
       ProductionVariants=[{
           'InstanceType':'ml.m4.xlarge',
           'InitialVariantWeight':1,
           'InitialInstanceCount':1,
           'ModelName':model_name,
           'VariantName':'AllTraffic'}])
   
   print("Endpoint Config Arn: " + create_endpoint_config_response['EndpointConfigArn'])
   ```

1. Create a SageMaker endpoint\. 

   ```
   %%time
   import time
   
   endpoint_name = 'DEMO-XGBoostEndpoint-' + strftime("%Y-%m-%d-%H-%M-%S", gmtime())
   print(endpoint_name)
   create_endpoint_response = sm.create_endpoint(
       EndpointName=endpoint_name,
       EndpointConfigName=endpoint_config_name)
   print(create_endpoint_response['EndpointArn'])
   
   resp = sm.describe_endpoint(EndpointName=endpoint_name)
   status = resp['EndpointStatus']
   print("Status: " + status)
   
   while status=='Creating':
       time.sleep(60)
       resp = sm.describe_endpoint(EndpointName=endpoint_name)
       status = resp['EndpointStatus']
       print("Status: " + status)
   
   print("Arn: " + resp['EndpointArn'])
   print("Status: " + status)
   ```

   This code continuously calls the `describe_endpoint` command in a `while` loop until the endpoint either fails or is in service, and then prints the status of the endpoint\. When the status changes to `InService`, the endpoint is ready to serve inference requests\.

**Next Step**  
[Step 7: Validate the Model](ex1-test-model.md)