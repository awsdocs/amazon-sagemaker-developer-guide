# Step 2\.4\.2: Deploy the Model with Batch Transform<a name="ex1-batch-transform"></a>

To to get inference for an entire dataset, use batch transform\. You can use batch transform in the console or by using the API\.

For information about batch transforms, see [Get Inferences for an Entire Dataset with Batch Transform](how-it-works-batch.md)\. For an example that uses batch transform, see the [batch transform sample notebook](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/advanced_functionality/batch_transform)\.

**Topics**
+ [Deploy a Model with Batch Transform \(API\)](#ex1-batch-transform-api-examples)

## Deploy a Model with Batch Transform \(API\)<a name="ex1-batch-transform-api-examples"></a>

 To create a batch transform job, you can use the Amazon SageMaker high\-level Python library or the low\-level SDK for Python \(Boto 3\)\.

### Deploy a Model with Batch Transform \(Amazon SageMaker High\-level Python Library\)<a name="ex1-batch-transform-api-high-level"></a>

The following code creates a transform job using the Amazon SageMaker high\-level Python library:

```
import boto3
import sagemaker
import json

input_key = 'kmeans_batch_example/input/valid-data.csv'
input_location = 's3://{}/{}'.format(bucket, input_key)
output_location = 's3://{}/kmeans_batch_example/output'.format(bucket)

### Convert the validation set NumPy array to a CSV file and upload it to Amazon S3
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
# Start a transform job
transformer.transform(input_location, content_type='text/csv', split_type='Line')
# Then wait until the transform job has completed
transformer.wait()

# Fetch validation result 
s3_client.download_file(bucket, 'kmeans_batch_example/output/valid-data.csv.out', 'valid-result')
with open('valid-result') as f:
    results = f.readlines()   
print("Sample transform result: {}".format(results[0]))
```

### Deploy a Model with Batch Transform \(Low\-level SDK for Python \(Boto 3\)\)<a name="ex1-batch-transform-api-low-level"></a>

To use batch transform, you need to create a transform job\.

#### <a name="ex1-batch-transform-api-low-level-create"></a>

With the Low\-level SDK for Python \(Boto 3\), you use the `create_transform_job` method to create a transform job\. With this method, you do the following:

1. Import the required libraries \(`boto3`\) and the following classes from the time library: `gtime` and `strftime` \.

1. Set the required variables: `job_name`, `bucket`, `prefix`, `sm`, and `model_name`\. The `sm` variable allows you to use the Amazon SageMaker API and is especially important\.

1. Write the request\. For information, see [CreateTransformJob](API_CreateTransformJob.md)\.

   To avoid generating errors and to improve readability, use the variables that you created in Step when creating the request\. 

1. Use the request with the `create_transform_job` method to start a batch transform job\. The following code shows how to use the `create_transform_job` operation\.

```
import boto3
import sagemaker
import json
from urllib.parse import urlparse
from time import gmtime, strftime

batch_job_name = 'Batch-Transform-' + strftime("%Y-%m-%d-%H-%M-%S", gmtime())
input_location = 's3://{}/kmeans_batch_example/input'.format(bucket)
output_location = 's3://{}/kmeans_batch_example/output'.format(bucket)

### Convert the validation set NumPy array to a CSV file and upload it to Amazon S3
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

### Wait until the job finishes
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

### Fetch the transform output
output_key = "{}/valid_data.csv.out".format(urlparse(output_location).path.lstrip('/'))
s3_client.download_file(bucket, output_key, 'valid-result')
with open('valid-result') as f:
    results = f.readlines()   
print("Sample transform result: {}".format(results[0]))
```

#### View the Details of a Transform Job \(Low\-level SDK for Python \(Boto 3\)\)<a name="ex1-batch-transform-api-low-level-describe"></a>

To see the details of a specific transform job, specify the name in a `DescribeTransformJjobs` requestand the corresponding `describe_transform_jobs` method: 

```
response = sm.describe_transform_job(TransformJobName=job_name)
```

#### List Transform Jobs \(Low\-level SDK for Python \(Boto 3\)\)<a name="ex1-batch-transform-api-low-level-list"></a>

To list previous transform jobs, specify the listing and sorting criteria  and use the `list_transform_jobs` method:

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
When using the `list_transform_jobs` method, you can specify any of the members in the request structure\. For more information, see [ListTransformJobs](API_ListTransformJobs.md)\.

**Next Step**  
[Step 2\.5: Validate the Model](ex1-test-model.md)