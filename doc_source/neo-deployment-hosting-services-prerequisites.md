# Prerequisites<a name="neo-deployment-hosting-services-prerequisites"></a>

**Note**  
 Follow the instructions in this section if you compiled your model using AWS SDK for Python \(Boto3\), AWS CLI, or the SageMaker console\. 

To create a model, you need the following:

1. A docker image ECR URI\. You can select one that meets your needs from the list [here](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-deployment-hosting-services-container-images.html)\. 

1. An entry point script file:

   1. **For PyTorch and MXNet models:**

      *If you trained your model using SageMaker*, the training script must implement the functions described below\. The training script serves as the entry point script during inference\. In the example detailed in [ MNIST Training, Compilation and Deployment with MXNet Module and SageMaker Neo](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker_neo_compilation_jobs/mxnet_mnist/mxnet_mnist_neo.ipynb), the training script \(`mnist.py`\) implements the required functions\.

      * If you did not train your model using SageMaker*, you need to provide an entry point script \(`inference.py`\) file that can be used at the time of inference\. Based on the framework—MXNet or PyTorch—the inference script location must conform to the SageMaker Python SDK [Model Directory Structure for MxNet](https://sagemaker.readthedocs.io/en/stable/frameworks/mxnet/using_mxnet.html#model-directory-structure) or [ Model Directory Structure for PyTorch](https://sagemaker.readthedocs.io/en/stable/frameworks/pytorch/using_pytorch.html#model-directory-structure)\. 

      When using Neo Inference Optimized Container images with **PyTorch** and **MXNet** on CPU and GPU instance types, the inference script must implement the following functions: 
      + `model_fn` – Loads the model\.
      + `input_fn` – Converts the incoming request payload into a numpy array\.
      + `predict_fn` – Performs the prediction\.
      + `output_fn` – Converts the prediction output into the response payload\.
      + Alternatively, you can define `transform_fn` to combine `input_fn`, `predict_fn` and `output_fn`\.

      The following are examples of `inference.py` script within a directory named `code` \(`code/inference.py`\) for **PyTorch and MXNet \(Gluon and Module\)\.** The examples first load the model and then serve it on image data on a GPU: 

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

   1.  **For inf1 instance or onnx, xgboost, keras container images** 

      For all other Neo Inference optimized container images, or inferentia instance types, the entry point script must implement the following functions for Neo Deep Learning Runtime: 
      + `neo_preprocess` – Converts the incoming request payload into a numpy array\.
      + `neo_postprocess` – Converts the prediction output from Neo Deep Learning Runtime into the response body\.
**Note**  
The preceding two functions do not use any of the functionalities of MXNet, PyTorch, or TensorFlow\.

       For examples of how to use these functions, see [Neo Model Compilation Sample Notebooks](https://docs.aws.amazon.com/sagemaker/latest/dg/neo.html#neo-sample-notebooks)\. 

   1. **For TensorFlow models**

      If your model requires custom pre\- and post\-processing logic before data is sent to the model, then you must specify an entry point script `inference.py` file that can be used at the time of inference\. The script should implement either a either a pair of `input_handler` and `output_handler` functions or a single handler function\. 
**Note**  
Note that if handler function is implemented, `input_handler` and `output_handler` are ignored\. 

      The following is a code example of `inference.py` script that you can put together with the compile model to perform custom pre\- and post\-processing on an image classification model\. The SageMaker client will send the image file as an `application/x-image` content type to the `input_handler` function where it is converted to JSON\. The converted image file is then sent to the [Tensorflow Model Server \(TFX\)](https://www.tensorflow.org/tfx/serving/api_rest) using then REST API\. 

      ```
      import json
      import numpy as np
      import json
      import io
      from PIL import Image
       
      def input_handler(data, context):
          """ Pre-process request input before it is sent to TensorFlow Serving REST API
          Args:
              data (obj): the request data, in format of dict or string
              context (Context): an object containing request and configuration details
          Returns:
              (dict): a JSON-serializable dict that contains request body and headers
          """
          f = data.read()
          f = io.BytesIO(f)
          image = Image.open(f).convert('RGB')
          batch_size = 1
          image = np.asarray(image.resize((512, 512)))
          image = np.concatenate([image[np.newaxis, :, :]] * batch_size)
          body = json.dumps({"signature_name": "serving_default", "instances": image.tolist()})
          return body
       
      def output_handler(data, context):
          """Post-process TensorFlow Serving output before it is returned to the client.
          Args:
              data (obj): the TensorFlow serving response
              context (Context): an object containing request and configuration details
          Returns:
              (bytes, string): data to return to client, response content type
          """
          if data.status_code != 200:
              raise ValueError(data.content.decode('utf-8'))
       
          response_content_type = context.accept_header
          prediction = data.content
          return prediction, response_content_type
      ```

       If there is no custom pre\- or post\-processing, the SageMaker client will convert the file image to JSON in a similar way before sending it over to the SageMaker endpoint\. 

      For more information, see the [Deploying to TensorFlow Serving Endpoints in the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/frameworks/tensorflow/deploying_tensorflow_serving.html#providing-python-scripts-for-pre-pos-processing)\. 

1. The Amazon S3 bucket URI to an archive containing the compiled model artifacts\. 