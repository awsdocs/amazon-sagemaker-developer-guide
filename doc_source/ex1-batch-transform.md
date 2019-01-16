# Step 3\.4\.2: Deploy the Model to Amazon SageMaker Batch Transform<a name="ex1-batch-transform"></a>

Amazon SageMaker offers the ability to use batch transform either through the console or by using the API\. For more information, see [Getting Inferences by Using Amazon SageMaker Batch Transform](how-it-works-batch.md)\. The following examples assume that you have already run a training job to create a model \(see [Step 3\.3: Train a Model](ex1-train-model.md)\) and created a Model based on the training output \(see [Step 3\.4\.1: Deploy the Model to Amazon SageMaker Hosting Services ](ex1-deploy-model.md)\)\.

**Topics**
+ [Using Batch Transform \(Console\)](#ex1-batch-transform-console)
+ [Using the Batch Transform API](#ex1-batch-transform-api-examples)

## Using Batch Transform \(Console\)<a name="ex1-batch-transform-console"></a>

To generate inferences, create transform jobs by using the console\.

### Creating a Batch Transform Job \(Console\)<a name="ex1-create-batch-transform-job"></a>

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker](https://console.aws.amazon.com/sagemaker)

1. In the navigation pane, choose **Inference**, then choose **Batch transform jobs**\. 

1. Choose **Create batch transform job**\. 

1. For **Batch transform job configuration**, provide the following:

   1. For **Job name**, type a name for the new batch transform job\. 

   1. For **Model name**, choose **Choose model**, a list of available models appears\.

   1. Select the model that you want to use from the list of available models and choose **Choose model**\. 

   1. For **Instance type**, choose the instance type that you want to use for the batch transform job\. To use models created by built\-in algorithms to transform moderately\-sized datasets, ml\.m4\.xlarge or ml\.m5\.large should suffice\.

   1. For **Instance count**, type the number of compute instances to use for the transform job\. For most use cases, 1 should suffice\. By setting a higher value, you use more resources to reduce the time it takes to complete the transform job\. 

   1. \(Optional\) For **Max concurrent transforms**, type the maximum number of parallel requests to use for each instance node\. For most use cases, 1 should suffice\. To allow Amazon SageMaker to determine the appropriate number of concurrent transforms, set the value to 0 or leave it blank\.

   1. \(Optional\) For **Batch strategy**, if you want to use only one record per batch, select **SingleRecord**\. If you want to use the maximum number of records per batch, select **MultiRecord**\. 

   1. \(Optional\) For **Max payload size \(MB\)**, type the maximum payload size, in MB\. You can approximate an appropriate value by: dividing the size of your dataset by the number of records and multiply by the number of records you want per batch\. The value you enter should be proportional to the number of records you want per batch\. It is recommended to enter a slightly higher value to ensure the records will fit within the maximum payload size\. The default value is 6 MB\. For infinite stream mode, where a transform job is set to continuously input and transform data, set the value to 0\.

   1. \(Optional\) for **Environment variables**, specify a **key** and **value** for the internal Docker container to use\. To add more entries to input more variables, choose **Add environment variables**\. To remove an entry choose **Remove** next to the respective entry\. 

1. For **Input data configuration**, provide the following information:   For **S3 location**, type the path to the location where the input data is stored in Amazon S3\.   For **S3 data type**, select the S3 data type\. The default value is **ManifestFile**\. If the object is a manifest file that contains a list of objects to transform, choose **ManifestFile**\. To use all objects with a specified key name prefix, choose **S3Prefix**\.   For **Compression type**, you specify the compression used on the input data\. The default value is **None**\. If the input data is uncompressed select **None**\. If the input data is compressed using Gzip, select **Gzip**\.   \(Optional\) For **Split type**, choose how to split the data into smaller batches for the transform job\. The default is **None**\. If you don't want to split the input data, choose **None**\. If you want to split the data using newline characters\. choose **Line**\. If you want to split the data according to the RecordIO format, choose **RecordIO**\. For infinite stream mode, where a transform job is set to continuously input and transform data, choose **None**\.   \(Optional\) For **Content type**, specified the MIME type that you want to use\.   

1. For **Output data configuration**, provide the following information: 

   1. For **S3 location**, type the path to the S3 bucket where you want to store the output data\.

   1. \(Optional\) For **Encryption key**, you can add your AWS Key Management Service \(AWS KMS\) encryption key to encrypt the output data at rest\. Provide the key ID or its Amazon Resource Number \(ARN\)\. For more information, see [KMS\-Managed Encryption Keys](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingKMSEncryption.html)\.

   1. \(Optional\) For **Accept**, specify the MIME type you want to be used for the output data\.

1. \(Optional\) For **Tags**, add tags to the transform job\. A tag is a label you assign to help manage your training jobs\. A tag consists of a key and a value both of which you define\.

1. Choose **Create job**\.

### Viewing Details for a Batch Transform Job \(Console\)<a name="ex1-view-batch-transform-job"></a>

The following explain how to view details relating to a batch transform job:
+ To see details about a batch transform job, such as job name or status, choose a batch transform job from the batch transform jobs table\. 
+ To view the details of a model used in a batch transform job, on the batch transform job details page, choose the link under model name\.

### Stopping a Batch Transform Job \(Console\)<a name="ex1-stop-batch-transform-job"></a>

You can stop a batch transform job from the **Batch transform job** page or from the **Batch detail page**\.
+ 

****To stop a batch transform job using the Actions menu****

  1. Choose the checkbox near the batch transform job's name\.

  1. In the **Actions** menu, choose **Stop**\.
+ 

**To stop a batch transform job from the Batch detail page**

  1. Choose the transformation job name to view its details\.

  1. Choose **Stop**\.

## Using the Batch Transform API<a name="ex1-batch-transform-api-examples"></a>

There are several ways to interact with transform jobs\. For more information, see [CreateTransformJob](API_CreateTransformJob.md)\. You can use the high\-level Python library or the low\-level SDK \(boto3\)\.

### Using the high\-level Python Library<a name="ex1-batch-transform-api-high-level"></a>

The following shows how to create a transform job using the high\-level Python library:

```
import boto3
import sagemaker
import json

input_key = 'kmeans_batch_example/input/valid-data.csv'
input_location = 's3://{}/{}'.format(bucket, input_key)
output_location = 's3://{}/kmeans_batch_example/output'.format(bucket)

### Convert the validation set numpy array to a csv file and upload to s3
numpy.savetxt('valid-data.csv', valid_set[0], delimiter=',', fmt='%g')
s3_client = boto3.client('s3')
s3_client.upload_file('valid-data.csv', bucket, input_key)

# Initialize the transformer object
transformer =sagemaker.transformer.Transformer(
    base_transform_job_name='Batch-Transform',
    model_name=model_name,
    instance_count=1,
    instance_type='ml.c4.xlarge',
    output_path=output_location
    )
# To start a transform job:
transformer.transform(input_location, content_type='text/csv', split_type='Line')
# Then wait until transform job is completed
transformer.wait()

# To fetch validation result 
s3_client.download_file(bucket, 'kmeans_batch_example/output/valid-data.csv.out', 'valid-result')
with open('valid-result') as f:
    results = f.readlines()   
print("Sample transform result: {}".format(results[0]))
```

### Using the low\-level SDK \(boto3\)<a name="ex1-batch-transform-api-low-level"></a>

The following examples demonstrate how to interact with transform jobs using The Amazon SageMaker low\-level SDK:

#### Using CreateTransformJob<a name="w8aac13c16c27c11b9c11b5"></a>

To start using batch transform, you need to create a transform job\. The low\-level AWS SDK for Python provides the corresponding `create_transform_job` method\. First, import the needed libraries \(`boto3`\) and classes \(`gtime` and `strftime` from the time library\)\. Second, set the needed variables \(`job_name`, `bucket`, `prefix`, `sm`, and `model_name`\)\. The `sm`, variable is especially important as it allows us to use the Amazon SageMaker API\. Third you write the request\. For information, see [CreateTransformJob](API_CreateTransformJob.md)\.

To help prevent errors and improve readability, use the variables you created in the previous step in creating the request\. Finally, use the request with `create_transform_job` to start a batch transform job\. The following code shows how to use the `create_transform_job` operation:

```
import boto3
import sagemaker
import json
from urllib.parse import urlparse
from time import gmtime, strftime

batch_job_name = 'Batch-Transform-' + strftime("%Y-%m-%d-%H-%M-%S", gmtime())
input_location = 's3://{}/kmeans_batch_example/input'.format(bucket)
output_location = 's3://{}/kmeans_batch_example/output'.format(bucket)

### Convert the validation set numpy array to a csv file and upload to s3
numpy.savetxt('valid-data.csv', valid_set[0], delimiter=',', fmt='%g')
s3_client = boto3.client('s3')
input_key = "{}/valid_data.csv".format(urlparse(input_location).path.lstrip('/'))
s3_client.upload_file('valid-data.csv', bucket, input_key)

### Create a transform job
sm = boto3.client('sagemaker')

request = \
{
    "TransformJobName": batch_job_name,
    "ModelName": model_name,
    "MaxConcurrentTransforms": 4,
    "MaxPayloadInMB": 6,
    "BatchStrategy": "MultiRecord",
    "TransformOutput": {
        "S3OutputPath": output_location
    },
    "TransformInput": {
        "DataSource": {
            "S3DataSource": {
                "S3DataType": "S3Prefix",
                "S3Uri": input_location 
            }
        },
        "ContentType": "text/csv",
        "SplitType": "Line",
        "CompressionType": "None"
    },
    "TransformResources": {
            "InstanceType": "ml.m4.xlarge",
            "InstanceCount": 1
    }
}

sm.create_transform_job(**request)

print("Created Transform job with name: ", batch_job_name)

### Wait until job completion
while(True):
    response = sm.describe_transform_job(TransformJobName=batch_job_name)
    status = response['TransformJobStatus']
    if  status == 'Completed':
        print("Transform job ended with status: " + status)
        break
    if status == 'Failed':
        message = response['FailureReason']
        print('Transform failed with the following error: {}'.format(message))
        raise Exception('Transform job failed') 
    print("Transform job is still in status: " + status)    
    time.sleep(30)    

### Fetch transform output
output_key = "{}/valid_data.csv.out".format(urlparse(output_location).path.lstrip('/'))
s3_client.download_file(bucket, output_key, 'valid-result')
with open('valid-result') as f:
    results = f.readlines()   
print("Sample transform result: {}".format(results[0]))
```

#### Using DescribeTransformJob<a name="w8aac13c16c27c11b9c11b7"></a>

To see the details of a specific transform job, specify the name in a describe transform jobs request\. The low\-level AWS SDK for Python provides the corresponding `describe_transform_jobs` method\. The following code shows how to use the `describe_transform_job` method:

```
response = sm.describe_transform_job(TransformJobName=job_name)
```

#### Using ListTransformJobs<a name="w8aac13c16c27c11b9c11b9"></a>

To list previous transform jobs, specify criteria to list and to sort the list\. The low\-level AWS SDK for Python provides the corresponding `list_transform_jobs` method\. The following code shows how to use the `list_transform_jobs` method:

```
import boto3
from time import gmtime, strftime

request = \
{
   "StatusEquals": "Completed",
   "SortBy": "CreationTime",
   "SortOrder": "Descending",
   "MaxResults": 20,
}

response = sm.list_transform_jobs(**request)
```

**Tip**  
When using the `list_transform_jobs` method, you can specify any or all of the members in the request structure\. For more information, see [ListTransformJobs](API_ListTransformJobs.md)\.

**Next Step**  
[Step 3\.5: Validate the Model](ex1-test-model.md)