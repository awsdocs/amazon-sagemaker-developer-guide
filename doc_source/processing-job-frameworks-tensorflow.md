# TensorFlow Framework Processor<a name="processing-job-frameworks-tensorflow"></a>

TensorFlow is an open\-source machine learning and artificial intelligence library\. The `TensorFlowProcessor` in the Amazon SageMaker Python SDK provides you with the ability to run processing jobs with TensorFlow scripts\. When you use the `TensorFlowProcessor`, you can leverage an Amazon\-built Docker container with a managed TensorFlow environment so that you don’t need to bring your own container\.

The following code example shows how you can use the `TensorFlowProcessor` to run your Processing job using a Docker image provided and maintained by SageMaker\. Note that when you run the job, you can specify a directory containing your scripts and dependencies in the `source_dir` argument, and you can have a `requirements.txt` file located inside your `source_dir` directory that specifies the dependencies for your processing script\(s\)\. SageMaker Processing installs the dependencies in `requirements.txt` in the container for you\.

```
from sagemaker.tensorflow import TensorFlowProcessor
from sagemaker.processing import ProcessingInput, ProcessingOutput
from sagemaker import get_execution_role

#Initialize the TensorFlowProcessor
tp = TensorFlowProcessor(
    framework_version='2.3',
    role=get_execution_role(),
    instance_type='ml.m5.xlarge',
    instance_count=1,
    base_job_name='frameworkprocessor-TF',
    py_version='py37'
)

#Run the processing job
tp.run(
    code='processing-script.py',
    source_dir='scripts',
    inputs=[
        ProcessingInput(
            input_name='data',
            source=f's3://{BUCKET}/{S3_INPUT_PATH}',
            destination='/opt/ml/processing/input/data'
        ),
        ProcessingInput(
            input_name='model',
            source=f's3://{BUCKET}/{S3_PATH_TO_MODEL}',
            destination='/opt/ml/processing/input/model'
        )
    ],
    outputs=[
        ProcessingOutput(
            output_name='predictions',
            source='/opt/ml/processing/output',
            destination=f's3://{BUCKET}/{S3_OUTPUT_PATH}'
        )
    ]
)
```

If you have a `requirements.txt` file, it should be a list of libraries you want to install in the container\. The path for `source_dir` can be a relative, absolute, or Amazon S3 URI path\. However, if you use an Amazon S3 URI, then it must point to a tar\.gz file\. You can have multiple scripts in the directory you specify for `source_dir`\. To learn more about the `TensorFlowProcessor` class, see [TensorFlow Estimator](https://sagemaker.readthedocs.io/en/stable/frameworks/tensorflow/sagemaker.tensorflow.html#tensorflow-estimator) in the *Amazon SageMaker Python SDK*\.