# Adapting Your Own Inference Container<a name="adapt-inference-container"></a>

If none of the Amazon SageMaker prebuilt inference containers suffice for your situation, and you want to use your own Docker container, use the [SageMaker Inference Toolkit](https://github.com/aws/sagemaker-inference-toolkit) to adapt your container to work with SageMaker hosting\. To adapt your container to work with SageMaker hosting, create the inference code in one or more Python script files and a Dockerfile that imports the inference toolkit\.

The inference code includes an inference handler, a handler service, and an entrypoint\. In this example, they are stored as three separate Python files\. All three of these Python files must be in the same directory as your Dockerfile\.

For an example Jupyter notebook that shows a complete example of extending a container by using the SageMaker inference toolkit, see [Amazon SageMaker Multi\-Model Endpoints using your own algorithm container](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/advanced_functionality/multi_model_bring_your_own/multi_model_endpoint_bring_your_own.ipynb)\.

## Step 1: Create an Inference Handler<a name="byoc-inference-handler"></a>

The SageMaker inference toolkit is built on the multi\-model server \(MMS\)\. MMS expects a Python script that implements functions to load the model, pre\-process input data, get predictions from the model, and process the output data in a model handler\.

### The model\_fn Function<a name="byoc-inference-handler-modelfn"></a>

The `model_fn` function is responsible for loading your model\. It takes a `model_dir` argument that specifies where the model is stored\. How you load your model depends on the framework you are using\. There is no default implementation for the `model_fn` function\. You must implement it yourself\. The following simple example shows an implementation of a `model_fn` function that loads a PyTorch model:

```
def model_fn(self, model_dir):
    import torch
    
    logger.info('model_fn')
    device = "cuda" if torch.cuda.is_available() else "cpu"
    with open(os.path.join(model_dir, 'model.pth'), 'rb') as f:
        model = torch.jit.load(f)
    return model.to(device)
```

### The input\_fn Function<a name="byoc-inference-handler-inputfn"></a>

The `input_fn` function is responsible for deserializing your input data so that it can be passed to your model\. It takes input data and content type as parameters, and returns deserialized data\. The SageMaker inference toolkit provides a default implementation that deserializes the following content types:
+ JSON
+ CSV
+ Numpy array
+ NPZ

If your model requires a different content type, or you want to preprocess your input data before sending it to the model, you must implement the `input_fn` function\. The following example shows a simple implementation of the `input_fn` function\.

```
from sagemaker_inference import content_types, decoder
def input_fn(self, input_data, content_type):
        """A default input_fn that can handle JSON, CSV and NPZ formats.
         
        Args:
            input_data: the request payload serialized in the content_type format
            content_type: the request content_type

        Returns: input_data deserialized into torch.FloatTensor or torch.cuda.FloatTensor depending if cuda is available.
        """
        return decoder.decode(input_data, content_type)
```

### The predict\_fn Function<a name="byoc-inference-handler-predictfn"></a>

The `predict_fn` function is responsible for getting predictions from the model\. It takes the model and the data returned from `input_fn` as parameters, and returns the prediction\. There is no default implementation for the `predict_fn`\. You must implement it yourself\. The following is a simple implementation of the `predict_fn` function for a PyTorch model\.

```
def predict_fn(self, data, model):
        """A default predict_fn for PyTorch. Calls a model on data deserialized in input_fn.
        Runs prediction on GPU if cuda is available.

        Args:
            data: input data (torch.Tensor) for prediction deserialized by input_fn
            model: PyTorch model loaded in memory by model_fn

        Returns: a prediction
        """
        return model(input_data)
```

### The output\_fn Function<a name="byoc-inference-handler-outputfn"></a>

The `output_fn` function is responsible for serializing the data that the `predict_fn` function returns as a prediction\. The SageMaker inference toolkit implements a default `output_fn` function that serializes Numpy arrays, JSON, and CSV\. If your model outputs any other content type, or you want to perform other post\-processing of your data before sending it to the user, you must implement your own `output_fn` function\. The following shows a simple `output_fn` function for a PyTorch model\.

```
from sagemaker_inference import encoder
def output_fn(self, prediction, accept):
        """A default output_fn for PyTorch. Serializes predictions from predict_fn to JSON, CSV or NPY format.

        Args:
            prediction: a prediction result from predict_fn
            accept: type which the output data needs to be serialized

        Returns: output data serialized
        """
        return encoder.encode(prediction, accept)
```

## Step 2: Implement a Handler Service<a name="byoc-inference-handler-service"></a>

The handler service is executed by the model server\. The handler service implements `initialize` and `handle` methods\. The `initialize` method is invoked when the model server starts, and the `handle` method is invoked for all incoming inference requests to the model server\. For more information, see [Custom Service](https://github.com/awslabs/multi-model-server/blob/master/docs/custom_service.md) in the Multi\-model server documentation\. The following is an example of a handler service for a PyTorch model server\.

```
from sagemaker_inference.default_handler_service import DefaultHandlerService
from sagemaker_inference.transformer import Transformer
from sagemaker_pytorch_serving_container.default_inference_handler import DefaultPytorchInferenceHandler


class HandlerService(DefaultHandlerService):
    """Handler service that is executed by the model server.
    Determines specific default inference handlers to use based on model being used.
    This class extends ``DefaultHandlerService``, which define the following:
        - The ``handle`` method is invoked for all incoming inference requests to the model server.
        - The ``initialize`` method is invoked at model server start up.
    Based on: https://github.com/awslabs/mxnet-model-server/blob/master/docs/custom_service.md
    """
    def __init__(self):
        transformer = Transformer(default_inference_handler=DefaultPytorchInferenceHandler())
        super(HandlerService, self).__init__(transformer=transformer)
```

## Step 3: Implement an Entrypoint<a name="byoc-inference-entrypoint"></a>

The entrypoint starts the model server by invoking the handler service\. You specify the location of the entrypoint in your Dockerfile\. The following is an example of an entrypoint\.

```
from sagemaker_inference import model_server

model_server.start_model_server(handler_service=HANDLER_SERVICE)
```

## Step 4: Write a Dockerfile<a name="byoc-inference-dockerfile"></a>

In your `Dockerfile`, copy the model handler from step 2 and specify the Python file from the previous step as the entrypoint in your `Dockerfile`\. The following is an example of the lines you can add to your Dockerfile to copy the model handler and specify the entrypoint\. For a full example of a Dockerfile for an inference container, see [Dockerfile](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/advanced_functionality/multi_model_bring_your_own/container/Dockerfile)\.

```
# Copy the default custom service file to handle incoming data and inference requests
COPY model_handler.py /home/model-server/model_handler.py

# Define an entrypoint script for the docker image
ENTRYPOINT ["python", "/usr/local/bin/entrypoint.py"]
```

## Step 5: Build and Register Your Container<a name="byoc-inference-build-register"></a>

Now you can build your container and register it in Amazon Elastic Container Registry \(Amazon ECR\)\. The following shell script from the sample notebook builds the container and uploads it to an Amazon ECR repository in your AWS account\.

**Note**  
SageMaker hosting supports using inference containers that are stored in repositories other than Amazon ECR\. For information, see [Use a Private Docker Registry for Real\-Time Inference Containers](your-algorithms-containers-inference-private.md)\.

The following shell script shows how to build and register a container\.

```
%%sh

# The name of our algorithm
algorithm_name=demo-sagemaker-multimodel

cd container

account=$(aws sts get-caller-identity --query Account --output text)

# Get the region defined in the current configuration (default to us-west-2 if none defined)
region=$(aws configure get region)
region=${region:-us-west-2}

fullname="${account}.dkr.ecr.${region}.amazonaws.com/${algorithm_name}:latest"

# If the repository doesn't exist in ECR, create it.
aws ecr describe-repositories --repository-names "${algorithm_name}" > /dev/null 2>&1

if [ $? -ne 0 ]
then
    aws ecr create-repository --repository-name "${algorithm_name}" > /dev/null

# Get the login command from ECR and execute it directly
$(aws ecr get-login --region ${region} --no-include-email)

# Build the docker image locally with the image name and then push it to ECR
# with the full name.

docker build -q -t ${algorithm_name} .
docker tag ${algorithm_name} ${fullname}

docker push ${fullname}
```

You can now use this container to deploy endpoints in SageMaker\. For an example of how to deploy an endpoint in SageMaker, see [Deploy the Model to SageMaker Hosting Services](ex1-model-deployment.md#ex1-deploy-model)\.