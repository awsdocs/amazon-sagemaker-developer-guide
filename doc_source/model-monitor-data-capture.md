# Capture Data<a name="model-monitor-data-capture"></a>

After you have created an endpoint, configure the permissions and paths to Amazon S3 locations for storing data, report, and processing code\.

```
import boto3
import re
import json
from sagemaker import get_execution_role, session

region= boto3.Session().region_name

role = get_execution_role()
print("RoleArn: {}".format(role))

# You can use a different bucket, but make sure the role you chose for this notebook
# has s3:PutObject permissions. This is the bucket into which the data is captured
bucket =  session.Session(boto3.Session()).default_bucket()
print("Demo Bucket: {}".format(bucket))
prefix = 'sagemaker/DEMO-ModelMonitor'

data_capture_prefix = '{}/datacapture'.format(prefix)
s3_capture_upload_path = 's3://{}/{}'.format(bucket, data_capture_prefix)
reports_prefix = '{}/reports'.format(prefix)
s3_report_path = 's3://{}/{}'.format(bucket,reports_prefix)
code_prefix = '{}/code'.format(prefix)
s3_code_preprocessor_uri = 's3://{}/{}/{}'.format(bucket,code_prefix, 'preprocessor.py')
s3_code_postprocessor_uri = 's3://{}/{}/{}'.format(bucket,code_prefix, 'postprocessor.py')

print("Capture path: {}".format(s3_capture_upload_path))
print("Report path: {}".format(s3_report_path))
print("Preproc Code path: {}".format(s3_code_preprocessor_uri))
print("Postproc Code path: {}".format(s3_code_postprocessor_uri))
```

**Upload the pre\-trained model to Amazon S3**: 

```
model_file = open("model/your-prediction-model.tar.gz", 'rb')
s3_key = os.path.join(prefix, 'your-prediction-model.tar.gz')
boto3.Session().resource('s3').Bucket(bucket).Object(s3_key).upload_fileobj(model_file)
```

**Enable data capture**: You specify the capture option called [ `DataCaptureConfig`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DataCaptureConfig.html)\. You can capture the request payload, the response payload, or both with this configuration\. The capture configuration applies to all variants\.

```
from sagemaker.model_monitor import DataCaptureConfig

endpoint_name = 'your-pred-model-monitor-' + strftime("%Y-%m-%d-%H-%M-%S", gmtime())
print("EndpointName={}".format(endpoint_name))

data_capture_config=DataCaptureConfig(
                        enable_capture = True,
                        sampling_percentage=100,
                        destination_s3_uri=s3_capture_upload_path)

predictor = model.deploy(initial_instance_count=1,
                instance_type='ml.m4.xlarge',
                endpoint_name=endpoint_name
                data_capture_config=data_capture_config)
```

**Invoke the deployed model**: You can now send data to this endpoint to get inferences in real time\. Because you enabled the data capture in the previous steps, the request and response payload, along with some additional metadata, is saved in the Amazon S3 location that you specified in [ `DataCaptureConfig`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DataCaptureConfig.html)\.

```
from sagemaker.predictor import RealTimePredictor
import time

predictor = RealTimePredictor(endpoint=endpoint_name,content_type='text/csv')

# get a subset of test data for a quick test
!head -120 test_data/test-dataset-input-cols.csv > test_data/test_sample.csv
print("Sending test traffic to the endpoint {}. \nPlease wait...".format(endpoint_name))

with open('test_data/test_sample.csv', 'r') as f:
    for row in f:
        payload = row.rstrip('\n')
        response = predictor.predict(data=payload)
        time.sleep(0.5)
        
print("Done!")
```

**View captured data**: List the data capture files stored in Amazon S3\. Expect to see different files from different time periods, organized based on the hour when the invocation occurred\. 

```
s3_client = boto3.Session().client('s3')
current_endpoint_capture_prefix = '{}/{}'.format(data_capture_prefix, endpoint_name)
result = s3_client.list_objects(Bucket=bucket, Prefix=current_endpoint_capture_prefix)
capture_files = [capture_file.get("Key") for capture_file in result.get('Contents')]
print("Found Capture Files:")
print("\n ".join(capture_files))
```

The format of the Amazon S3 path is:

```
s3://{destination-bucket-prefix}/{endpoint-name}/{variant-name}/yyyy/mm/dd/hh/filename.jsonl
```