# Request Inferences from a Deployed Service<a name="neo-requests"></a>

If you have followed instructions in [Deploy a Model Compiled with Neo with Hosting Services](neo-deployment-hosting-services.md), you should have a Amazon SageMaker endpoint set up and running\. You can now submit inference requests using Boto3 client\. Here is an example of sending an image for inference:

```
import boto3
import json
 
endpoint = '<insert name of your endpoint here>'
 
runtime = boto3.Session().client('sagemaker-runtime')
 
# Read image into memory
with open(image, 'rb') as f:
    payload = f.read()
# Send image via InvokeEndpoint API
response = runtime.invoke_endpoint(EndpointName=endpoint, ContentType='application/x-image', Body=payload)
# Unpack response
result = json.loads(response['Body'].read().decode())
```

For XGBoost application, you should submit a CSV text instead:

```
import boto3
import json
 
endpoint = '<insert your endpoint here>'
 
runtime = boto3.Session().client('sagemaker-runtime')
 
csv_text = '1,-1.0,1.0,1.5,2.6'
# Send CSV text via InvokeEndpoint API
response = runtime.invoke_endpoint(EndpointName=endpoint, ContentType='text/csv', Body=csv_text)
# Unpack response
result = json.loads(response['Body'].read().decode())
```

Note that BYOM allows for a custom content type\. For more information, see [ `runtime_InvokeEndpoint`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_runtime_InvokeEndpoint.html)\.