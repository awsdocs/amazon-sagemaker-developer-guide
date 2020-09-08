# Build Your Own Processing Container \(Advanced Scenario\)<a name="build-your-own-processing-container"></a>

You can provide Amazon SageMaker Processing with a Docker image that has your own code and dependencies to run your data processing, feature engineering, and model evaluation workloads\. 

The following example of a Dockerfile builds a container with the Python libraries scikit\-learn and pandas, which you can run as a processing job\. 

```
FROM python:3.7-slim-buster

# Install scikit-learn and pandas
RUN pip3 install pandas==0.25.3 scikit-learn==0.21.3

# Add a Python script and configure Docker to run it
ADD processing_script.py /
ENTRYPOINT ["python3", "/processing_script.py"]
```

Build and push this Docker image to an Amazon Elastic Container Registry \(Amazon ECR\) repository and ensure that your SageMaker IAM role can pull the image from Amazon ECR\. Then you can run this image on Amazon SageMaker Processing\.

## How Amazon SageMaker Processing Runs Your Processing Container Image<a name="byoc-run-image"></a>

Amazon SageMaker Processing runs your processing container image in a similar way as the following command, where `AppSpecification.ImageUri` is the Amazon ECR image URI that you specify in a `CreateProcessingJob` operation\. 

```
docker run [AppSpecification.ImageUri]
```

This command runs the `ENTRYPOINT` command configured in your Docker image\. 

You can also override the entrypoint command in the image or give command\-line arguments to your entrypoint command using the `AppSpecification.ContainerEntrypoint` and `AppSpecification.ContainerArgument` parameters in your `CreateProcessingJob` request\. Specifying these parameters configures Amazon SageMaker Processing to run the container similar to the way that the following command does\. 

```
 docker run --entry-point [AppSpecification.ContainerEntrypoint] [AppSpecification.ImageUri] [AppSpecification.ContainerArguments]
```

For example, if you specify the `ContainerEntrypoint` to be `[python3, -v, /processing_script.py]` in your `CreateProcessingJob `request, and `ContainerArguments` to be `[data-format, csv]`, Amazon SageMaker Processing runs your container with the following command\. 

```
 python3 -v /processing_script.py data-format csv 
```

 When building your processing container, consider the following details: 
+ Amazon SageMaker Processing decides whether the job completes or fails depending on the exit code of the command run\. A processing job completes if all of the processing containers exit successfully with an exit code of 0, and fails if any of the containers exits with a non\-zero exit code\.
+  Amazon SageMaker Processing lets you override the processing container's entrypoint and set command\-line arguments just like you can with the Docker API\. Docker images can also configure the entrypoint and command\-line arguments using the `ENTRYPOINT` and CMD instructions\. The way `CreateProcessingJob`'s `ContainerEntrypoint` and `ContainerArgument` parameters configure a Docker image's entrypoint and arguments mirrors how Docker overrides the entrypoint and arguments through the Docker API:
  + If neither `ContainerEntrypoint` nor `ContainerArguments` are provided, Processing uses the default `ENTRYPOINT` or CMD in the image\.
  + If `ContainerEntrypoint` is provided, but not `ContainerArguments`, Processing runs the image with the given entrypoint, and ignores the `ENTRYPOINT` and CMD in the image\.
  + If `ContainerArguments` is provided, but not `ContainerEntrypoint`, Processing runs the image with the default `ENTRYPOINT` in the image and with the provided arguments\.
  + If both `ContainerEntrypoint` and `ContainerArguments` are provided, Processing runs the image with the given entrypoint and arguments, and ignores the `ENTRYPOINT` and CMD in the image\.
