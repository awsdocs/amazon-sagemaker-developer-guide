# Data Processing and Model Evaluation with Scikit\-Learn<a name="use-scikit-learn-processing-container"></a>

For a sample notebook that shows how to run scikit\-learn scripts to do data preprocessing and model evaluation with the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) for Processing, see [scikit\-learn Processing](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker_processing/scikit_learn_data_processing_and_model_evaluation)\. This notebook runs a processing job using `SKLearnProcessor` to execute a scikit\-learn script that you provide that preprocesses data, trains a model using an Amazon SageMaker training job, and then runs a processing job to evaluate the trained model to estimate how the model is expected to perform in production\.

The following example shows how to use a `SKLearnProcessor` to run your own scikit\-learn script using a Docker image provided and maintained by Amazon SageMaker, rather than your own Docker image\.

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