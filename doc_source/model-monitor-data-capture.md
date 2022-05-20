# Capture data<a name="model-monitor-data-capture"></a>

To log the inputs to your endpoint and the inference outputs from SageMaker Real\-time endpoints to Amazon S3, you can enable a feature called *Data Capture*\. It is commonly used to record information that can be used for training, debugging, and monitoring\. Amazon SageMaker Model Monitor automatically parses this captured data and compares metrics from this data with a baseline that you create for the model\. For more information about Model Monitor see [Monitor models for data and model quality, bias, and explainability](model-monitor.md)\.

**Note**  
To prevent impact to inference requests, Data Capture stops capturing requests at high levels of disk usage\. It is recommended you keep your disk utilization below 75% in order to ensure data capture continues capturing requests\.

To capture data, you must deploy a model using SageMaker hosting services\. This requires that you create a SageMaker model, define an endpoint configuration, and create an HTTPS endpoint\.

The steps required to enable data capture are similar regardless if you use the AWS SDK for Python \(Boto\) or the SageMaker Python SDK\. If you use the AWS SDK, define the [DataCaptureConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DataCaptureConfig.html) dictionary, along with required fields, within the [CreateEndpointConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpointConfig.html) method to enable data capture\. If you use the SageMaker Python SDK, import the [DataCaptureConfig](https://sagemaker.readthedocs.io/en/stable/api/inference/model_monitor.html#sagemaker.model_monitor.data_capture_config.DataCaptureConfig) Class and initialize an instance from this class\. Then, pass this object to the `DataCaptureConfig` parameter in the `sagemaker.model.Model.deploy()` method\.

To use the proceeding code snippets, replace the *italicized placeholder text* in the example code with your own information\.

## How to enable data capture<a name="model-monitor-data-capture-defing.title"></a>

Specify a data capture configuration\. You can capture the request payload, the response payload, or both with this configuration\. The proceeding code snippet demonstrates how to enable data capture using the AWS SDK for Python \(Boto\) and the SageMaker Python SDK\.

**Note**  
You do not need to use Model Monitor to capture request or response payloads\.

------
#### [ AWS SDK for Python \(Boto\) ]

Configure the data you want to capture with the [DataCaptureConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DataCaptureConfig.html) dictionary when you create an endpoint using the `CreateEndpointConfig` method\. Set `EnableCapture` to the boolean value True\. In addition, provide the following mandatory parameters:
+ `EndpointConfigName`: the name of your endpoint configuration\. You will use this name when you make a `CreateEndpoint` request\.
+ `ProductionVariants`: a list of models you want to host at this endpoint\. Define a dictionary data type for each model\.
+ `DataCaptureConfig`: dictionary data type where you specify an integer value that corresponds to the initial percentage of data to sample \(`InitialSamplingPercentage`\), the Amazon S3 URI where you want captured data to be stored, and a capture options \(`CaptureOptions`\) list\. Specify either `Input` or `Output` for `CaptureMode` within the `CaptureOptions` list\. 

You can optionally specify how SageMaker should encode captured data by passing key\-value pair arguments to the `CaptureContentTypeHeader` dictionary\.

```
# Create an endpoint config name.
endpoint_config_name = '<endpoint-config-name>'

# The name of the production variant.
variant_name = '<name-of-production-variant>'                   
  
# The name of the model that you want to host. 
# This is the name that you specified when creating the model.
model_name = '<The_name_of_your_model>'

instance_type = '<instance-type>'
#instance_type='ml.m5.xlarge' # Example    

# Number of instances to launch initially.
initial_instance_count = <integer>

# Sampling percentage. Choose an integer value between 0 and 100
initial_sampling_percentage = <integer>                                                                                                                                                                                                                        

# The S3 URI of where to store captured data in S3
s3_capture_upload_path = 's3://<bucket-name>/<data_capture_s3_key>'

# Specify either Input, Output, or both
capture_modes = [ "Input",  "Output" ] 
#capture_mode = [ "Input"] # Example - If you want to capture input only
                            
endpoint_config_response = sagemaker_client.create_endpoint_config(
    EndpointConfigName=endpoint_config_name, 
    # List of ProductionVariant objects, one for each model that you want to host at this endpoint.
    ProductionVariants=[
        {
            "VariantName": variant_name, 
            "ModelName": model_name, 
            "InstanceType": instance_type, # Specify the compute instance type.
            "InitialInstanceCount": initial_instance_count # Number of instances to launch initially.
        }
    ],
    DataCaptureConfig= {
        'EnableCapture': True, # Whether data should be captured or not.
        'InitialSamplingPercentage' : initial_sampling_percentage,
        'DestinationS3Uri': s3_capture_upload_path,
        'CaptureOptions': [{"CaptureMode" : capture_mode} for capture_mode in capture_modes] # Example - Use list comprehension to capture both Input and Output
    }
)
```

For more information about other endpoint configuration options, see the [CreateEndpointConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpointConfig.html) API in the [Amazon SageMaker Service API Reference Guide](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_Operations_Amazon_SageMaker_Service.html)\.

------
#### [ SageMaker Python SDK ]

Import the `DataCaptureConfig` Class from the [sagemaker\.model\_monitor](https://sagemaker.readthedocs.io/en/stable/api/inference/model_monitor.html) module\. Enable data capture by setting `EnableCapture` to the boolean value `True`\.

Optionally provide arguments for the following parameters:
+ `SamplingPercentage`: an integer value that corresponds to percentage of data to sample\. If you do not provide a sampling percentage, SageMaker will sample a default of 20 \(20%\) of your data\.
+ `DestinationS3Uri`: the Amazon S3 URI SageMaker will use to store captured data\. If you do not provide one, SageMaker will store captured data in `"s3://<default-session-bucket>/ model-monitor/data-capture"`\.

```
from sagemaker.model_monitor import DataCaptureConfig

# Set to True to enable data capture
enable_capture = True

# Optional - Sampling percentage. Choose an integer value between 0 and 100
sampling_percentage = <int> 
# sampling_percentage = 30 # Example 30%

# Optional - The S3 URI of where to store captured data in S3
s3_capture_upload_path = 's3://<bucket-name>/<data_capture_s3_key>'

# Specify either Input, Output or both. 
capture_modes = ['REQUEST','RESPONSE'] # In this example, we specify both
# capture_mode = ['REQUEST'] # Example - If you want to only capture input.

# Configuration object passed in when deploying Models to SM endpoints
data_capture_config = DataCaptureConfig(
    enable_capture = enable_capture, 
    sampling_percentage = sampling_percentage, # Optional
    destination_s3_uri = s3_capture_upload_path, # Optional
    capture_options = [{"CaptureMode": capture_mode} for capture_mode in capture_modes]
)
```

------

## Deploy your model<a name="model-monitor-data-capture-deploy"></a>

Deploy your model and create an HTTPS endpoint with `DataCapture` enabled\.

------
#### [ AWS SDK for Python \(Boto3\) ]

Provide the endpoint configuration to SageMaker\. The service launches the ML compute instances and deploys the model or models as specified in the configuration\.

Once you have your model and endpoint configuration, use the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html) API to create your endpoint\. The endpoint name must be unique within an AWS Region in your AWS account\. 

The following creates an endpoint using the endpoint configuration specified in the request\. Amazon SageMaker uses the endpoint to provision resources and deploy models\.

```
# The name of the endpoint. The name must be unique within an AWS Region in your AWS account.
endpoint_name = '<endpoint-name>' 

# The name of the endpoint configuration associated with this endpoint.
endpoint_config_name='<endpoint-config-name>'

create_endpoint_response = sagemaker_client.create_endpoint(
                                            EndpointName=endpoint_name, 
                                            EndpointConfigName=endpoint_config_name)
```

For more information, see the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html) API\.