+ You must use the exec form of the `ENTRYPOINT` instruction in your Dockerfile \(`ENTRYPOINT` `["executable", "param1", "param2"])` instead of the shell form \(`ENTRYPOINT`` command param1 param2`\)\. This lets your processing container receive `SIGINT` and `SIGKILL` signals, which Processing uses to stop processing jobs with the `StopProcessingJob` API\.
+ `/opt/ml` and all its subdirectories are reserved by SageMaker\. When building your Processing Docker image, don't place any data required by your processing container in these directories\.
+ If you plan to use GPU devices, make sure that your containers are nvidia\-docker compatible\. Include only the CUDA toolkit in containers\. Don't bundle NVIDIA drivers with the image\. For more information about nvidia\-docker, see [NVIDIA/nvidia\-docker](https://github.com/NVIDIA/nvidia-docker)\.

## How Amazon SageMaker Processing Configures Input and Output For Your Processing Container<a name="byoc-input-and-output"></a>

When you create a processing job using the `CreateProcessingJob` operation, you can specify multiple `ProcessingInput` and `ProcessingOutput`\. values\. 

You use the `ProcessingInput` parameter to specify an Amazon Simple Storage Service \(Amazon S3\) URI to download data from, and a path in your processing container to download the data to\. The `ProcessingOutput` parameter configures a path in your processing container from which to upload data, and where in Amazon S3 to upload that data to\. For both `ProcessingInput` and `ProcessingOutput`, the path in the processing container must begin with `/opt/ml/processing/ `\.

For example, you might create a processing job with one `ProcessingInput` parameter that downloads data from `s3://your-data-bucket/path/to/input/csv/data` into `/opt/ml/processing/csv` in your processing container, and a `ProcessingOutput` parameter that uploads data from `/opt/ml/processing/processed_csv` to `s3://your-data-bucket/path/to/output/csv/data`\. Your processing job would read the input data, and write output data to `/opt/ml/processing/processed_csv`\. Then it uploads the data written to this path to the specified Amazon S3 output location\. 

**Important**  
Symbolic links \(symlinks\) can not be used to upload output data to Amazon S3\. Symlinks are not followed when uploading output data\. 

## How Amazon SageMaker Processing Provides Logs and Metrics for Your Processing Container<a name="byoc-logs-and-metrics"></a>

When your processing container writes to `stdout` or `stderr`, Amazon SageMaker Processing saves the output from each processing container and puts it in Amazon CloudWatch logs\. For information about logging, see [Log Amazon SageMaker Events with Amazon CloudWatch](logging-cloudwatch.md)\.

Amazon SageMaker Processing also provides CloudWatch metrics for each instance running your processing container\. For information about metrics, see [Monitor Amazon SageMaker with Amazon CloudWatch](monitoring-cloudwatch.md)\. 

## How Amazon SageMaker Processing Configures Your Processing Container<a name="byoc-config"></a>

Amazon SageMaker Processing provides configuration information to your processing container through environment variables and two JSON files—`/opt/ml/config/processingjobconfig.json` and `/opt/ml/config/resourceconfig.json`— at predefined locations in the container\. 

When a processing job starts, it uses the environment variables that you specified with the `Environment` map in the `CreateProcessingJob` request\. The `/opt/ml/config/processingjobconfig.json` file contains information about the hostnames of your processing containers, and is also specified in the `CreateProcessingJob` request\. 

The following example shows the format of the `/opt/ml/config/processingjobconfig.json` file\.

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

The `/opt/ml/config/resourceconfig.json` file contains information about the hostnames of your processing containers\. Use the following hostnames when creating or running distributed processing code\.

```
{
  "current_host": "algo-1",
  "hosts": ["algo-1","algo-2","algo-3"]
}
```

Don't use the information about hostnames contained in `/etc/hostname` or `/etc/hosts` because it might be inaccurate\.

Hostname information might not be immediately available to the processing container\. We recommend adding a retry policy on hostname resolution operations as nodes become available in the cluster\.

## Save and Access Metadata Information About Your Processing Job<a name="byoc-metadata"></a>

To save metadata from the processing container after exiting it, containers can write UTF\-8 encoded text to the `/opt/ml/output/message` file\. After the processing job enters any terminal status \("`Completed`", "`Stopped`", or "`Failed`"\), the "`ExitMessage`" field in [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeProcessingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeProcessingJob.html) contains the first 1 KB of this file\. Access that initial part of file with a call to [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeProcessingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeProcessingJob.html), which returns it through the `ExitMessage` parameter\. For failed processing jobs, you can use this field to communicate information about why the processing container failed\.

**Important**  
Don't write sensitive data to the `/opt/ml/output/message` file\. 

If the data in this file isn't UTF\-8 encoded, the job fails and returns a `ClientError`\. If multiple containers exit with an `ExitMessage,` the content of the `ExitMessage` from each processing container is concatenated, then truncated to 1 KB\.

## Run Your Processing Container Using the SageMaker Python SDK<a name="byoc-run"></a>

You can use the SageMaker Python SDK to run your own processing image by using the `Processor` class\. The following example shows how to run your own processing container with one input from Amazon Simple Storage Service \(Amazon S3\) and one output to Amazon S3\.

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

Instead of building your processing code into your processing image, you can provide a `ScriptProcessor` with your image and the command that you want to run, along with the code that you want to run inside that container\. For an example, see [Run Scripts with Your Own Processing Container](processing-container-run-scripts.md)\.

You can also use the scikit\-learn image that Amazon SageMaker Processing provides through `SKLearnProcessor` to run scikit\-learn scripts\. For an example, see [Process Data and Evaluate Models with scikit\-learn](use-scikit-learn-processing-container.md)\. 