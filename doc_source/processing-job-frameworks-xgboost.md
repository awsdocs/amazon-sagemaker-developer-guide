# XGBoost Framework Processor<a name="processing-job-frameworks-xgboost"></a>

XGBoost is an open\-source machine learning framework\. The `XGBoostProcessor` in the Amazon SageMaker Python SDK provides you with the ability to run processing jobs with XGBoost scripts\. When you use the XGBoostProcessor, you can leverage an Amazon\-built Docker container with a managed XGBoost environment so that you don’t need to bring your own container\.

The following code example shows how you can use the `XGBoostProcessor` to run your Processing job using a Docker image provided and maintained by SageMaker\. Note that when you run the job, you can specify a directory containing your scripts and dependencies in the `source_dir` argument, and you can have a `requirements.txt` file located inside your `source_dir` directory that specifies the dependencies for your processing script\(s\)\. SageMaker Processing installs the dependencies in `requirements.txt` in the container for you\.

```
from sagemaker.xgboost import XGBoostProcessor
from sagemaker.processing import ProcessingInput, ProcessingOutput
from sagemaker import get_execution_role

#Initialize the XGBoostProcessor
xgb = XGBoostProcessor(
    framework_version='1.2-2',
    role=get_execution_role(),
    instance_type='ml.m5.xlarge',
    instance_count=1,
    base_job_name='frameworkprocessor-XGB',
)

#Run the processing job
xgb.run(
    code='processing-script.py',
    source_dir='scripts',
    inputs=[
        ProcessingInput(
            input_name='data',
            source=f's3://{BUCKET}/{S3_INPUT_PATH}',
            destination='/opt/ml/processing/input/data'
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

If you have a `requirements.txt` file, it should be a list of libraries you want to install in the container\. The path for `source_dir` can be a relative, absolute, or Amazon S3 URI path\. However, if you use an Amazon S3 URI, then it must point to a tar\.gz file\. You can have multiple scripts in the directory you specify for `source_dir`\. To learn more about the `XGBoostProcessor` class, see [XGBoost Estimator](https://sagemaker.readthedocs.io/en/stable/frameworks/xgboost/xgboost.html) in the *Amazon SageMaker Python SDK*\.