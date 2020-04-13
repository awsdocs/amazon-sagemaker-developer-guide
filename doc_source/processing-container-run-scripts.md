# Run Scripts with Your Own Processing Container<a name="processing-container-run-scripts"></a>

For a sample notebook that shows how to run scikit\-learn scripts to do data preprocessing and model evaluation with the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) for Processing, see the [scikit\-learn Processing](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker_processing/scikit_learn_data_processing_and_model_evaluation) sample\. 

The following example shows how to use a `ScriptProcessor` class from the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) to run a Python script with your own image to run a processing job that processes input data, and saves the processed data in Amazon S3\.

The notebook shows the general workflow as follows\. It includes the code from the steps that use the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\.

1. Write the Dockerfile with the dependencies, build it, and push it to an Amazon Elastic Container Registry \(Amazon ECR\) repository\.

1. Write the `preprocessing.py` that contains the preprocessing code\.

1. Set up the ScriptProcessor to run the script\.

   ```
   from sagemaker.processing import ScriptProcessor, ProcessingInput, ProcessingOutput
   
   script_processor = ScriptProcessor(command=['python3'],
                   image_uri='<image_uri>',
                   role='<role_arn>',
                   instance_count=1,
                   instance_type='ml.m5.xlarge')
   ```

1. Run the script\.

   ```
   script_processor.run(code='preprocessing.py',
                        inputs=[ProcessingInput(
                           source='s3://path/to/my/input-data.csv',
                           destination='/opt/ml/processing/input')],
                         outputs=[ProcessingOutput(source='/opt/ml/processing/output/train'),
                                  ProcessingOutput(source='/opt/ml/processing/output/validation'),
                                  ProcessingOutput(source='/opt/ml/processing/output/test')]
   ```

The same procedure could be used with any other library or system dependencies\. More generally, you can use existing Docker images on Amazon SageMaker Processing, including images that you run on other platforms, such Kubernetes\.