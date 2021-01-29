# Request Inferences from a Deployed Service \(Amazon SageMaker SDK\)<a name="neo-requests-sdk"></a>

Use the following the code examples to request inferences from your deployed service based on the framework you used to train your model\. The code examples for the different frameworks are similar\. The main difference is that TensorFlow requires `application/json` as the content type\. 

 

## PyTorch and MXNet<a name="neo-requests-sdk-py-mxnet"></a>

 If you are using **PyTorch v1\.4 or later** or **MXNet 1\.7\.0** or later and you have an Amazon SageMaker endpoint `InService`, you can make inference requests using the `predictor` package of the SageMaker SDK for Python\. 

**Note**  
The API varies based on the SageMaker SDK for Python version:  
For version 1\.x, use the [ `RealTimePredictor`](https://sagemaker.readthedocs.io/en/v1.72.0/api/inference/predictors.html#sagemaker.predictor.RealTimePredictor) and [ `Predict`](https://sagemaker.readthedocs.io/en/v1.72.0/api/inference/predictors.html#sagemaker.predictor.RealTimePredictor.predict) API\.
For version 2\.x, use the [ `Predictor`](https://sagemaker.readthedocs.io/en/stable/api/inference/predictors.html#sagemaker.predictor.Predictor) and the [https://sagemaker.readthedocs.io/en/stable/api/inference/predictors.html#sagemaker.predictor.Predictor.predict](https://sagemaker.readthedocs.io/en/stable/api/inference/predictors.html#sagemaker.predictor.Predictor.predict) API\.

The following code example shows how to use these APIs to send an image for inference: 

------
#### [ SageMaker Python SDK v1\.x ]

```
from sagemaker.predictor import RealTimePredictor

endpoint = 'insert name of your endpoint here'

# Read image into memory
payload = None
with open("image.jpg", 'rb') as f:
    payload = f.read()

predictor = RealTimePredictor(endpoint=endpoint, content_type='application/x-image')
inference_response = predictor.predict(data=payload)
print (inference_response)
```

------
#### [ SageMaker Python SDK v2\.x ]

```
from sagemaker.predictor import Predictor

endpoint = 'insert name of your endpoint here'

# Read image into memory
payload = None
with open("image.jpg", 'rb') as f:
    payload = f.read()
    
predictor = Predictor(endpoint)
inference_response = predictor.predict(data=payload)
print (inference_response)
```

------

## TensorFlow<a name="neo-requests-sdk-py-tf"></a>

The following code example shows how to use the SageMaker Python SDK API to send an image for inference: 

```
from sagemaker.predictor import Predictor
from PIL import Image
import numpy as np
import json

endpoint = 'insert the name of your endpoint here'

# Read image into memory
image = Image.open(input_file)
batch_size = 1
image = np.asarray(image.resize((224, 224)))
image = image / 128 - 1
image = np.concatenate([image[np.newaxis, :, :]] * batch_size)
body = json.dumps({"instances": image.tolist()})
    
predictor = Predictor(endpoint)
inference_response = predictor.predict(data=body)
print(inference_response)
```