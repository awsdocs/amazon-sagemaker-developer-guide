# PyTorch Framework Processor<a name="processing-job-frameworks-pytorch"></a>

PyTorch is an open\-source machine learning framework\. The `PyTorchProcessor` in the Amazon SageMaker Python SDK provides you with the ability to run processing jobs with PyTorch scripts\. When you use the `PyTorchProcessor`, you can leverage an Amazon\-built Docker container with a managed PyTorch environment so that you don’t need to bring your own container\.

The following code example shows how you can use the `PyTorchProcessor` to run your Processing job using a Docker image provided and maintained by SageMaker\. Note that when you run the job, you can specify a directory containing your scripts and dependencies in the `source_dir` argument, and you can have a `requirements.txt` file located inside your `source_dir` directory that specifies the dependencies for your processing script\(s\)\. SageMaker Processing installs the dependencies in `requirements.txt` in the container for you\.

```
from sagemaker.pytorch.processing import PyTorchProcessor
from sagemaker.processing import ProcessingInput, ProcessingOutput
from sagemaker import get_execution_role

#Initialize the PyTorchProcessor
pytorch_processor = PyTorchProcessor(
    framework_version='1.8',
    role=get_execution_role(),
    instance_type='ml.m5.xlarge',
    instance_count=1,
    base_job_name='frameworkprocessor-PT'
)

#Run the processing job
pytorch_processor.run(
    code='processing-script.py',
    source_dir='scripts',
    inputs=[
        ProcessingInput(
            input_name='data',
            source=f's3://{BUCKET}/{S3_INPUT_PATH}',
            destination='/opt/ml/processing/input'
        )
    ],
    outputs=[
        ProcessingOutput(output_name='data_structured', source='/opt/ml/processing/tmp/data_structured', destination=f's3://{BUCKET}/{S3_OUTPUT_PATH}'),
        ProcessingOutput(output_name='train', source='/opt/ml/processing/output/train', destination=f's3://{BUCKET}/{S3_OUTPUT_PATH}'),
        ProcessingOutput(output_name='validation', source='/opt/ml/processing/output/val', destination=f's3://{BUCKET}/{S3_OUTPUT_PATH}'),
        ProcessingOutput(output_name='test', source='/opt/ml/processing/output/test', destination=f's3://{BUCKET}/{S3_OUTPUT_PATH}'),
        ProcessingOutput(output_name='logs', source='/opt/ml/processing/logs', destination=f's3://{BUCKET}/{S3_OUTPUT_PATH}')
    ]
)
```

If you have a `requirements.txt` file, it should be a list of libraries you want to install in the container\. The path for `source_dir` can be a relative, absolute, or Amazon S3 URI path\. However, if you use an Amazon S3 URI, then it must point to a tar\.gz file\. You can have multiple scripts in the directory you specify for `source_dir`\. To learn more about the `PyTorchProcessor` class, see [PyTorch Estimator](https://sagemaker.readthedocs.io/en/stable/frameworks/pytorch/sagemaker.pytorch.html) in the *Amazon SageMaker Python SDK*\.