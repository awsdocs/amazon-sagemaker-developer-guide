# Preprocessing and Postprocessing<a name="model-monitor-pre-and-post-processing"></a>

In addition to using the built\-in mechanisms, you can extend the code with the preprocessing and postprocessing scripts\.

**Topics**
+ [Postprocessing Script](#model-monitor-post-processing-script)
+ [Preprocessing Script](#model-monitor-pre-processing-script)

## Postprocessing Script<a name="model-monitor-post-processing-script"></a>

You can extend the code with the post processing script by following this contract\. 

```
def postprocess_handler():
    print("Hello from post-proc script!")
```

Specify it as a path in Amazon Simple Storage Service \(Amazon S3\) in the `CreateMonitoringSchedule` request\.

```
.MonitoringAppSpecification.PostAnalyticsProcessorSourceUri.
```

## Preprocessing Script<a name="model-monitor-pre-processing-script"></a>

The Amazon SageMaker Model Monitor container works only with tabular or flattened json structures\. We provide a per\-record preprocessor for some small changes required to transform the dataset\. For example, if your output is an array \[1\.0, 2\.1\], you need to convert this into a flattened JSON, like \{“prediction0”: 1\.0, “prediction1” : 2\.1"\}\. A sample implementation might look like the following\.

```
def preprocess_handler(inference_record):
    
    input_data = inference_record.endpoint_input.data
    output_data = inference_record.endpoint_output.data

    input_data['feature0'] = random.randint(1, 3)
    input_data['feature1'] = random.uniform(0, 1.6)
    input_data['feature2'] = random.uniform(0, 1.6)

    output_data['prediction0'] = random.uniform(1, 30)
    
    return {**input_data, **output_data}
```

Specify it as a path in Amazon S3 in the `CreateMonitoringSchedule` request:

```
.MonitoringAppSpecification.RecordPreprocessorSourceUri.
```

The structure of the inference\_record is defined as follows\.

```
KEY_EVENT_METADATA = "eventMetadata"
KEY_EVENT_METADATA_EVENT_ID = "eventId"
KEY_EVENT_METADATA_EVENT_TIME = "inferenceTime"
KEY_EVENT_METADATA_CUSTOM_ATTR = "customAttributes"

KEY_EVENTDATA = "captureData"
KEY_EVENTDATA_INPUT = "endpointInput"
KEY_EVENTDATA_OUTPUT = "endpointOutput"
KEY_EVENTDATA_ENCODING = "encoding"
KEY_EVENTDATA_DATA = "data"
KEY_EVENTDATA_OBSERVED_CONTENT_TYPE = "observedContentType"
KEY_EVENTDATA_MODE = "mode"

KEY_EVENT_VERSION = "eventVersion"

"""
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
"""

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
        self.custom_attribute = event_metadata_dict.get(KEY_EVENTDATA_OBSERVED_CONTENT_TYPE, None)
                                   

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
        self.event_version = None
        self.event_dict = event_dict
        self._event_dict_postprocessed = False
        if KEY_EVENT_METADATA in event_dict:
            self.event_metadata = EventMetadata(event_dict[KEY_EVENT_METADATA])
        if KEY_EVENTDATA in event_dict:
            if KEY_EVENTDATA_INPUT in event_dict[KEY_EVENTDATA]:
                self.endpoint_input = EventData(event_dict[KEY_EVENTDATA][KEY_EVENTDATA_INPUT])
            if KEY_EVENTDATA_OUTPUT in event_dict[KEY_EVENTDATA]:
                self.endpoint_output = EventData(event_dict[KEY_EVENTDATA][KEY_EVENTDATA_OUTPUT])
        if KEY_EVENT_VERSION in event_dict:
            self.event_version = event_dict[KEY_EVENT_VERSION]

    def as_dict(self):
        if self._event_dict_postprocessed is True:
            return self.event_dict
        if KEY_EVENTDATA in self.event_dict:
            if KEY_EVENTDATA_INPUT in self.event_dict[KEY_EVENTDATA]:
                self.event_dict[KEY_EVENTDATA][KEY_EVENTDATA_INPUT] = self.endpoint_input.as_dict()
            if KEY_EVENTDATA_OUTPUT in self.event_dict[KEY_EVENTDATA]:
                self.event_dict[KEY_EVENTDATA][
                    KEY_EVENTDATA_OUTPUT
                ] = self.endpoint_output.as_dict()
        self._event_dict_postprocessed = True
        return self.event_dict
```