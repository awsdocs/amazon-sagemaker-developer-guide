# Process Data and Evaluate Models with scikit\-learn<a name="use-scikit-learn-processing-container"></a>

For a sample notebook that shows how to run scikit\-learn scripts using a Docker image provided and maintained by SageMaker to preprocess data and evaluate models, see [scikit\-learn Processing](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker_processing/scikit_learn_data_processing_and_model_evaluation)\. To use this notebook, you need to install the SageMaker Python SDK for Processing\. 

This notebook runs a processing job using `SKLearnProcessor` class from the the SageMaker Python SDK to execute a scikit\-learn script that you provide\. The script preprocesses data, trains a model using a SageMaker training job, and then runs a processing job to evaluate the trained model\. The processing job estimates how the model is expected to perform in production\.

To learn more about using the SageMaker Python SDK with Processing containers, see [SageMaker Python SDK ReadTheDocs](https://sagemaker.readthedocs.io/en/stable/)\.

The following code example shows how the notebook uses `SKLearnProcessor` to run your own scikit\-learn script using a Docker image provided and maintained by SageMaker, instead of your own Docker image\.

```
from sagemaker.sklearn.processing import SKLearnProcessor
from sagemaker.processing import ProcessingInput, ProcessingOutput

sklearn_processor = SKLearnProcessor(framework_version='0.20.0',
                                     role=role,
                                     instance_type='ml.m5.xlarge',
                                     instance_count=1)

sklearn_processor.run(code='preprocessing.py',
                      inputs=[ProcessingInput(
                        source='s3://path/to/my/input-data.csv',
                        destination='/opt/ml/processing/input')],
                      outputs=[ProcessingOutput(source='/opt/ml/processing/output/train'),
                               ProcessingOutput(source='/opt/ml/processing/output/validation'),
                               ProcessingOutput(source='/opt/ml/processing/output/test')]
                     )
```

To process data in parallel using Scikit\-Learn on Amazon SageMaker Processing, you can shard input objects by S3 key by setting `s3_data_distribution_type='ShardedByS3Key'` inside a `ProcessingInput` so that each instance receives about the same number of input objects\.