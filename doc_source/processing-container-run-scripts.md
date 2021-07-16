# Run Scripts with Your Own Processing Container<a name="processing-container-run-scripts"></a>

You can use scikit\-learn scripts to preprocess data and evaluate your models\. To see how to run scikit\-learn scripts to perform these tasks, see the [scikit\-learn Processing](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker_processing/scikit_learn_data_processing_and_model_evaluation) sample notebook\. This notebook uses the `ScriptProcessor` class from the Amazon SageMaker Python SDK for Processing\.

The following example shows a general workflow for using a `ScriptProcessor` class with your own processing container\. The workflow shows how to create your own image, build your container, and use a `ScriptProcessor` class to run a Python preprocessing script with the container\. The processing job processes your input data and saves the processed data in Amazon Simple Storage Service \(Amazon S3\)\.

Before using the following examples, you need to have your own input data and a Python script prepared to process your data\. For an end\-to\-end, guided example of this process, refer back to the [scikit\-learn Processing](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker_processing/scikit_learn_data_processing_and_model_evaluation) sample notebook\.

1. Create a Docker directory and add the Dockerfile used to create the processing container\. Install pandas and scikit\-learn into it\. \(You could also install your own dependencies with a similar `RUN` command\.\)

   ```
   mkdir docker
   
   %%writefile docker/Dockerfile
   
   FROM python:3.7-slim-buster
   
   RUN pip3 install pandas==0.25.3 scikit-learn==0.21.3
   ENV PYTHONUNBUFFERED=TRUE
   
   ENTRYPOINT ["python3"]
   ```

1. Build the container using the docker command, create an Amazon Elastic Container Registry \(Amazon ECR\) repository, and push the image to Amazon ECR\.

   ```
   import boto3
   
   account_id = boto3.client('sts').get_caller_identity().get('Account')
   region = boto3.Session().region_name
   ecr_repository = 'sagemaker-processing-container'
   tag = ':latest'
   processing_repository_uri = '{}.dkr.ecr.{}.amazonaws.com/{}'.format(account_id, region, ecr_repository + tag)
   
   # Create ECR repository and push docker image
   !docker build -t $ecr_repository docker
   !aws ecr get-login-password --region {region} | docker login --username AWS --password-stdin {account_id}.dkr.ecr.{region}.amazonaws.com
   !aws ecr create-repository --repository-name $ecr_repository
   !docker tag {ecr_repository + tag} $processing_repository_uri
   !docker push $processing_repository_uri
   ```

1. Set up the `ScriptProcessor` from the SageMaker Python SDK to run the script\. Replace *image\_uri* with the URI for the image you created, and replace *role\_arn* with the ARN for an AWS Identity and Access Management role that has access to your target Amazon S3 bucket\.

   ```
   from sagemaker.processing import ScriptProcessor, ProcessingInput, ProcessingOutput
   
   script_processor = ScriptProcessor(command=['python3'],
                   image_uri='image_uri',
                   role='role_arn',
                   instance_count=1,
                   instance_type='ml.m5.xlarge')
   ```

1. Run the script\. Replace *preprocessing\.py* with the name of your own Python processing script, and replace *s3://path/to/my/input\-data\.csv* with the Amazon S3 path to your input data\.

   ```
   script_processor.run(code='preprocessing.py',
                        inputs=[ProcessingInput(
                           source='s3://path/to/my/input-data.csv',
                           destination='/opt/ml/processing/input')],
                        outputs=[ProcessingOutput(source='/opt/ml/processing/output/train'),
                                  ProcessingOutput(source='/opt/ml/processing/output/validation'),
                                  ProcessingOutput(source='/opt/ml/processing/output/test')])
   ```

You can use the same procedure with any other library or system dependencies\. You can also use existing Docker images\. This includes images that you run on other platforms such as [Kubernetes](https://kubernetes.io/)\.