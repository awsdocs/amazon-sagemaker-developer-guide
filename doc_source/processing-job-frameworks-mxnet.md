# MXNet Framework Processor<a name="processing-job-frameworks-mxnet"></a>

Apache MXNet is an open\-source deep learning framework commonly used for training and deploying neural networks\. The `MXNetProcessor` in the Amazon SageMaker Python SDK provides you with the ability to run processing jobs with MXNet scripts\. When you use the `MXNetProcessor`, you can leverage an Amazon\-built Docker container with a managed MXNet environment so that you don’t need to bring your own container\.

The following code example shows how you can use the `MXNetProcessor` to run your Processing job using a Docker image provided and maintained by SageMaker\. Note that when you run the job, you can specify a directory containing your scripts and dependencies in the `source_dir` argument, and you can have a `requirements.txt` file located inside your `source_dir` directory that specifies the dependencies for your processing script\(s\)\. SageMaker Processing installs the dependencies in `requirements.txt` in the container for you\.

```
from sagemaker.mxnet import MXNetProcessor
from sagemaker.processing import ProcessingInput, ProcessingOutput
from sagemaker import get_execution_role

#Initialize the MXNetProcessor
mxp = MXNetProcessor(
    framework_version='1.8.0',
    py_version='py37',
    role=get_execution_role(), 
    instance_count=1,
    instance_type='ml.c5.xlarge',
    base_job_name='frameworkprocessor-mxnet'
)

#Run the processing job
mxp.run(
    code='processing-script.py',
    source_dir='scripts',
    inputs=[
        ProcessingInput(
            input_name='data',
            source=f's3://{BUCKET}/{S3_INPUT_PATH}',
            destination='/opt/ml/processing/input/data/'
        )
    ],
    outputs=[
        ProcessingOutput(
            output_name='processed_data',
            source='/opt/ml/processing/output/',
            destination=f's3://{BUCKET}/{S3_OUTPUT_PATH}'
        )
    ]
)
```

If you have a `requirements.txt` file, it should be a list of libraries you want to install in the container\. The path for `source_dir` can be a relative, absolute, or Amazon S3 URI path\. However, if you use an Amazon S3 URI, then it must point to a tar\.gz file\. You can have multiple scripts in the directory you specify for `source_dir`\. To learn more about the `MXNetProcessor` class, see [MXNet Estimator](https://sagemaker.readthedocs.io/en/stable/frameworks/mxnet/sagemaker.mxnet.html#mxnet-estimator) in the *Amazon SageMaker Python SDK*\.