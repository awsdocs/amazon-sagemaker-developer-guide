# Use EI on Amazon SageMaker Hosted Endpoints<a name="ei-endpoints"></a>

To use Elastic Inference \(EI\) in Amazon SageMaker with a hosted endpoint for real\-time inference, specify an EI accelerator when you create the deployable model to be hosted at that endpoint\. You can do this in one of the following ways:
+ Use the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) versions of either the TensorFlow, MXNet, or PyTorch and the SageMaker pre\-built containers for TensorFlow, MXNet, and PyTorch
+ Build your own container, and use the low\-level SageMaker API \(Boto 3\)\. You will need to import the EI\-enabled version of either TensorFlow, MXNet, or PyTorch from the provided Amazon S3 locations into your container, and use one of those versions to write your training script\.
+ Use either the [Image Classification Algorithm](image-classification.md) or [Object Detection Algorithm](object-detection.md) build\-in algorithms, and use the AWS SDK for Python \(Boto3\) to run your training job and create your deployable model and hosted endpoint\.

**Topics**
+ [Use EI with a SageMaker TensorFlow Container](#ei-endpoints-tensorflow)
+ [Use EI with a SageMaker MXNet Container](#ei-endpoints-mxnet)
+ [Use EI with a SageMaker PyTorch Container](#ei-endpoints-pytorch)
+ [Use EI with Your Own Container](#ei-endpoints-boto3)

## Use EI with a SageMaker TensorFlow Container<a name="ei-endpoints-tensorflow"></a>

To use TensorFlow with EI in SageMaker, you need to call the `deploy` method of either the [Estimator](https://sagemaker.readthedocs.io/en/stable/sagemaker.tensorflow.html#tensorflow-estimator) or [Model](https://sagemaker.readthedocs.io/en/stable/sagemaker.tensorflow.html#tensorflow-model) objects\. You then specify an accelerator type using the accelerator\_type input argument\. For information on using TensorFlow in the SageMaker Python SDK, see: [https://sagemaker\.readthedocs\.io/en/stable/frameworks/tensorflow/index\.html](https://sagemaker.readthedocs.io/en/stable/frameworks/tensorflow/index.html)\.

SageMaker provides default model training and inference code for your convenience\. For custom file formats, you might need to implement custom model training and inference code\.

### Use an Estimator Object<a name="ei-endpoints-tensorflow-estimator"></a>

To use an estimator object with EI, when you use the deploy method, include the `accelerator_type` input argument\. The estimator returns a predictor object, which we call its deploy method, as shown in the example code\.

```
# Deploy an estimator using EI (using the accelerator_type input argument)
predictor = estimator.deploy(initial_instance_count=1,
                             instance_type='ml.m4.xlarge',
                             accelerator_type='ml.eia2.medium')
```

### Use a Model Object<a name="ei-endpoints-tensorflow-model"></a>

To use a model object with EI, when you use the deploy method, include the `accelerator_type` input argument\. The estimator returns a predictor object, which we call its deploy method, as shown in the example code\.

```
# Deploy a model using EI (using the accelerator_type input argument)
predictor = model.deploy(initial_instance_count=1,
                             instance_type='ml.m4.xlarge',
                             accelerator_type='ml.eia2.medium')
```

## Use EI with a SageMaker MXNet Container<a name="ei-endpoints-mxnet"></a>

To use MXNet with EI in SageMaker, you need to call the `deploy` method of either the [Estimator](https://sagemaker.readthedocs.io/en/stable/sagemaker.mxnet.html#mxnet-estimator) or [Model](https://sagemaker.readthedocs.io/en/stable/sagemaker.mxnet.html#mxnet-model) objects\. You then specify an accelerator type using the `accelerator_type` input argument\. For information about using MXNet in the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io), see [https://sagemaker\.readthedocs\.io/en/stable/frameworks/mxnet/index\.html](https://sagemaker.readthedocs.io/en/stable/frameworks/mxnet/index.html)

For your convenience, SageMaker provides default model training and inference code\. For custom file formats, you might need to write custom model training and inference code\.

### Use an Estimator Object<a name="ei-endpoints-mxnet-estimator"></a>

To use an estimator object with EI, when you use the deploy method, include the `accelerator_type` input argument\. The estimator returns a predictor object, which we call its deploy method, as shown in the example code\. 

```
# Deploy an estimator using EI (using the accelerator_type input argument)
predictor = estimator.deploy(initial_instance_count=1,
                             instance_type='ml.m4.xlarge',
                             accelerator_type='ml.eia2.medium')
```

### Use a Model Object<a name="ei-endpoints-mxnet-model"></a>

To use a model object with EI, when you use the deploy method, include the `accelerator_type` input argument\. The estimator returns a predictor object, which we call its deploy method, as shown in the example code\. 

```
# Deploy a model using EI (using the accelerator_type input argument)
predictor = model.deploy(initial_instance_count=1,
                             instance_type='ml.m4.xlarge',
                             accelerator_type='ml.eia2.medium')
```

For a complete example of using EI with MXNet in SageMaker, see the sample notebook at [https://github\.com/awslabs/amazon\-sagemaker\-examples/blob/master/sagemaker\-python\-sdk/mxnet\_mnist/mxnet\_mnist\_elastic\_inference\.ipynb ](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker-python-sdk/mxnet_mnist/mxnet_mnist_elastic_inference.ipynb)

## Use EI with a SageMaker PyTorch Container<a name="ei-endpoints-pytorch"></a>

To use PyTorch with EI in SageMaker, you need to call the `deploy` method of either the [Estimator](https://sagemaker.readthedocs.io/en/stable/sagemaker.pytorch.html#pytorch-estimator) or [Model](https://sagemaker.readthedocs.io/en/stable/sagemaker.pytorch.html#pytorch-model) objects\. You then specify an accelerator type using the `accelerator_type` input argument\. For information about using PyTorch in the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io), see [SageMaker PyTorch Estimators and Models](https://sagemaker.readthedocs.io/en/stable/frameworks/pytorch/index.html)\.

For your convenience, SageMaker provides default model training and inference code\. For custom file formats, you might need to write custom model training and inference code\.

### Use an Estimator Object<a name="ei-endpoints-pytorch-estimator"></a>

To use an estimator object with EI, when you use the deploy method, include the `accelerator_type` input argument\. The estimator returns a predictor object, which we call its deploy method, as shown in this example code\.

```
# Deploy an estimator using EI (using the accelerator_type input argument)
predictor = estimator.deploy(initial_instance_count=1,
                             instance_type='ml.m4.xlarge',
                             accelerator_type='ml.eia2.medium')
```

### Use a Model Object<a name="ei-endpoints-pytorch-model"></a>

To use a model object with EI, when you use the deploy method, include the `accelerator_type` input argument\. The model returns a predictor object, which we call its deploy method, as shown in this example code\.

```
# Deploy a model using EI (using the accelerator_type input argument)
predictor = model.deploy(initial_instance_count=1,
                             instance_type='ml.m4.xlarge',
                             accelerator_type='ml.eia2.medium')
```

## Use EI with Your Own Container<a name="ei-endpoints-boto3"></a>

To use EI with a model in a custom container that you build, use the low\-level AWS SDK for Python \(Boto 3\)\. download and import the AWS EI\-enabled versions of TensorFlow, Apache MXNet, or PyTorch machine learning frameworks, and write your training script using those frameworks\.

### Import the EI Version of TensorFlow, MXNet, or PyTorch into Your Docker Container<a name="ei-docker-container"></a>

To use EI with your own container, you need to import either the Amazon EI TensorFlow Serving library, the Amazon EI Apache MXNet library, or the Elastic Inference enabled PyTorch library into your container\. The EI\-enabled versions of TensorFlow and MXNet are currently available as binary files stored in Amazon S3 locations\. You can download the EI\-enabled binary for TensorFlow from the Amazon S3 bucket at [ https://s3\.console\.aws\.amazon\.com/s3/buckets/amazonei\-tensorflow](                         https://s3.console.aws.amazon.com/s3/buckets/amazonei-tensorflow)\. For information about building a container that uses the EI\-enabled version of TensorFlow, see [https://github\.com/aws/sagemaker\-tensorflow\-container\#building\-the\-sagemaker\-elastic\-inference\-tensorflow\-serving\-container](https://github.com/aws/sagemaker-tensorflow-container#building-the-sagemaker-elastic-inference-tensorflow-serving-container)\. You can download the EI\-enabled binary for Apache MXNet from the public Amazon S3 bucket at [https://s3\.console\.aws\.amazon\.com/s3/buckets/amazonei\-apachemxnet](https://s3.console.aws.amazon.com/s3/buckets/amazonei-apachemxnet)\. For information about building a container that uses the EI\-enabled version of MXNet, see [https://github\.com/aws/sagemaker\-mxnet\-container\#building\-the\-sagemaker\-elastic\-inference\-mxnet\-container](https://github.com/aws/sagemaker-mxnet-container#building-the-sagemaker-elastic-inference-mxnet-container)\. You can download the Elastic Inference enabled binary for PyTorch from the public Amazon S3 bucket at [https://s3\.console\.aws\.amazon\.com/s3/buckets/amazonei\-pytorch](https://s3.console.aws.amazon.com/s3/buckets/amazonei-pytorch)\. For information about building a container that uses the Elastic Inference enabled version of PyTorch, see [Building your image](https://github.com/aws/sagemaker-pytorch-serving-container/#building-your-image)\. 

### Create an EI Endpoint with AWS SDK for Python \(Boto 3\)<a name="ei-create-endpoint-boto"></a>

To create an endpoint by using AWS SDK for Python \(Boto 3\), you first create an endpoint configuration\. The endpoint configuration specifies one or more models \(called production variants\) that you want to host at the endpoint\. To attach EI to one or more of the production variants hosted at the endpoint, you specify one of the EI instance types as the `AcceleratorType` field for that `ProductionVariant`\. You then pass that endpoint configuration when you create the endpoint\.

#### Create an Endpoint Configuration<a name="ei-endpoints-boto3-endpoint-config"></a>

To use EI, you need to specify an accelerator type in the endpoint configuration\.

```
# Create Endpoint Configuration
from time import gmtime, strftime

endpoint_config_name = 'ImageClassificationEndpointConfig-' + strftime("%Y-%m-%d-%H-%M-%S", gmtime())
print(endpoint_config_name)
create_endpoint_config_response = sagemaker.create_endpoint_config(
    EndpointConfigName = endpoint_config_name,
    ProductionVariants=[{
        'InstanceType':'ml.m4.xlarge',
        'InitialInstanceCount':1,
        'ModelName':model_name,
        'VariantName':'AllTraffic',
        'AcceleratorType':'ml.eia2.medium'}])

print("Endpoint Config Arn: " + create_endpoint_config_response['EndpointConfigArn'])
```

#### Create an Endpoint<a name="ei-endpoints-boto3-endpoint"></a>

After you create an endpoint configuration with an accelerator type, you can create an endpoint\.

```
endpoint_name = 'ImageClassificationEndpoint-' + strftime("%Y-%m-%d-%H-%M-%S", gmtime())
endpoint_response = sagemaker.create_endpoint(
    EndpointName=endpoint_name,
    EndpointConfigName=endpoint_config_name)
```

After creating the endpoint, you can invoke it using the `invoke_endpoint` method in a Boto3 runtime object, as you would any other endpoint\.