------
#### [ SageMaker Python SDK ]

Define a name for your endpoint\. This step is optional\. If you do not provide one, SageMaker will create a unique name for you:

```
from datetime import datetime

endpoint_name = f"DEMO-{datetime.utcnow():%Y-%m-%d-%H%M}"
print("EndpointName =", endpoint_name)
```

Deploy your model to a real\-time, HTTPS endpoint with the Model object’s built\-in `deploy()` method\. Provide the name of the Amazon EC2 instance type to deploy this model to in the `instance_type` field along with the initial number of instances to run the endpoint on for the `initial_instance_count` field:

```
initial_instance_count=<integer>
# initial_instance_count=1 # Example

instance_type='<instance-type>'
# instance_type='ml.m4.xlarge' # Example

# Uncomment if you did not define this variable in the previous step
#data_capture_config = <name-of-data-capture-configuration>

model.deploy(
    initial_instance_count=initial_instance_count,
    instance_type=instance_type,
    endpoint_name=endpoint_name,
    data_capture_config=data_capture_config
)
```

------

## View Captured Data<a name="model-monitor-data-capture-view"></a>

Create a predictor object from the SageMaker Python SDK [Predictor](https://sagemaker.readthedocs.io/en/stable/api/inference/predictors.html) Class\. You will use the object returned by the `Predictor` Class to invoke your endpoint in a future step\. Provide the name of your endpoint \(defined earlier as `endpoint_name`\), along with serializer and deserializer objects for the serializer and deserializer, respectively\. For information about serializer types, see the [Serializers](https://sagemaker.readthedocs.io/en/stable/api/inference/serializers.html) Class in the [SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/index.html)\.

```
from sagemaker.predictor import Predictor
from sagemaker.serializers import <Serializer>
from sagemaker.deserializers import <Deserializers>

predictor = Predictor(endpoint_name=endpoint_name,
                      serializer = <Serializer_Class>,
                      deserializer = <Deserializer_Class>)

# Example
#from sagemaker.predictor import Predictor
#from sagemaker.serializers import CSVSerializer
#from sagemaker.deserializers import JSONDeserializer

#predictor = Predictor(endpoint_name=endpoint_name,
#                      serializer=CSVSerializer(),
#                      deserializer=JSONDeserializer())
```

In the proceeding code example scenario we invoke the endpoint with sample validation data that we have stored locally in a CSV file named `validation_with_predictions`\. Our sample validation set contains labels for each input\.

The first few lines of the with statement first opens the validation set CSV file, then splits each row within the file by the comma character `","`, and then stores the two returned objects into a label and input\_cols variables\. For each row, the input \(`input_cols`\) is passed to the predictor variable's \(`predictor`\) objects built\-in method `Predictor.predict()`\.

Suppose the model returns a probability\. Probabilities range between integer values of 0 and 1\.0\. If the probability returned by the model is greater than 80% \(0\.8\) we assign the prediction an integer value label of 1\. Otherwise, we assign the prediction an integer value label of 0\.

```
from time import sleep

validate_dataset = "validation_with_predictions.csv"

# Cut off threshold of 80%
cutoff = 0.8

limit = 200  # Need at least 200 samples to compute standard deviations
i = 0
with open(f"test_data/{validate_dataset}", "w") as validation_file:
    validation_file.write("probability,prediction,label\n")  # CSV header
    with open("test_data/validation.csv", "r") as f:
        for row in f:
            (label, input_cols) = row.split(",", 1)
            probability = float(predictor.predict(input_cols))
            prediction = "1" if probability > cutoff else "0"
            baseline_file.write(f"{probability},{prediction},{label}\n")
            i += 1
            if i > limit:
                break
            print(".", end="", flush=True)
            sleep(0.5)
print()
print("Done!")
```

Because you enabled the data capture in the previous steps, the request and response payload, along with some additional meta data, is saved in the Amazon S3 location that you specified in `DataCaptureConfig`\. The delivery of capture data to Amazon S3 can require a couple of minutes\.

View captured data by listing the data capture files stored in Amazon S3\. The format of the Amazon S3 path is: `s3:///{endpoint-name}/{variant-name}/yyyy/mm/dd/hh/filename.jsonl`\.

Expect to see different files from different time periods, organized based on the hour when the invocation occurred\. Run the following to print out the contents of a single capture file:

```
print("\n".join(capture_file[-3:-1]))
```

This will return a SageMaker specific JSON\-line formatted file\. The following is a response sample taken from a real\-time endpoint that we invoked using `csv/text` data:

```
{"captureData":{"endpointInput":{"observedContentType":"text/csv","mode":"INPUT",
"data":"69,0,153.7,109,194.0,105,256.1,114,14.1,6,1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1,0,1,0,1,0\n",
"encoding":"CSV"},"endpointOutput":{"observedContentType":"text/csv; charset=utf-8","mode":"OUTPUT","data":"0.0254181120544672","encoding":"CSV"}},
"eventMetadata":{"eventId":"aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee","inferenceTime":"2022-02-14T17:25:49Z"},"eventVersion":"0"}
{"captureData":{"endpointInput":{"observedContentType":"text/csv","mode":"INPUT",
"data":"94,23,197.1,125,214.5,136,282.2,103,9.5,5,4,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1,0,0,0,1,0,1,0,1\n",
"encoding":"CSV"},"endpointOutput":{"observedContentType":"text/csv; charset=utf-8","mode":"OUTPUT","data":"0.07675473392009735","encoding":"CSV"}},
"eventMetadata":{"eventId":"aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee","inferenceTime":"2022-02-14T17:25:49Z"},"eventVersion":"0"}
```

In the proceeding example, the `capture_file` object is a list type\. Index the first element of the list to view a single inference request\.

```
# The capture_file object is a list. Index the first element to view a single inference request  
print(json.dumps(json.loads(capture_file[0]), indent=2))
```

This will return a response similar to the following\. The values returned will differ based on your endpoint configuration, SageMaker model, and captured data:

```
{
  "captureData": {
    "endpointInput": {
      "observedContentType": "text/csv", # data MIME type
      "mode": "INPUT",
      "data": "50,0,188.9,94,203.9,104,151.8,124,11.6,8,3,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1,0,0,0,0,0,0,0,0,0,1,1,0,1,0\n",
      "encoding": "CSV"
    },
    "endpointOutput": {
      "observedContentType": "text/csv; charset=character-encoding",
      "mode": "OUTPUT",
      "data": "0.023190177977085114",
      "encoding": "CSV"
    }
  },
  "eventMetadata": {
    "eventId": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee",
    "inferenceTime": "2022-02-14T17:25:06Z"
  },
  "eventVersion": "0"
}
```