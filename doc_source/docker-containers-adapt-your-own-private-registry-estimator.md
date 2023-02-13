# Use a SageMaker estimator to run a training job<a name="docker-containers-adapt-your-own-private-registry-estimator"></a>

You can also use an [estimator](https://sagemaker.readthedocs.io/en/stable/api/training/estimators.html) from the SageMaker Python SDK to handle the configuration and running of your SageMaker training job\. The following code examples show how to configure and run an estimator using images from a private Docker registry\.

1. Import the required libraries and dependencies, as shown in the following code example\.

   ```
   import boto3
   import sagemaker
   from sagemaker.estimator import Estimator
   
   session = sagemaker.Session()
   
   role = sagemaker.get_execution_role()
   ```

1. Provide a Uniform Resource Identifier \(URI\) to your training image, security groups and subnets for the VPC configuration for your training job, as shown in the following code example\.

   ```
   image_uri = "myteam.myorg.com/docker-local/my-training-image:<IMAGE-TAG>"
   
   security_groups = ["sg-0123456789abcdef0"]
   subnets = ["subnet-0123456789abcdef0", "subnet-0123456789abcdef0"]
   ```

   For more information about `security_group_ids` and `subnets`, see the appropriate parameter description in the [Estimators](https://sagemaker.readthedocs.io/en/stable/api/training/estimators.html) section of the SageMaker Python SDK\.
**Note**  
SageMaker uses a network connection within your VPC to access images in your Docker registry\. To use the images in your Docker registry for training, the registry must be accessible from an Amazon VPC in your account\.

1. Optionally, if your Docker registry requires authentication, you must also specify the Amazon Resource Name \(ARN\) of an AWS Lambda function that provides access credentials to SageMaker\. The following code example shows how to specify the ARN\. 

   ```
   training_repository_credentials_provider_arn = "arn:aws:lambda:us-west-2:1234567890:function:test"
   ```

   For more information about using images in a Docker registry requiring authentication, see **Use a Docker registry that requires authentication for training** below\.

1. Use the code examples from the previous steps to configure an estimator, as shown in the following code example\.

   ```
   # The training repository access mode must be 'Vpc' for private docker registry jobs 
   training_repository_access_mode = "Vpc"
   
   # Specify the instance type, instance count you want to use
   instance_type="ml.m5.xlarge"
   instance_count=1
   
   # Specify the maximum number of seconds that a model training job can run
   max_run_time = 1800
   
   # Specify the output path for the model artifacts
   output_path = "s3://your-output-bucket/your-output-path"
   
   estimator = Estimator(
       image_uri=image_uri,
       role=role,
       subnets=subnets,
       security_group_ids=security_groups,
       training_repository_access_mode=training_repository_access_mode,
       training_repository_credentials_provider_arn=training_repository_credentials_provider_arn,  # remove this line if auth is not needed
       instance_type=instance_type,
       instance_count=instance_count,
       output_path=output_path,
       max_run=max_run_time
   )
   ```

1. Start your training job by calling `estimator.fit` with your job name and input path as parameters, as shown in the following code example\.

   ```
   input_path = "s3://your-input-bucket/your-input-path"
   job_name = "your-job-name"
   
   estimator.fit(
       inputs=input_path,
       job_name=job_name
   )
   ```