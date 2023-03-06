# Preprocessing and Postprocessing<a name="model-monitor-pre-and-post-processing"></a>

You can use custom preprocessing and postprocessing Python scripts to transform the input to your model monitor or extend the code after a successful monitoring run\. Upload these scripts to Amazon S3 and reference them when creating your model monitor\.

The following example shows how you can customize monitoring schedules with preprocessing and postprocessing scripts\. Replace *user placeholder text* with your own information\.

```
import boto3, os
from sagemaker import get_execution_role, Session
from sagemaker.model_monitor import CronExpressionGenerator, DefaultModelMonitor

# Upload pre and postprocessor scripts
session = Session()
bucket = boto3.Session().resource("s3").Bucket(session.default_bucket())
prefix = "demo-sagemaker-model-monitor"
pre_processor_script = bucket.Object(os.path.join(prefix, "preprocessor.py")).upload_file("preprocessor.py")
post_processor_script = bucket.Object(os.path.join(prefix, "postprocessor.py")).upload_file("postprocessor.py")

# Get execution role
role = get_execution_role() # can be an empty string

# Instance type
instance_type = "instance-type"
# instance_type = "ml.m5.xlarge" # Example

# Create a monitoring schedule with pre and postprocessing
my_default_monitor = DefaultModelMonitor(
    role=role,
    instance_count=1,
    instance_type=instance_type,
    volume_size_in_gb=20,
    max_runtime_in_seconds=3600,
)

s3_report_path = "s3://{}/{}".format(bucket, "reports")
monitor_schedule_name = "monitor-schedule-name"
endpoint_name = "endpoint-name"
my_default_monitor.create_monitoring_schedule(
    post_analytics_processor_script=post_processor_script,
    record_preprocessor_script=pre_processor_script,
    monitor_schedule_name=monitor_schedule_name,
    # use endpoint_input for real-time endpoint
    endpoint_input=endpoint_name,
    # or use batch_transform_input for batch transform jobs
    # batch_transform_input=batch_transform_name,
    output_s3_uri=s3_report_path,
    statistics=my_default_monitor.baseline_statistics(),
    constraints=my_default_monitor.suggested_constraints(),
    schedule_cron_expression=CronExpressionGenerator.hourly(),
    enable_cloudwatch_metrics=True,
)
```

