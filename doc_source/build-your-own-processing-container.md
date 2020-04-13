# Build Your Own Processing Container<a name="build-your-own-processing-container"></a>

You can provide SageMaker Processing a Docker image to run your data processing, feature engineering, and model evaluation workloads\. You can add your own dependencies to this Docker image, and SageMaker Processing will run this image with your own code and dependencies\. 

The following example of a Dockerfile builds a container with the Python libraries “scikit\-learn” and “pandas” that you can run as a processing job\. 

```
FROM python:3.7-slim-buster

# Install scikit-learn and pandas
RUN pip3 install pandas==0.25.3 scikit-learn==0.21.3

# Add a python script and configure Docker to run it
ADD processing_script.py /
ENTRYPOINT ["python3", "/processing_script.py"]
```

After building and pushing this Docker image to an Amazon Elastic Container Registry \(Amazon ECR\) repository and ensuring that your Amazon SageMaker IAM role can pull the image from Amazon ECR, you can run this image on Amazon SageMaker Processing\.

## How Amazon SageMaker Runs Your Processing Image<a name="byoc-run-image"></a>

Amazon SageMaker Processing runs the container in a similar way as the following command, where `AppSpecification.ImageUri` is the Amazon ECR image URI that you specify in your `CreateProcessingJob` API\. 

```
docker run [AppSpecification.ImageUri]
```

This runs the entrypoint command configured in your Docker image\. 

You can also override the entrypoint command in the image or give command\-line arguments to your entrypoint command using the `AppSpecification.ContainerEntrypoint` and `AppSpecification.ContainerArgument`s parameters in your CreateProcessingJob request\. Specifying these parameters configures Amazon SageMaker Processing to run the container in a similar way as the following command\. 

```
 docker run --entry-point [AppSpecification.ContainerEntrypoint] [AppSpecification.ImageUri] [AppSpecification.ContainerArguments]
```

For example, if you specify the `ContainerEntrypoint` to be `[“python3”, “-v”, “/processing_script.py”]` in your `CreateProcessingJob `request, and `ContainerArguments` to be `[“—data-format“, ”csv“]`, Processing runs your container with the command `”python3 -v /processing_script.py —data-format csv“`\. 

Be aware of the following details when building your processing container: 
+ Processing decides whether the job completes or fails depending on the exit code of the command run\. A processing job completes if all of the processing containers exit successfully with an exit code of 0, and fails if any of the containers exits with a non\-zero exit code\.
+  Processing lets you override the processing container’s entrypoint and set command\-line arguments just like you can with the Docker API\. Docker images can also configure the entrypoint and command\-line arguments using the `ENTRYPOINT` and CMD instructions\. The way `CreateProcessingJob`’s `ContainerEntrypoint` and `ContainerArgument` parameters configure a Docker image’s entrypoint and arguments mirrors how Docker overrides the entrypoint and arguments through the Docker API:
  + If neither `ContainerEntrypoint` nor `ContainerArguments` are provided, the default `ENTRYPOINT` or CMD in the image is used\.
  + If `ContainerEntrypoint` is provided, but not `ContainerArguments`, the image is run with the given entrypoint, and the `ENTRYPOINT` and CMD in the image are ignored\.
  + If `ContainerArguments` is provided, but not `ContainerEntrypoint`, the image is run with the default `ENTRYPOINT` in the image with the arguments provided\.
  + If both `ContainerEntrypoint` and `ContainerArguments` are provided, the image is run with the given entrypoint and arguments, and the `ENTRYPOINT` and CMD in the image are ignored\.
