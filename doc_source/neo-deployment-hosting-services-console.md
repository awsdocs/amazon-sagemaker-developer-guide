# Deploy a Compiled Model Using the Console<a name="neo-deployment-hosting-services-console"></a>

Follow the steps below to create and deploy a Neo compiled model using the [Amazon SageMaker console](https://console.aws.amazon.com/sagemaker/)\. 

**Topics**
+ [Prerequisites](#neo-deployment-hosting-services-cli-prerequisites)
+ [Deploy the Model](#deploy-the-model-console-steps)

## Prerequisites<a name="neo-deployment-hosting-services-cli-prerequisites"></a>

To create a model, you need the following:

1. A docker image ECR URI\. You can select one that meets your needs from the list [here](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-deployment-hosting-services-container-images.html)\.

1. An entry point script file:

   *If you trained your model using SageMaker*, the training script must implement the functions described below\. The training script serves as the entry point script during inference\. In the example detailed in [ MNIST Training, Compilation and Deployment with MXNet Module and SageMaker Neo](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker_neo_compilation_jobs/mxnet_mnist/mxnet_mnist_neo.ipynb), the training script \(`mnist.py`\) implements the required functions\.

   *If you did not train your model using SageMaker*, you should provide an entry point script \(`inference.py`\) file that can be used at the time of inference\. Based on the framework—MXNet or PyTorch—the inference script location must conform to the SageMaker Python SDK [Model Directory Structure for MxNet](https://sagemaker.readthedocs.io/en/stable/frameworks/mxnet/using_mxnet.html#model-directory-structure) or [ Model Directory Structure for PyTorch](https://sagemaker.readthedocs.io/en/stable/frameworks/pytorch/using_pytorch.html#model-directory-structure)\. 

    When using Neo inference optimized container images with **PyTorch** and **MXNet** on CPU and GPU instance types, the inference script must implement the following functions: 
   + `model_fn` – Loads the model\.
   + `input_fn` – Converts the incoming request payload into a numpy array\.
   + `predict_fn` – Performs the prediction\.
   + `output_fn` – Converts the prediction output into the response payload\.
   + Alternatively, you can define `transform_fn` to combine `input_fn`, `predict_fn` and `output_fn`\.

   The following are examples of `inference.py` script within a directory named `code` \(`code/inference.py`\) for **PyTorch and MXNet \(Gluon and Module\)\.** These examples first load the model and then serve it on image data on a GPU: 

------
#### [ MXNet Module ]

   ```
   import numpy as np
   import json
   import mxnet as mx
   # Please make sure to import neomxnet
   import neomxnet  # noqa: F401
   from collections import namedtuple
   
   Batch = namedtuple('Batch', ['data'])
   # Change the context to mx.cpu() if deploying to a CPU endpoint
   ctx = mx.gpu()
   
   def model_fn(model_dir):
       # The compiled model artifacts are saved with the prefix 'compiled'
       sym, arg_params, aux_params = mx.model.load_checkpoint('compiled', 0)
       mod = mx.mod.Module(symbol=sym, context=ctx, label_names=None)
       exe = mod.bind(for_training=False,
                  data_shapes=[('data', (1,3,224,224))],
                  label_shapes=mod._label_shapes)
       mod.set_params(arg_params, aux_params, allow_missing=True)
       # Run warm-up inference on empty data during model load (required for GPU)
       data = mx.nd.empty((1,3,224,224), ctx=ctx)
       mod.forward(Batch([data]))
       return mod
       
   def transform_fn(mod, image, input_content_type, output_content_type):
       # pre-processing
       decoded = mx.image.imdecode(image)
       resized = mx.image.resize_short(decoded, 224)
       cropped, crop_info = mx.image.center_crop(resized, (224, 224))
       normalized = mx.image.color_normalize(cropped.astype(np.float32) / 255,
                                             mean=mx.nd.array([0.485, 0.456, 0.406]),
                                             std=mx.nd.array([0.229, 0.224, 0.225]))
       transposed = normalized.transpose((2, 0, 1))
       batchified = transposed.expand_dims(axis=0)
       casted = batchified.astype(dtype='float32')
       processed_input = casted.as_in_context(ctx)
       # prediction/inference
       mod.forward(Batch([processed_input]))
       # post-processing
       prob = mod.get_outputs()[0].asnumpy().tolist()
       prob_json = json.dumps(prob)
       return prob_json, output_content_type
   ```

------
#### [ MXNet Gluon ]

   ```
   import numpy as np
   import json
   import mxnet as mx
   # Please make sure to import neomxnet
   import neomxnet  # noqa: F401
   
   # Change the context to mx.cpu() if deploying to a CPU endpoint
   ctx = mx.gpu()
   
   def model_fn(model_dir):
       # The compiled model artifacts are saved with the prefix 'compiled'
       block = mx.gluon.nn.SymbolBlock.imports('compiled-symbol.json',['data'],'compiled-0000.params', ctx=ctx)
       # Hybridize the model & pass required options for Neo: static_alloc=True & static_shape=True
       block.hybridize(static_alloc=True, static_shape=True)
       # Run warm-up inference on empty data during model load (required for GPU)
       data = mx.nd.empty((1,3,224,224), ctx=ctx)
       warm_up = block(data)
       return block
   
   def input_fn(image, input_content_type):
       # pre-processing
       decoded = mx.image.imdecode(image)
       resized = mx.image.resize_short(decoded, 224)
       cropped, crop_info = mx.image.center_crop(resized, (224, 224))
       normalized = mx.image.color_normalize(cropped.astype(np.float32) / 255,
                                             mean=mx.nd.array([0.485, 0.456, 0.406]),
                                             std=mx.nd.array([0.229, 0.224, 0.225]))
       transposed = normalized.transpose((2, 0, 1))
       batchified = transposed.expand_dims(axis=0)
       casted = batchified.astype(dtype='float32')
       processed_input = casted.as_in_context(ctx)
       return processed_input
   
   def predict_fn(processed_input_data, block):
       # prediction/inference
       prediction = block(processed_input_data)
       return prediction
   
   def output_fn(prediction, output_content_type):
       # post-processing
       prob = prediction.asnumpy().tolist()
       prob_json = json.dumps(prob)
       return prob_json, output_content_type
   ```

------
#### [ PyTorch ]

   ```
   import os
   import torch
   import torch.nn.parallel
   import torch.optim
   import torch.utils.data
   import torch.utils.data.distributed
   import torchvision.transforms as transforms
   from PIL import Image
   import io
   import json
   import pickle
   
   
   def model_fn(model_dir):
       """Load the model and return it.
       Providing this function is optional.
       There is a default model_fn available which will load the model
       compiled using SageMaker Neo. You can override it here.
   
       Keyword arguments:
       model_dir -- the directory path where the model artifacts are present
       """
   
       # The compiled model is saved as "compiled.pt"
       model_path = os.path.join(model_dir, 'compiled.pt')
       with torch.neo.config(model_dir=model_dir, neo_runtime=True):
           model = torch.jit.load(model_path)
           device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
           model = model.to(device)
   
           # We recommend that you run warm-up inference during model load
           sample_input_path = os.path.join(model_dir, 'sample_input.pkl')
           with open(sample_input_path, 'rb') as input_file:
               model_input = pickle.load(input_file)
           if torch.is_tensor(model_input):
               model_input = model_input.to(device)
               model(model_input)
           elif isinstance(model_input, tuple):
               model_input = (inp.to(device)
                              for inp in model_input if torch.is_tensor(inp))
               model(*model_input)
           else:
               print("Only supports a torch tensor or a tuple of torch tensors")
   
           return model
   
   
   def transform_fn(model, request_body, request_content_type,
                    response_content_type):
       """Run prediction and return the output.
       The function
       1. Pre-processes the input request
       2. Runs prediction
       3. Post-processes the prediction output.
       """
       # preprocess
       decoded = Image.open(io.BytesIO(request_body))
       preprocess = transforms.Compose([
           transforms.Resize(256),
           transforms.CenterCrop(224),
           transforms.ToTensor(),
           transforms.Normalize(
               mean=[
                   0.485, 0.456, 0.406], std=[
                   0.229, 0.224, 0.225]),
       ])
       normalized = preprocess(decoded)
       batchified = normalized.unsqueeze(0)
       # predict
       device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
       batchified = batchified.to(device)
       output = model.forward(batchified)
   
       return json.dumps(output.cpu().numpy().tolist()), response_content_type
   ```

------

    For all other Neo Inference optimized container images or for inferentia instance types, the entry point script must implement the following functions to be used with the Neo Deep Learning Runtime: 
   + `neo_preprocess` – Converts the incoming request payload into a numpy array\.
   + `neo_postprocess` – Converts the prediction output from Neo Deep Learning Runtime into the response body\.
**Note**  
The preceding two functions do not use any of the functionalities of MXNet, PyTorch, or TensorFlow\.

    For examples of how to use these functions, see [Neo Model Compilation Sample Notebooks](https://docs.aws.amazon.com/sagemaker/latest/dg/neo.html#neo-sample-notebooks)\. 

1. The Amazon S3 bucket URI to an archive containing the compiled model artifacts and the entry point script file\.

## Deploy the Model<a name="deploy-the-model-console-steps"></a>

 After you have satisfied the prerequisites, use the below steps to deploy a model compiled with Neo: 

1. Choose **Models**, and then choose **Create models** from the **Inference** group\. On the **Create model** page, complete the **Model name**,** IAM role**, and **VPC** fields \(optional\), if needed\.  
![\[Create Neo Model for Inference\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/neo-deploy-console-create-inference-model.png)

1. To add information about the container used to deploy your model, choose **Add** container, then choose **Next**\. Complete the **Container input options**, **Location of inference code image**, and **Location of model artifacts**, and optionally, **Container host name**, and **Environmental variables** fields\.  
![\[Create Neo Model for Inference\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/neo-deploy-console-container-definition.png)

1. To deploy Neo\-compiled models, choose the following:
   + **Container input options** – Choose **Provide model artifacts and inference image**\.
   + **Location of inference code image** – Choose the inference image URI from [Neo Inference Container Images](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-deployment-hosting-services-container-images.html), depending on the AWS Region and kind of application\. 
   + **Location of model artifact** – Enter the Amazon S3 bucket URI of the compiled model artifact generated by the Neo compilation API\.
   + **Environment variables**:
     + Leave this field blank for **SageMaker XGBoost**\.
     +  If you trained your model using SageMaker, specify the environment variable `SAGEMAKER_SUBMIT_DIRECTORY` as the Amazon S3 bucket URI that contains the training script\. 
     +  If you did not train your model using SageMaker, specify the following environment variables:     
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/neo-deployment-hosting-services-console.html)

1. Confirm that the information for the containers is accurate, and then choose **Create model**\. On the **Create model landing page**, choose **Create endpoint**\.   
![\[Create Model Landing Page\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/neo-deploy-console-create-model-land-page.png)

1. In **Create and configure endpoint** diagram, specify the **Endpoint name**\. For **Attach endpoint configuration**, choose **Create a new endpoint configuration**\.  
![\[Neo console Create and configure endpoint UI.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/neo-deploy-console-config-endpoint.png)

1. In **New endpoint configuration** page, specify the **Endpoint configuration name**\.   
![\[Neo console New endpoint configuration UI.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/neo-deploy-console-new-endpoint-config.png)

1. Press **Edit** next to the name of the model and specify the correct **Instance type** on the **Edit Production Variant** page\. It is imperative that the **Instance type** value match the one specified in your compilation job\.  
![\[Neo console New endpoint configuration UI.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/neo-deploy-console-edit-production-variant.png)

1. Choose **Save**\.

1. On the **New endpoint configuration** page, choose **Create endpoint configuration**, and then choose **Create endpoint**\.