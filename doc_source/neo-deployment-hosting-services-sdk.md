# Deploy a Compiled Model Using SageMaker SDK<a name="neo-deployment-hosting-services-sdk"></a>

You must satisfy the [ prerequisites](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-deployment-hosting-services-prerequisites) section if the model was compiled using AWS SDK for Python \(Boto3\), AWS CLI, or the Amazon SageMaker console\. Follow one of the following use cases to deploy a model compiled with SageMaker Neo based on you how you compiled your model\. 

**Topics**
+ [If you compiled your model using the SageMaker SDK](#neo-deployment-hosting-services-sdk-deploy-sm-sdk)
+ [If you compiled your model using MXNet or PyTorch](#neo-deployment-hosting-services-sdk-deploy-sm-boto3)
+ [If you compiled your model using Boto3, SageMaker console, or the CLI for TensorFlow](#neo-deployment-hosting-services-sdk-deploy-sm-boto3-tensorflow)

## If you compiled your model using the SageMaker SDK<a name="neo-deployment-hosting-services-sdk-deploy-sm-sdk"></a>

The [sagemaker\.Model](https://sagemaker.readthedocs.io/en/stable/api/inference/model.html?highlight=sagemaker.Model) object handle for the compiled model supplies the [deploy\(\)](https://sagemaker.readthedocs.io/en/stable/api/inference/model.html?highlight=sagemaker.Model#sagemaker.model.Model.deploy) function, which enables you to create an endpoint to serve inference requests\. The function lets you set the number and type of instances that are used for the endpoint\. You must choose an instance for which you have compiled your model\. For example, in the job compiled in [Compile a Model \(Amazon SageMaker SDK\)](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-job-compilation-sagemaker-sdk.html) section, this is `ml_c5`\. 

```
predictor = compiled_model.deploy(initial_instance_count = 1, instance_type = 'ml.c5.4xlarge')

# Print the name of newly created endpoint
print(predictor.endpoint_name)
```

## If you compiled your model using MXNet or PyTorch<a name="neo-deployment-hosting-services-sdk-deploy-sm-boto3"></a>

Create the SageMaker model and deploy it using the deploy\(\) API under the framework\-specific Model APIs\. For MXNet, it is [MXNetModel](https://sagemaker.readthedocs.io/en/stable/frameworks/mxnet/sagemaker.mxnet.html?highlight=MXNetModel#mxnet-model) and for PyTorch, it is [ PyTorchModel](https://sagemaker.readthedocs.io/en/stable/frameworks/pytorch/sagemaker.pytorch.html?highlight=PyTorchModel#sagemaker.pytorch.model.PyTorchModel)\. When you are creating and deploying an SageMaker model, you must set `MMS_DEFAULT_RESPONSE_TIMEOUT` environment variable to `500` and specify the `entry_point` parameter as the inference script \(`inference.py`\) and the `source_dir` parameter as the directory location \(`code`\) of the inference script\. To prepare the inference script \(`inference.py`\) follow the Prerequisites step\. 

The following example shows how to use these functions to deploy a compiled model using the SageMaker SDK for Python: 

------
#### [ MXNet ]

```
from sagemaker.mxnet import MXNetModel

# Create SageMaker model and deploy an endpoint
sm_mxnet_compiled_model = MXNetModel(
    model_data='insert S3 path of compiled MXNet model archive',
    role='AmazonSageMaker-ExecutionRole',
    entry_point='inference.py',
    source_dir='code',
    framework_version='1.8.0',
    py_version='py3',
    image_uri='insert appropriate ECR Image URI for MXNet',
    env={'MMS_DEFAULT_RESPONSE_TIMEOUT': '500'},
)

# Replace the example instance_type below to your preferred instance_type
predictor = sm_mxnet_compiled_model.deploy(initial_instance_count = 1, instance_type = 'ml.p3.2xlarge')

# Print the name of newly created endpoint
print(predictor.endpoint_name)
```

------
#### [ PyTorch 1\.4 and Older ]

```
from sagemaker.pytorch import PyTorchModel

# Create SageMaker model and deploy an endpoint
sm_pytorch_compiled_model = PyTorchModel(
    model_data='insert S3 path of compiled PyTorch model archive',
    role='AmazonSageMaker-ExecutionRole',
    entry_point='inference.py',
    source_dir='code',
    framework_version='1.4.0',
    py_version='py3',
    image_uri='insert appropriate ECR Image URI for PyTorch',
    env={'MMS_DEFAULT_RESPONSE_TIMEOUT': '500'},
)

# Replace the example instance_type below to your preferred instance_type
predictor = sm_pytorch_compiled_model.deploy(initial_instance_count = 1, instance_type = 'ml.p3.2xlarge')

# Print the name of newly created endpoint
print(predictor.endpoint_name)
```

------
#### [ PyTorch 1\.5 and Newer ]

```
from sagemaker.pytorch import PyTorchModel

# Create SageMaker model and deploy an endpoint
sm_pytorch_compiled_model = PyTorchModel(
    model_data='insert S3 path of compiled PyTorch model archive',
    role='AmazonSageMaker-ExecutionRole',
    entry_point='inference.py',
    source_dir='code',
    framework_version='1.5',
    py_version='py3',
    image_uri='insert appropriate ECR Image URI for PyTorch',
)

# Replace the example instance_type below to your preferred instance_type
predictor = sm_pytorch_compiled_model.deploy(initial_instance_count = 1, instance_type = 'ml.p3.2xlarge')

# Print the name of newly created endpoint
print(predictor.endpoint_name)
```

------

**Note**  
The `AmazonSageMakerFullAccess` and `AmazonS3ReadOnlyAccess` policies must be attached to the `AmazonSageMaker-ExecutionRole` IAM role\. 

## If you compiled your model using Boto3, SageMaker console, or the CLI for TensorFlow<a name="neo-deployment-hosting-services-sdk-deploy-sm-boto3-tensorflow"></a>

Construct a `TensorFlowModel` object, then call deploy: 

```
role='AmazonSageMaker-ExecutionRole'
model_path='S3 path for model file'
framework_image='inference container arn'
tf_model = TensorFlowModel(model_data=model_path,
                framework_version='1.15.3',
                role=role, 
                image_uri=framework_image)
instance_type='ml.c5.xlarge'
predictor = tf_model.deploy(instance_type=instance_type,
                    initial_instance_count=1)
```

See [Deploying directly from model artifacts](https://sagemaker.readthedocs.io/en/stable/frameworks/tensorflow/deploying_tensorflow_serving.html#deploying-directly-from-model-artifacts) for more information\. 

You can select a Docker image Amazon ECR URI that meets your needs from [this list](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-deployment-hosting-services-container-images.html)\. 

For more information on how to construct a `TensorFlowModel` object, see the [SageMaker SDK](https://sagemaker.readthedocs.io/en/stable/frameworks/tensorflow/sagemaker.tensorflow.html#tensorflow-serving-model)\. 

**Note**  
Your first inference request might have high latency if you deploy your model on a GPU\. This is because an optimized compute kernel is made on the first inference request\. We recommend that you make a warm\-up file of inference requests and store that alongside your model file before sending it off to a TFX\. This is known as “warming up” the model\. 

The following code snippet demonstrates how to produce the warm\-up file for image classification example in the [prerequisites](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-deployment-hosting-services-prerequisites) section: 

```
import tensorflow as tf
from tensorflow_serving.apis import classification_pb2
from tensorflow_serving.apis import inference_pb2
from tensorflow_serving.apis import model_pb2
from tensorflow_serving.apis import predict_pb2
from tensorflow_serving.apis import prediction_log_pb2
from tensorflow_serving.apis import regression_pb2
import numpy as np

with tf.python_io.TFRecordWriter("tf_serving_warmup_requests") as writer:       
    img = np.random.uniform(0, 1, size=[224, 224, 3]).astype(np.float32)
    img = np.expand_dims(img, axis=0)
    test_data = np.repeat(img, 1, axis=0)
    request = predict_pb2.PredictRequest()
    request.model_spec.name = 'compiled_models'
    request.model_spec.signature_name = 'serving_default'
    request.inputs['Placeholder:0'].CopyFrom(tf.compat.v1.make_tensor_proto(test_data, shape=test_data.shape, dtype=tf.float32))
    log = prediction_log_pb2.PredictionLog(
    predict_log=prediction_log_pb2.PredictLog(request=request))
    writer.write(log.SerializeToString())
```

For more information on how to “warm up” your model, see the [TensorFlow TFX page](https://www.tensorflow.org/tfx/serving/saved_model_warmup)\.