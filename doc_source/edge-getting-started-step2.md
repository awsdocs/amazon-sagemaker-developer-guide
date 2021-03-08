# Package a Compiled Model<a name="edge-getting-started-step2"></a>

Packaging jobs take SageMaker Neo–compiled models and make any changes necessary to deploy the model with the inference engine, Edge Manager agent\. To package your model, create an edge packaging job with the `create_edge_packaging` API or the SageMaker console\.

You need to provide the name that you used for your Neo compilation job, a name for the packaging job, a role ARN \(see [Prerequisites](edge-getting-started-step1.md) section\) , a name for the model, a model version, and the Amazon S3 bucket URI for the output of the packaging job\. The following is an example using the API\.

```
# Create SageMaker client so you can submit a packaging job
sagemaker_client = boto3.client("sagemaker", region_name=AWS_REGION)

edge_packaging_name='edge-edge-packaging-demo'
compilation_job_name='getting-started-demo'

# Folder within S3 and full S3 bucket URI
folder="package_output"
s3_output_location=f"s3://{bucket}/{folder}"

# Give your model a name and version for reference
model_name="packaged-mobilenet-model"
model_version="1.1"

sagemaker_client.create_edge_packaging_job(
    EdgePackagingJobName=edge_packaging_name,
    CompilationJobName=compilation_job_name,
    RoleArn=role_arn,
    ModelName=model_name,
    ModelVersion=model_version,
    OutputConfig={
        "S3OutputLocation": s3_output_location,
    }
)

# Optional - Poll every 30 sec to check completion status
import time

while True:
    response = sagemaker_client.describe_edge_packaging_job(
                                            EdgePackagingJobName=edge_packaging_name)
    
    if response['EdgePackagingJobStatus'] == 'Completed':
        break
    elif response['EdgePackagingJobStatus'] == 'Failed':
        raise RuntimeError('Packaging job failed')
    print('Packaging model...')
    time.sleep(30)
print('Done!')
```

Once the packaging job is complete, download the model locally to your device\. You can use the Amazon S3 client previously defined to download the model:

```
s3_client.download_file(bucket=fleet_bucket,
                        key=folder,
                        filename="packaged-mobilenet-model-1.1.tar.gz"
```

You can also download your model using the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\. Search for your bucket by providing a partial name of your bucket in the search field\.