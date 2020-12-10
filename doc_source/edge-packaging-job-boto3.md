# Package a Model \(Boto3\)<a name="edge-packaging-job-boto3"></a>

You can create a SageMaker Edge Manager packaging job with the AWS SDK for Python \(Boto3\)\. Before continuing, make sure you have satisfied the [Prerequisites](/sagemaker/dg/latest/edge-packaging-job-prerequisites)\.

To request an edge packaging job, use `CreateEdgePackagingJob`\. You need to provide a name to your edge packaging job, the name of your SageMaker Neo compilation job, your role Amazon resource name \(ARN\), a name for your model, a version for your model, and the Amazon S3 bucket URI where you want to store the output of your packaging job\.

```
# Import AWS SDK for Python (Boto3)
import boto3

# Create Edge client so you can submit a packaging job
sagemaker_client = boto3.client("sagemaker", region_name='aws-region')

sagemaker_client.create_edge_packaging_job(
    EdgePackagingJobName="edge-packaging-name",
    CompilationJobName="neo-compilation-name",
    RoleArn="arn:aws:iam::99999999999:role/rolename",
    ModelName="sample-model-name",
    ModelVersion="model-version",
    OutputConfig={
        "S3OutputLocation": "s3://your-bucket/",
    }
)
```

You can check the status of an edge packaging job using `DescribeEdgePackagingJob`:

```
response = sagemaker_client.describe_edge_packaging_job(
                                    EdgePackagingJobName="edge-packaging-name")
```

This returns a dictionary that can be used to poll the status of the packaging job:

```
# Optional - Poll every 30 sec to check completion status
import time

while True:
    response = sagemaker_client.describe_edge_packaging_job(
                                         EdgePackagingJobName="edge-packaging-name")
    
    if response['EdgePackagingJobStatus'] == 'Completed':
        break
    elif response['EdgePackagingJobStatus'] == 'Failed':
        raise RuntimeError('Packaging job failed')
    print('Packaging model...')
    time.sleep(30)
print('Done!')
```

For a list of packaging jobs, use `ListEdgePackagingJobs`\. You can use this API to search for a specific packaging job\. Provide a partial name to filter packaging job names for `NameContains`, a partial name for `ModelNameContains` to filter for jobs in which the model name contains the name you provide\. Also specify with which column to sort with `SortBy`, and by which direction to sort for `SortOrder` \(either `Ascending` or `Descending`\)\.

```
sagemaker_client.list_edge_packaging_jobs(
    "NameContains": "sample",
    "ModelNameContains": "sample",
    "SortBy": "column-name",
    "SortOrder": "Descending"
)
```

To stop a packaging job, use `StopEdgePackagingJob` and provide the name of your edge packaging job\.

```
sagemaker_client.stop_edge_packaging_job(
        EdgePackagingJobName="edge-packaging-name"
)
```

For a full list of Edge Manager APIs, see the [Boto3 documentation](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html)\.