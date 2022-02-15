# Request Inferences from a Deployed Service \(Boto3\)<a name="neo-requests-boto3"></a>

 You can submit inference requests using SageMaker SDK for Python \(Boto3\) client and [https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker-runtime.html#SageMakerRuntime.Client.invoke_endpoint](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker-runtime.html#SageMakerRuntime.Client.invoke_endpoint) API once you have an SageMaker endpoint `InService`\. The following code example shows how to send an image for inference: 

------
#### [ PyTorch and MXNet ]

```
import boto3

import json
 
endpoint = 'insert name of your endpoint here'
 
runtime = boto3.Session().client('sagemaker-runtime')
 
# Read image into memory
with open(image, 'rb') as f:
    payload = f.read()
# Send image via InvokeEndpoint API
response = runtime.invoke_endpoint(EndpointName=endpoint, ContentType='application/x-image', Body=payload)

# Unpack response
result = json.loads(response['Body'].read().decode())
```

------
#### [ TensorFlow ]

For TensorFlow submit an input with `application/json` for the content type\. 

```
from PIL import Image
import numpy as np
import json
import boto3

client = boto3.client('sagemaker-runtime') 
input_file = 'path/to/image'
image = Image.open(input_file)
batch_size = 1
image = np.asarray(image.resize((224, 224)))
image = image / 128 - 1
image = np.concatenate([image[np.newaxis, :, :]] * batch_size)
body = json.dumps({"instances": image.tolist()})
ioc_predictor_endpoint_name = 'insert name of your endpoint here'
content_type = 'application/json'   
ioc_response = client.invoke_endpoint(
    EndpointName=ioc_predictor_endpoint_name,
    Body=body,
    ContentType=content_type
 )
```

------
#### [ XGBoost ]

 For an XGBoost application, you should submit a CSV text instead: 

```
import boto3
import json
 
endpoint = 'insert your endpoint name here'
 
runtime = boto3.Session().client('sagemaker-runtime')
 
csv_text = '1,-1.0,1.0,1.5,2.6'
# Send CSV text via InvokeEndpoint API
response = runtime.invoke_endpoint(EndpointName=endpoint, ContentType='text/csv', Body=csv_text)
# Unpack response
result = json.loads(response['Body'].read().decode())
```

------

 Note that BYOM allows for a custom content type\. For more information, see [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_runtime_InvokeEndpoint.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_runtime_InvokeEndpoint.html)\. 