+ Use the “exec” form of the ENTRYPOINT instruction in your Dockerfile \(`ENTRYPOINT` `["executable", "param1", "param2"])` instead of the “shell” form \(`ENTRYPOINT`` command param1 param2`\)\. This lets your processing container receive `SIGINT` and `SIGKILL` signals, which Processing uses to stop processing jobs using the `StopProcessingJob` API\.
+ `/opt/ml` and all subdirectories are reserved by Amazon SageMaker\. When building your processing Docker image, don't place any data required by your processing container in these directories\.
+ If you plan to use GPU devices, make sure that your containers are nvidia\-docker compatible\. Only the CUDA toolkit should be included on containers\. Don't bundle NVIDIA drivers with the image\. For more information about nvidia\-docker, see [NVIDIA/nvidia\-docker](https://github.com/NVIDIA/nvidia-docker)\.

## How Amazon SageMaker Processing Configures Input and Output For Your Processing Container<a name="byoc-input-and-output"></a>

When you create a processing job using the `CreateProcessingJob` API, you can specify multiple `ProcessingInputs` and `ProcessingOutputs`\. values 

A `ProcessingInput` configures an Amazon S3 URI to download data from, and a path in your processing container to download the data to\. A `ProcessingOutput` configures a path in your processing container to upload data from, and where in Amazon S3 to upload that data to\. For both `ProcessingInput` and `ProcessingOutput`, the path in the processing container must begin with `/opt/ml/processing/ `

For example, you might create a processing job with one `ProcessingInput` that downloads data from `s3://your-data-bucket/path/to/input/csv/data `into a path `/opt/ml/processing/csv` in your processing container, and a ProcessingOutput that uploads data from `/opt/ml/processing/processed_csv` to `s3://your-data-bucket/path/to/output/csv/data`\. The code running in your processing code might read this data, and write output data to `/opt/ml/processing/processed_csv`\. Processing uploads the data written into this path to the Amazon S3 output location\. 

## How Amazon SageMaker Processing Provides Logs and Metrics for your Processing Container<a name="byoc-logs-and-metrics"></a>

When your processing container writes to `stdout` or `stderr`, Processing saves the output from each processing container and puts the output in Amazon CloudWatch logs\. For information about logging, see [Log Amazon SageMaker Events with Amazon CloudWatch](logging-cloudwatch.md)\.

Processing also provides CloudWatch metrics for each instance running your processing container\. For information about metrics, see the [Monitor Amazon SageMaker with Amazon CloudWatch](monitoring-cloudwatch.md)\. 

## How Amazon SageMaker Processing Provides Configuration to Your Processing Container<a name="byoc-config"></a>

Amazon SageMaker Processing provides configuration to your processing container through environment variables and two JSON files at predefined locations in the processing container, `/opt/ml/config/processingjobconfig.json` and `/opt/ml/config/resourceconfig.json`\. 

Your processing job is started with the environment variables configured using the `Environment` map in the `CreateProcessingJob` API request\. The file `/opt/ml/config/processingjobconfig.json` contains information from the `CreateProcessingJob` request\. 

The following example shows the format of the file\. 

```
{
    "ProcessingJobArn": "<processing_job_arn>",
    "ProcessingJobName": "<processing_job_name>",
    "AppSpecification": {
        "ImageUri": "<image_uri>",
        "ContainerEntrypoint": null,
        "ContainerArguments": null
    },
    "Environment": {
        "KEY": "VALUE"
    },
    "ProcessingInputs": [
        {
            "InputName": "input-1",
            "S3Input": {
                "LocalPath": "/opt/ml/processing/input/dataset",
                "S3Uri": "<s3_uri>",
                "S3DataDistributionType": "FullyReplicated",
                "S3DataType": "S3Prefix",
                "S3InputMode": "File",
                "S3CompressionType": "None",
                "S3DownloadMode": "StartOfJob"
            }
        }
    ],
    "ProcessingOutputConfig": {
        "Outputs": [
            {
                "OutputName": "output-1",
                "S3Output": {
                    "LocalPath": "/opt/ml/processing/output/dataset",
                    "S3Uri": "<s3_uri>",
                    "S3UploadMode": "EndOfJob"
                }
            }
        ],
        "KmsKeyId": null
    },
    "ProcessingResources": {
        "ClusterConfig": {
            "InstanceCount": 1,
            "InstanceType": "ml.m5.xlarge",
            "VolumeSizeInGB": 30,
            "VolumeKmsKeyId": null
        }
    },
    "RoleArn": "<IAM role>",
    "StoppingCondition": {
        "MaxRuntimeInSeconds": 86400
    }
}
```

The `/opt/ml/config/resourceconfig.json` file contains information about the hostnames of your processing containers\. Use these hostnames when creating or running distributed processing code\.

```
{
  "current_host": "algo-1",
  "hosts": ["algo-1","algo-2","algo-3"]
}
```

Don't use the information in `/etc/hostname` or `/etc/hosts` because it might be inaccurate\. Hostname information might not be immediately available to the processing container\. We recommend adding a retry policy on hostname resolution operations as nodes become available in the cluster\.

## How You Can Save Metadata Information About Your Processing Job<a name="byoc-metadata"></a>

Your processing containers can write to a UTF\-8 to the file /opt/ml/output/message to communicate metadata from the processing container after the processing container exits\. After the processing job enters any terminal status \("`Completed`", "`Stopped`", or "`Failed`"\), the "`ExitMessage`" field in [ `DescribeProcessingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeProcessingJob.html) will contain the first 1 KB of this file\. The [ `DescribeProcessingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeProcessingJob.html) call returns up to 1 KB of data from your processing containers through the `ExitMessage` parameter\. For example, for failed processing jobs, you can use this field to communicate why the processing container failed

Do not write sensitive data to this file\. If the data in this file is not UTF\-8 encoded, the job will fail with a `ClientError`\. If multiple containers exit with "`ExitMessage",` the `"ExitMessage" `content from each processing container is concatenated, then truncated to 1 KB\.

## How You Can Run Your Processing Container Using the SageMaker Python SDK<a name="byoc-run"></a>

You can use the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) to run your own processing image by using the `Processor` class\. The following example shows how to run your own processing container with one input from Amazon Simple Storage Service \(Amazon S3\), and one output to Amazon S3\.

```
from sagemaker.processing import Processor, ProcessingInput, ProcessingOutput

processor = Processor(image_uri='<your_ecr_image_uri>',
                     role=role,
                     instance_count=1,
                     instance_type="ml.m5.xlarge")
                     
processor.run(inputs=[ProcessingInput(
                        source='<s3_uri or local path>',
                        destination='/opt/ml/processing/input_data')],
                    outputs=[ProcessingOutput(
                        source='/opt/ml/processing/processed_data',
                        destination='<s3_uri>')],
                    ))
```

Instead of building your processing code into your processing image, you can provide a `ScriptProcessor` with your own image and the command that you want to run, along with the code that you want to run inside that container, as in the [Run Scripts with Your Own Processing Container](processing-container-run-scripts.md) example\.

You can also use the scikit\-learn image that Processing provides through `SKLearnProcessor` to run scikit\-learn scripts, as in [Data Processing and Model Evaluation with Scikit\-Learn](use-scikit-learn-processing-container.md)\. To learn more about using the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) with Processing containers, see [SageMaker Python SDK ReadTheDocs](https://sagemaker.readthedocs.io/en/stable/)\.