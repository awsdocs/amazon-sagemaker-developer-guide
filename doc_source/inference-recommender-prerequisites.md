# Prerequisites<a name="inference-recommender-prerequisites"></a>

To use Amazon SageMaker Inference Recommender, first make sure you have met the prerequisites listed below\. As an example, we show how to use a PyTorch \(v1\.7\.1\) resnet\-18 pre\-trained model for both types of Inference Recommender jobs\. 

**Note**  
The following code examples use Python\. Remove the `!` prefix character if you run any of the following code samples in your terminal or AWS CLI\. We are using the `conda_pytorch_p36_latest` kernel in SageMaker Notebook Instances. This is running Python 3.6 and PyTorch 1.7.1.

1. **Create an IAM role for Amazon SageMaker\.**

   Create an IAM Role for Amazon SageMaker that has the `AmazonSageMakerFullAccess` IAM managed policy attached\.

1. **\(Optional\) Review existing models benchmarked by Inference Recommender\.**

   Inference Recommender benchmarks models from popular model zoos\. Inference Recommender supports your model even if it is not already benchmarked\.

   Use `ListModelMetaData` to get a response object that lists the domain, framework, task, and model name of machine learning models found in common model zoos\.

   You use the domain, framework, framework version, task, and model name in later steps to both select an inference Docker image and register your model with SageMaker Model Registry\. The following demonstrates how to list model metadata with SDK for Python \(Boto3\): 

   ```
   import boto3
   aws_region="<INSERT>"
   sagemaker_client = boto3.client("sagemaker", aws_region) 
   
   list_model_metadata_response=sagemaker_client.list_model_metadata()
   ```

   The output includes model summaries \(`ModelMetadataSummaries`\) and response metadata \(`ResponseMetadata`\) similar to the following:

   ```
   {
       'ModelMetadataSummaries': [{
               'Domain': 'NATURAL_LANGUAGE_PROCESSING',
               'Framework': 'PYTORCH:1.6.0',
                'Model': 'bert-base-cased',
                'Task': 'FILL_MASK'
                },
               {
                'Domain': 'NATURAL_LANGUAGE_PROCESSING',
                'Framework': 'PYTORCH:1.6.0',
                'Model': 'bert-base-uncased',
                'Task': 'FILL_MASK'
                },
               {
               'Domain': 'COMPUTER_VISION',
                'Framework': 'MXNET:1.8.0',
                'Model': 'resnet18v2-gluon',
                'Task': 'IMAGE_CLASSIFICATION'
                },
                {
                'Domain': 'COMPUTER_VISION',
                'Framework': 'PYTORCH:1.6.0',
                'Model': 'resnet152',
                'Task': 'IMAGE_CLASSIFICATION'
                }],
       'ResponseMetadata': {
                               'HTTPHeaders': {
                               'content-length': '2345',
                               'content-type': 'application/x-amz-json-1.1',
                               'date': 'Tue, 19 Oct 2021 20:52:03 GMT',
                               'x-amzn-requestid': 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'
                             },
       'HTTPStatusCode': 200,
       'RequestId': 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx',
       'RetryAttempts': 0
       }
   }
   ```

   For this demo, we use a PyTorch \(v1\.7\.1\) resnet\-18 model to perform image classification\. The following Python code sample stores the framework, framework version, domain, and task into variables for later use:

   ```
   # ML framework details
   framework = 'PYTORCH'
   framework_version = '1.7.1'
   
   # ML model details
   ml_domain = 'COMPUTER_VISION'
   ml_task = 'IMAGE_CLASSIFICATION'
   ```

1. **Upload your machine learning model to Amazon S3\.**

   Use this PyTorch \(v1\.7\.1\) resnet\-18 model if you do not have a pre\-trained machine learning model:

   ```
   # Optional: Download a sample PyTorch model
   import torch
   from torchvision import models, transforms, datasets
   # Create an example input for tracing
   image = torch.zeros([1, 3, 256, 256], dtype=torch.float32)
   # Load a pretrained resnet18 model from TorchHub
   model = models.resnet18(pretrained=True)
   # Tell the model we are using it for evaluation (not training). Note this is required for Inferentia compilation.
   model.eval()
   model_trace = torch.jit.trace(model, image)
   # Save your traced model
   model_trace.save('model.pth')
   # Download the inference script
   !wget https://aws-ml-blog-artifacts.s3.us-east-2.amazonaws.com/inference.py
   # move it into a code/ directory
   !mkdir code && mv inference.py code/
   ```

   SageMaker requires pre\-trained machine learning models be packaged as a compressed file \(`*.tar.gz`\)\. Compress your model to satisfy this requirement:

   ```
   !tar -czf test.tar.gz model.pth resnet18.py
   ```

   When your endpoint is provisioned, the files in the archive are extracted to `/opt/ml/model/` on the endpoint\.

   After you compress your model and model artifacts as a \.tar.gz file, upload them to your Amazon S3 bucket\. The following demonstrates how to upload your model to Amazon S3 using the AWS CLI:

   ```
   !aws s3 cp test.tar.gz s3://{your-bucket}/models/
   ```

