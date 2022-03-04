# Capture data<a name="model-monitor-data-capture"></a>

To use Model Monitor, configure your real\-time inference endpoint to capture data from requests and responses and store the captured data in Amazon S3\. Model monitor compares metrics from this data with a baseline that you create for the model\. For information about creating a baseline, see [Create a Baseline](model-monitor-create-baseline.md)\.

**Note**  
To prevent impact to inference requests, Data Capture stops capturing requests at high levels of disk usage\. It is recommended you keep your disk utilization below 75% in order to ensure data capture continues capturing requests\.

**To set up data capture**

1. First, configure the Amazon S3 buckets Model Monitor uses to store the captured data\.

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

1. Next, upload the pre\-trained model to Amazon S3\.

   ```
   model_file = open("model/your-prediction-model.tar.gz", 'rb')
   s3_key = os.path.join(prefix, 'your-prediction-model.tar.gz')
   boto3.Session().resource('s3').Bucket(bucket).Object(s3_key).upload_fileobj(model_file)
   ```

1. Configure the data you want to capture by configuring the data you want to capture in a [ `DataCaptureConfig`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DataCaptureConfig.html) structure\. You can capture the request payload, the response payload, or both with this configuration\.

   ```
   from sagemaker.model_monitor import DataCaptureConfig
   
   endpoint_name = 'your-pred-model-monitor-' + strftime("%Y-%m-%d-%H-%M-%S", gmtime())
   print("EndpointName={}".format(endpoint_name))
   
   data_capture_config=DataCaptureConfig(
                           enable_capture = True,
                           sampling_percentage=100,
                           destination_s3_uri=s3_capture_upload_path)
   ```

1. Enable data capture by passing the configuration you created in the previous step when you deploy the model\.

   ```
   predictor = model.deploy(initial_instance_count=1,
                   instance_type='ml.m4.xlarge',
                   endpoint_name=endpoint_name
                   data_capture_config=data_capture_config)
   ```

1. Invoke the endpoint to send data to the endpoint to get inferences in real time\. Because you enabled the data capture in the previous steps, the request and response payload, along with some additional metadata, is saved in the Amazon S3 location that you specified in [ `DataCaptureConfig`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DataCaptureConfig.html)\.

   ```
   from sagemaker.predictor import Predictor
   from sagemaker.serializers import CSVSerializer
   from sagemaker.deserializers import JSONDeserializer                    
   import time
   
   predictor = Predictor(endpoint=endpoint_name, 
                         serializer=CSVSerializer(),
                         deserializer=JSONDeserializer())
   
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

1. The delivery of captured data to Amazon S3 can require a couple of minutes\. You can optionally run the following to check if data has been captured:

   ```
   from sagemaker.s3 import S3Downloader
   print("Waiting for captures to show up", end="")
   for _ in range(120):
       capture_files = sorted(S3Downloader.list(f"{s3_capture_upload_path}/{endpoint_name}"))
       if capture_files:
           capture_file = S3Downloader.read_file(capture_files[-1]).split("\n")
           capture_record = json.loads(capture_file[0])
           if "inferenceId" in capture_record["eventMetadata"]:
               break
       print(".", end="", flush=True)
       sleep(1)
   print()
   print("Found Capture Files:")
   print("\n ".join(capture_files[-3:]))
   ```