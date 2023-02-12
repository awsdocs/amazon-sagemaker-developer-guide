# Adapt your training job to access images in a private Docker registry<a name="docker-containers-adapt-your-own-private-registry"></a>

You can use a private [Docker registry](https://docs.docker.com/registry/) instead of an Amazon Elastic Container Registry \(Amazon ECR\) to host your images for SageMaker Training\. The following instructions show you how to create a Docker registry, configure your virtual private cloud \(VPC\) and training job, store images, and give SageMaker access to the training image in the private docker registry\. These instructions also show you how to use a Docker registry that requires authentication for a SageMaker training job\.

## Create and store your images in a private Docker registry<a name="docker-containers-adapt-your-own-private-registry-prerequisites"></a>

Create a private Docker registry to store your images\. Your registry must:
+ use the [Docker Registry HTTP API](https://docs.docker.com/registry/spec/api/) protocol
+ be accessible from the same VPC specified in the [VpcConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html#API_CreateTrainingJob_RequestSyntax) parameter in the `CreateTrainingJob` API\. Input `VpcConfig` when you create your training job\.
+ secured with a [TLS certificate](http://aws.amazon.com/what-is/ssl-certificate/) from a known public certificate authority\.

For more information about creating a Docker registry, see [Deploy a registry server](https://docs.docker.com/registry/deploying/)\.

## Configure your VPC and SageMaker training job<a name="docker-containers-adapt-your-own-private-registry-configure"></a>

SageMaker uses a network connection within your VPC to access images in your Docker registry\. To use the images in your Docker registry for training, the registry must be accessible from an Amazon VPC in your account\. For more information, see [Use a Docker registry that requires authentication for training](docker-containers-adapt-your-own-private-registry-authentication.md)\.

You must also configure your training job to connect to the same VPC to which your Docker registry has access\. For more information, see [Configure a Training Job for Amazon VPC Access](https://docs.aws.amazon.com/sagemaker/latest/dg/train-vpc.html#train-vpc-configure)\.

## Create a training job using an image from your private Docker registry<a name="docker-containers-adapt-your-own-private-registry-create"></a>

To use an image from your private Docker registry for training, use the following guide to configure your image, configure and create a training job\. The code examples that follow use the AWS SDK for Python \(Boto3\) client\.

1. Create a training image configuration object and input `Vpc` the `TrainingRepositoryAccessMode` field as follows\.

   ```
   training_image_config = {
       'TrainingRepositoryAccessMode': 'Vpc'
   }
   ```
**Note**  
If your private Docker registry requires authentication, you must add a `TrainingRepositoryAuthConfig` object to the training image configuration object\. You must also specify the Amazon Resource Name \(ARN\) of an AWS Lambda function that provides access credentials to SageMaker using the `TrainingRepositoryCredentialsProviderArn` field of the `TrainingRepositoryAuthConfig` object\. For more information, see the example code structure below\.  

   ```
   training_image_config = {
      'TrainingRepositoryAccessMode': 'Vpc',
      'TrainingRepositoryAuthConfig': {
           'TrainingRepositoryCredentialsProviderArn': 'arn:aws:lambda:Region:Acct:function:FunctionName'
      }
   }
   ```

   For information about how to create the Lambda function to provide authentication, see [Use a Docker registry that requires authentication for training](docker-containers-adapt-your-own-private-registry-authentication.md)\.

1. Use a Boto3 client to create a training job and pass the correct configuration to the [create\_training\_job](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) API\. The following instructions show you how to configure the components and create a training job\.

   1. Create the `AlgorithmSpecification` object that you want to pass to `create_training_job`\. Use the training image configuration object that you created in the previous step, as shown in the following code example\.

      ```
      algorithm_specification = {
         'TrainingImage': 'myteam.myorg.com/docker-local/my-training-image:<IMAGE-TAG>',
         'TrainingImageConfig': training_image_config,
         'TrainingInputMode': 'File'
      }
      ```
**Note**  
To use a fixed, rather than an updated version of an image, refer to the image’s [digest](https://docs.docker.com/engine/reference/commandline/pull/#pull-an-image-by-digest-immutable-identifier) instead of by name or tag\.

   1. Specify the name of the training job and role that you want to pass to `create_training_job`, as shown in the following code example\. 

      ```
      training_job_name = 'private-registry-job'
      execution_role_arn = 'arn:aws:iam::123456789012:role/SageMakerExecutionRole'
      ```

   1. Specify a security group and subnet for the VPC configuration for your training job\. Your private Docker registry must allow inbound traffic from the security groups that you specify, as shown in the following code example\.

      ```
      vpc_config = {
          'SecurityGroupIds': ['sg-0123456789abcdef0'],
          'Subnets': ['subnet-0123456789abcdef0','subnet-0123456789abcdef1']
      }
      ```
**Note**  
If your subnet is not in the same VPC as your private Docker registry, you must set up a networking connection between the two VPCs\. SeeConnect VPCs using [VPC peering](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-peering.html) for more information\.

   1. Specify the resource configuration, including machine learning compute instances and storage volumes to use for training, as shown in the following code example\. 

      ```
      resource_config = {
          'InstanceType': 'ml.m4.xlarge',
          'InstanceCount': 1,
          'VolumeSizeInGB': 10,
      }
      ```

   1. Specify the input and output data configuration, where the training dataset is stored, and where you want to store model artifacts, as shown in the following code example\.

      ```
      input_data_config = [
          {
              "ChannelName": "training",
              "DataSource":
              {
                  "S3DataSource":
                  {
                      "S3DataDistributionType": "FullyReplicated",
                      "S3DataType": "S3Prefix",
                      "S3Uri": "s3://your-training-data-bucket/training-data-folder"
                  }
              }
          }
      ]
      
      output_data_config = {
          'S3OutputPath': 's3://your-output-data-bucket/model-folder'
      }
      ```

   1. Specify the maximum number of seconds that a model training job can run as shown in the following code example\.

      ```
      stopping_condition = {
          'MaxRuntimeInSeconds': 1800
      }
      ```

   1. Finally, create the training job using the parameters you specified in the previous steps as shown in the following code example\.

      ```
      import boto3
      sm = boto3.client('sagemaker')
      try:
          resp = sm.create_training_job(
              TrainingJobName=training_job_name,
              AlgorithmSpecification=algorithm_specification,
              RoleArn=execution_role_arn,
              InputDataConfig=input_data_config,
              OutputDataConfig=output_data_config,
              ResourceConfig=resource_config,
              VpcConfig=vpc_config,
              StoppingCondition=stopping_condition
          )
      except Exception as e:
          print(f'error calling CreateTrainingJob operation: {e}')
      else:
          print(resp)
      ```