1. **Select a prebuilt Docker inference image or create your own Inference Docker Image\.**

   SageMaker provides containers for its built\-in algorithms and prebuilt Docker images for some of the most common machine learning frameworks, such as Apache MXNet, TensorFlow, PyTorch, and Chainer\. For a full list of the available SageMaker images, see [Available Deep Learning Containers Images](https://github.com/aws/deep-learning-containers/blob/master/available_images.md)\.

   If none of the existing SageMaker containers meet your needs and you don't have an existing container of your own, create a new Docker image\. See [Use Your Own Inference Code](your-algorithms-inference-main.md) for information about how to create your Docker image\.

   The following demonstrates how to retrieve a PyTorch version 1\.7\.1 inference image using the SageMaker Python SDK:

   ```
   from sagemaker import image_uris
   
   aws_region="<aws_region>"
   
   ## Uncomment and replace with your own values if you did not define  
   ## these variables a previous step.
   #framework = 'PYTORCH'
   #framework_version = '1.7.1'
   
   # Note: you can use any CPU-based instance here, 
   # this is just to set the arch as CPU for the Docker image
   instance_type = 'ml.m5.2xlarge' 
   
   image_uri = image_uris.retrieve(framework, 
                                   region=aws_region, 
                                   version=framework_version, 
                                   py_version='py3', 
                                   instance_type=instance_type, 
                                   image_scope='inference')
   ```

   For a list of available SageMaker Instances, see [Amazon SageMaker Pricing](https://aws.amazon.com/sagemaker/pricing/)\.

1. **Create a sample payload archive\.**

   Create an archive that contains individual files that the load testing tool can send to your SageMaker endpoints\. Your inference code must be able to read the file formats from the sample payload\.

   The following downloads a \.jpg image that this example uses in a later step for the resnet18 model\.

   ```
   !wget https://cdn.pixabay.com/photo/2020/12/18/05/56/flowers-5841251_1280.jpg
   ```

   Compress the sample payload as a tarball:

   ```
   !tar -cvzf payload.tar.gz flowers-5841251_1280.jpg
   ```

   Upload the sample payload to Amazon S3 and note the Amazon S3 URI:

   ```
   !aws s3 cp payload.tar.gz s3://{bucket}/models/
   ```

   You need the Amazon S3 URI in a later step, so store it in a variable:

   ```
   bucket_prefix='models'
   bucket = '<your-bucket-name>' # Provide the name of your S3 bucket
   payload_s3_key = f"{bucket_prefix}/payload.tar.gz"
   sample_payload_url= f"s3://{bucket}/{payload_s3_key}"
   ```

1. **Register your model in the model registry**

   With SageMaker Model Registry, you can catalog models for production, manage model versions, associate metadata \(such as training metrics\) with a model, manage the approval status of a model, deploy models to production, and automate model deployment with CI/CD\.

   When you use Model Registry to track and manage your models, they are represented as a versioned model package within model package groups\. Unversioned model packages are not part a model group\. Model package groups hold multiple versions or iterations of a model\. Though it is not required to create them for every model in the registry, they help organize various models that all have the same purpose and provide automatic versioning\.

   To use Amazon SageMaker Inference Recommender, you must have a versioned model package\. You can create a versioned model package programmatically with the AWS SDK for Python \(Boto3\) or with Amazon SageMaker Studio\. To create a versioned model package programmatically, first create a model package group with the `CreateModelPackageGroup` API\. Next, create a model package using the `CreateModelPackage` API\. Calling this method makes a versioned model package\.

   See [Create a Model Group](model-registry-model-group.md) and [Register a Model Version](model-registry-version.md) for detailed instructions about how to programmatically and interactively create a model package group and how to create a versioned model package, respectively, with the AWS SDK for Python \(Boto3\) and Amazon SageMaker Studio\.

   The following code sample demonstrates how to create a versioned model package using the AWS SDK for Python \(Boto3\)\.
**Note**  
Note: You do not need to approve the model package to create an Inference Recommender job\.

   1. **Create a model package group**

      Create a model package group with the `CreateModelPackageGroup` API\. Provide a name to the model package group for the `ModelPackageGroupName` and optionally provide a description of the model package in the `ModelPackageGroupDescription` field\.

      ```
      model_package_group_name = '<INSERT>'
      model_package_group_description = '<INSERT>' 
      
      model_package_group_input_dict = {
       "ModelPackageGroupName" : model_package_group_name,
       "ModelPackageGroupDescription" : model_package_group_description,
      }
      
      model_package_group_response = sagemaker_client.create_model_package_group(**model_package_group_input_dict)
      ```

      See the [Amazon SageMaker API Reference Guide](https://docs.aws.amazon.com/sagemaker/latest/APIReference/Welcome.html) for a full list of optional and required arguments you can pass to [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModelPackageGroup.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModelPackageGroup.html)\.

      Create a Model Package by specifying a Docker image that runs your inference code and the Amazon S3 location of your model artifacts and provide values for `InferenceSpecification`\. `InferenceSpecification` should contain information about inference jobs that can be run with models based on this Model Package, including the following:
      + The Amazon ECR paths of images that run your inference code\.
      + The instance types that the Model Package supports for transform jobs and real\-time endpoints used for inference\. Note that this is optional.
      + The input and output content formats that the Model Package supports for inference\. 

      In a previous step we downloaded a pre\-trained resnet18 model and stored it in an Amazon S3 bucket in a directory called `models`\. We retrieved a PyTorch \(v1\.7\.1\) Deep Learning Container inference image and stored the URI in a variable called `image_uri`\. We use those variables in the following code sample where we define a dictionary used as input to the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModelPackage.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModelPackage.html) API\.

      ```
      # Provide the Amazon S3 URI of your compressed tarfile
      # so that Model Registry knows where to find your model artifacts
      bucket_prefix='models'
      bucket = '<your-bucket-name>' # Provide the name of your S3 bucket
      model_s3_key = f"{bucket_prefix}/test.tar.gz"
      model_url= f"s3://{bucket}/{model_s3_key}"
      
      # Similar open source model to the packaged model
      # The name of the ML model as standardized by common model zoos
      nearest_model_name = 'resnet18'
      
      # The supported MIME type for input data. 
      input_content_type='image/jpeg'      
      
      # Optional - provide a description of your model.
      model_package_description = '<INSERT>'
      
      ## Uncomment if you did not store the domain and task in an earlier
      ## step 
      #ml_domain = 'COMPUTER_VISION'
      #ml_task = 'IMAGE_CLASSIFICATION'
      
      ## Uncomment if you did not store the framework and framework version
      ## in a previous step.
      #framework = 'PYTORCH'
      #framework_version = '1.7.1'
      
      # Optional: Used for optimizing your model using SageMaker Neo
      # PyTorch uses NCHW format for images
      data_input_configuration = "[[1,3,256,256]]"
      
      # Create a dictionary to use as input for creating a model pacakge group
      model_package_input_dict = {
              "ModelPackageGroupName" : model_package_group_name,
              "ModelPackageDescription" : model_package_description,
              "Domain": ml_domain,
              "Task": ml_task,
              "SamplePayloadUrl": sample_payload_url,
              "InferenceSpecification": {
                      "Containers": [
                          {
                              "Image": image_uri,
                              "ModelDataUrl": model_url,
                              "Framework": framework.upper(), 
                              "FrameworkVersion": framework_version,
                              "NearestModelName": nearest_model_name,
                              "ModelInput": {"DataInputConfig": data_input_configuration}
                          }
                          ],
                      "SupportedContentTypes": [input_content_type]              
                      }
          }
      ```

   1. **Create a Model Package**

      Use the `CreateModelPackage` API to create a Model Package\. Pass the input dictionary defined in the previous step:

      ```
      model_package_response = sagemaker_client.create_model_package(**model_package_input_dict)
      ```

      You need the model package ARN to use Amazon SageMaker Inference Recommender\. Note the ARN of the model package or store it in a variable:

      ```
      model_package_arn = model_package_response["ModelPackageArn"]
      
      print('ModelPackage Version ARN : {}'.format(model_package_arn))
      ```
