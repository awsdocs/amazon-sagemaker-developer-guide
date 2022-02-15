# Hugging Face Framework Processor<a name="processing-job-frameworks-hugging-face"></a>

Hugging Face is an open\-source provider of natural language processing \(NLP\) models\. The `HuggingFaceProcessor` in the Amazon SageMaker Python SDK provides you with the ability to run processing jobs with Hugging Face scripts\. When you use the `HuggingFaceProcessor`, you can leverage an Amazon\-built Docker container with a managed Hugging Face environment so that you don't need to bring your own container\.

The following code example shows how you can use the `HuggingFaceProcessor` to run your Processing job using a Docker image provided and maintained by SageMaker\. Note that when you run the job, you can specify a directory containing your scripts and dependencies in the `source_dir` argument, and you can have a `requirements.txt` file located inside your `source_dir` directory that specifies the dependencies for your processing script\(s\)\. SageMaker Processing installs the dependencies in `requirements.txt` in the container for you\.

```
from sagemaker.huggingface import HuggingFaceProcessor
from sagemaker.processing import ProcessingInput, ProcessingOutput
from sagemaker import get_execution_role

#Initialize the HuggingFaceProcessor
hfp = HuggingFaceProcessor(
    role=get_execution_role(), 
    instance_count=1,
    instance_type='ml.g4dn.xlarge',
    transformers_version='4.4.2',
    pytorch_version='1.6.0', 
    base_job_name='frameworkprocessor-hf'
)

#Run the processing job
hfp.run(
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
        ProcessingOutput(output_name='train', source='/opt/ml/processing/output/train/', destination=f's3://{BUCKET}/{S3_OUTPUT_PATH}'),
        ProcessingOutput(output_name='test', source='/opt/ml/processing/output/test/', destination=f's3://{BUCKET}/{S3_OUTPUT_PATH}'),
        ProcessingOutput(output_name='val', source='/opt/ml/processing/output/val/', destination=f's3://{BUCKET}/{S3_OUTPUT_PATH}')
    ]
)
```

If you have a `requirements.txt` file, it should be a list of libraries you want to install in the container\. The path for `source_dir` can be a relative, absolute, or Amazon S3 URI path\. However, if you use an Amazon S3 URI, then it must point to a tar\.gz file\. You can have multiple scripts in the directory you specify for `source_dir`\. To learn more about the `HuggingFaceProcessor` class, see [Hugging Face Estimator](https://sagemaker.readthedocs.io/en/stable/frameworks/huggingface/sagemaker.huggingface.html) in the *Amazon SageMaker Python SDK*\.