**Topics**
+ [Preprocessing Script](#model-monitor-pre-processing-script)
+ [Custom Sampling](#model-monitor-pre-processing-custom-sampling)
+ [Postprocessing Script](#model-monitor-post-processing-script)

## Preprocessing Script<a name="model-monitor-pre-processing-script"></a>

Use preprocessing scripts when you need to transform the inputs to your model monitor\.

For example, suppose the output of your model is an array `[1.0, 2.1]`\. The Amazon SageMaker Model Monitor container only works with tabular or flattened JSON structures, like `{“prediction0”: 1.0, “prediction1” : 2.1}`\. You could use a preprocessing script like the following to transform the array into the correct JSON structure\.

```
def preprocess_handler(inference_record):
    input_data = inference_record.endpoint_input.data
    output_data = inference_record.endpoint_output.data.rstrip("\n")
    data = output_data + "," + input_data
    return { str(i).zfill(20) : d for i, d in enumerate(data.split(",")) }
```

In another example, suppose your model has optional features and you use `-1` to denote that the optional feature has a missing value\. If you have a data quality monitor, you may want to remove the `-1` from the input value array so that it isn't included in the monitor's metric calculations\. You could use a script like the following to remove those values\.

```
def preprocess_handler(inference_record):
    input_data = inference_record.endpoint_input.data
    return {i : None if x == -1 else x for i, x in enumerate(input_data.split(","))}
```

Your preprocessing script receives an `inference_record` as its only input\. The following code snippet shows an example of an `inference_record`\.

```
{
  "captureData": {
    "endpointInput": {
      "observedContentType": "text/csv",
      "mode": "INPUT",
      "data": "132,25,113.2,96,269.9,107,,0,0,0,0,0,0,1,0,1,0,0,1",
      "encoding": "CSV"
    },
    "endpointOutput": {
      "observedContentType": "text/csv; charset=utf-8",
      "mode": "OUTPUT",
      "data": "0.01076381653547287",
      "encoding": "CSV"
    }
  },
  "eventMetadata": {
    "eventId": "feca1ab1-8025-47e3-8f6a-99e3fdd7b8d9",
    "inferenceTime": "2019-11-20T23:33:12Z"
  },
  "eventVersion": "0"
}
```

The following code snippet shows the full class structure for an `inference_record`\.

```
KEY_EVENT_METADATA = "eventMetadata"
KEY_EVENT_METADATA_EVENT_ID = "eventId"
KEY_EVENT_METADATA_EVENT_TIME = "inferenceTime"
KEY_EVENT_METADATA_CUSTOM_ATTR = "customAttributes"

KEY_EVENTDATA_ENCODING = "encoding"
KEY_EVENTDATA_DATA = "data"

KEY_GROUND_TRUTH_DATA = "groundTruthData"

KEY_EVENTDATA = "captureData"
KEY_EVENTDATA_ENDPOINT_INPUT = "endpointInput"
KEY_EVENTDATA_ENDPOINT_OUTPUT = "endpointOutput"

KEY_EVENTDATA_BATCH_OUTPUT = "batchTransformOutput"
KEY_EVENTDATA_OBSERVED_CONTENT_TYPE = "observedContentType"
KEY_EVENTDATA_MODE = "mode"

KEY_EVENT_VERSION = "eventVersion"

class EventConfig:
    def __init__(self, endpoint, variant, start_time, end_time):
        self.endpoint = endpoint
        self.variant = variant
        self.start_time = start_time
        self.end_time = end_time


class EventMetadata:
    def __init__(self, event_metadata_dict):
        self.event_id = event_metadata_dict.get(KEY_EVENT_METADATA_EVENT_ID, None)
        self.event_time = event_metadata_dict.get(KEY_EVENT_METADATA_EVENT_TIME, None)
        self.custom_attribute = event_metadata_dict.get(KEY_EVENT_METADATA_CUSTOM_ATTR, None)


class EventData:
    def __init__(self, data_dict):
        self.encoding = data_dict.get(KEY_EVENTDATA_ENCODING, None)
        self.data = data_dict.get(KEY_EVENTDATA_DATA, None)
        self.observedContentType = data_dict.get(KEY_EVENTDATA_OBSERVED_CONTENT_TYPE, None)
        self.mode = data_dict.get(KEY_EVENTDATA_MODE, None)

    def as_dict(self):
        ret = {
            KEY_EVENTDATA_ENCODING: self.encoding,
            KEY_EVENTDATA_DATA: self.data,
            KEY_EVENTDATA_OBSERVED_CONTENT_TYPE: self.observedContentType,
        }
        return ret


class CapturedData:
    def __init__(self, event_dict):
        self.event_metadata = None
        self.endpoint_input = None
        self.endpoint_output = None
        self.batch_transform_output = None
        self.ground_truth = None
        self.event_version = None
        self.event_dict = event_dict
        self._event_dict_postprocessed = False
        
        if KEY_EVENT_METADATA in event_dict:
            self.event_metadata = EventMetadata(event_dict[KEY_EVENT_METADATA])
        if KEY_EVENTDATA in event_dict:
            if KEY_EVENTDATA_ENDPOINT_INPUT in event_dict[KEY_EVENTDATA]:
                self.endpoint_input = EventData(event_dict[KEY_EVENTDATA][KEY_EVENTDATA_ENDPOINT_INPUT])
            if KEY_EVENTDATA_ENDPOINT_OUTPUT in event_dict[KEY_EVENTDATA]:
                self.endpoint_output = EventData(event_dict[KEY_EVENTDATA][KEY_EVENTDATA_ENDPOINT_OUTPUT])
            if KEY_EVENTDATA_BATCH_OUTPUT in event_dict[KEY_EVENTDATA]:
                self.batch_transform_output = EventData(event_dict[KEY_EVENTDATA][KEY_EVENTDATA_BATCH_OUTPUT])

        if KEY_GROUND_TRUTH_DATA in event_dict:
            self.ground_truth = EventData(event_dict[KEY_GROUND_TRUTH_DATA])
        if KEY_EVENT_VERSION in event_dict:
            self.event_version = event_dict[KEY_EVENT_VERSION]

    def as_dict(self):
        if self._event_dict_postprocessed is True:
            return self.event_dict
        if KEY_EVENTDATA in self.event_dict:
            if KEY_EVENTDATA_ENDPOINT_INPUT in self.event_dict[KEY_EVENTDATA]:
                self.event_dict[KEY_EVENTDATA][KEY_EVENTDATA_ENDPOINT_INPUT] = self.endpoint_input.as_dict()
            if KEY_EVENTDATA_ENDPOINT_OUTPUT in self.event_dict[KEY_EVENTDATA]:
                self.event_dict[KEY_EVENTDATA][
                    KEY_EVENTDATA_ENDPOINT_OUTPUT
                ] = self.endpoint_output.as_dict()
            if KEY_EVENTDATA_BATCH_OUTPUT in self.event_dict[KEY_EVENTDATA]:
                self.event_dict[KEY_EVENTDATA][KEY_EVENTDATA_BATCH_OUTPUT] = self.batch_transform_output.as_dict()
        
        self._event_dict_postprocessed = True
        return self.event_dict

    def __str__(self):
        return str(self.as_dict())
```

## Custom Sampling<a name="model-monitor-pre-processing-custom-sampling"></a>

You can also apply a custom sampling strategy in your preprocessing script\. To do this, configure Model Monitor's first\-party, pre\-built container to ignore a percentage of the records according to your specified sampling rate\. In the following example, the handler samples 10 percent of the records by returning the record in 10 percent of handler calls and an empty list otherwise\.

```
import random

def preprocess_handler(inference_record):
    # we set up a sampling rate of 0.1
    if random.random() > 0.1:
        # return an empty list
        return []
    input_data = inference_record.endpoint_input.data
    return {i : None if x == -1 else x for i, x in enumerate(input_data.split(","))}
```

### Custom logging for preprocessing script<a name="model-monitor-pre-processing-custom-logging"></a>

 If your preprocessing script returns an error, check the exception messages logged to CloudWatch to debug\. You can access the logger on CloudWatch through the `preprocess_handler` interface\. You can log any information you need from your script to CloudWatch\. This can be useful when debug your preprocessing script\. The following example shows how you can use the `preprocess_handler` interface to log to CloudWatch 

```
def preprocess_handler(inference_record, logger):
    logger.info(f"I'm a processing record: {inference_record}")
    logger.debug(f"I'm debugging a processing record: {inference_record}")
    logger.warning(f"I'm processing record with missing value: {inference_record}")
    logger.error(f"I'm a processing record with bad value: {inference_record}")
    return inference_record
```

## Postprocessing Script<a name="model-monitor-post-processing-script"></a>

Use a postprocessing script when you want to extend the code following a successful monitoring run\.

```
def postprocess_handler():
    print("Hello from post-proc script!")
```