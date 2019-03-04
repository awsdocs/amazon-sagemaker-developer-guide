# Use EI on Amazon SageMaker Hosted Endpoints<a name="ei-endpoints"></a>

To use Elastic Inference \(EI\) in Amazon SageMaker with a hosted endpoint for real\-time inference, specify an EI accelerator when you create the deployable model to be hosted at that endpoint\. You can do this in one of the following ways:
+ Use the Amazon SageMaker Python SDK versions of either the TensorFlow or MXNet and the Amazon SageMaker pre\-built containers for TensorFlow and MXNet
+ Build your own container, and use the low\-level Amazon SageMaker API \(Boto 3\)\. You will need to import the EI\-enabled version of either TensorFlow or MXNet from the provided Amazon S3 locations into your container, and use one of those versions to write your training script\.
+ Use either the [Image Classification Algorithm](image-classification.md) or [Object Detection Algorithm](object-detection.md) build\-in algorithms, and use Boto 3 to run your training job and create your deployable model and hosted endpoint\.

**Topics**
+ [Use EI with an Amazon SageMaker TensorFlow Container](#ei-endpoints-tensorflow)
+ [Use EI with an Amazon SageMaker MXNet Container](#ei-endpoints-mxnet)
+ [Use EI with Your Own Container](#ei-endpoints-boto3)

## Use EI with an Amazon SageMaker TensorFlow Container<a name="ei-endpoints-tensorflow"></a>

To use TensorFlow with EI in Amazon SageMaker, you need to call the `deploy` method of either the [Estimator](https://sagemaker.readthedocs.io/en/latest/sagemaker.tensorflow.html#tensorflow-estimator) or [Model](https://sagemaker.readthedocs.io/en/latest/sagemaker.tensorflow.html#tensorflow-model) objects\. You then specify an accelerator type using the accelerator\_type input argument\. For information on using TensorFlow in the Amazon SageMaker Python SDK, see: [https://github\.com/aws/sagemaker\-python\-sdk/blob/master/src/sagemaker/tensorflow/README\.rst](https://github.com/aws/sagemaker-python-sdk/blob/master/src/sagemaker/tensorflow/README.rst)\.

Amazon SageMaker provides default model training and inference code for your convenience\. For custom file formats, you might need to implement custom model training and inference code\.

### Use an Estimator Object<a name="ei-endpoints-tensorflow-estimator"></a>

To use an estimator object with EI, include the `accelerator_type` input argument when you use the deploy method\. The estimator returns a predictor object which we call its deploy method as shown in the example code:

```
# Deploy an estimator using EI (using the accelerator_type input argument)
predictor = estimator.deploy(initial_instance_count=1,
                             instance_type='ml.m4.xlarge',
                             accelerator_type='ml.eia1.medium')
```

### Use a Model Object<a name="ei-endpoints-tensorflow-model"></a>

To use a model object with EI, include the `accelerator_type` input argument when you use the deploy method\. The estimator returns a predictor object which we call its deploy method as shown in the example code: 

```
# Deploy a model using EI (using the accelerator_type input argument)
predictor = model.deploy(initial_instance_count=1,
                             instance_type='ml.m4.xlarge',
                             accelerator_type='ml.eia1.medium')
```

## Use EI with an Amazon SageMaker MXNet Container<a name="ei-endpoints-mxnet"></a>

To use MXNet with EI in Amazon SageMaker, you need to call the `deploy` method of either the [Estimator](https://sagemaker.readthedocs.io/en/latest/sagemaker.mxnet.html#mxnet-estimator) or [Model](https://sagemaker.readthedocs.io/en/latest/sagemaker.mxnet.html#mxnet-model) objects\. You then specify an accelerator type using the accelerator\_type input argument\. For information on using MXNet in the Amazon SageMaker Python SDK, see [https://github\.com/aws/sagemaker\-python\-sdk/blob/master/src/sagemaker/mxnet/README\.rst](https://github.com/aws/sagemaker-python-sdk/blob/master/src/sagemaker/mxnet/README.rst)

Amazon SageMaker provides default model training and inference code for your convenience\. For custom file formats, you might need to implement custom model training and inference code\.

### Use an Estimator Object<a name="ei-endpoints-mxnet-estimator"></a>

To use an estimator object with EI, include the `accelerator_type` input argument when you use the deploy method\. The estimator returns a predictor object which we call its deploy method as shown in the example code: 

```
# Deploy an estimator using EI (using the accelerator_type input argument)
predictor = estimator.deploy(initial_instance_count=1,
                             instance_type='ml.m4.xlarge',
                             accelerator_type='ml.eia1.medium')
```

### Use a Model Object<a name="ei-endpoints-mxnet-model"></a>

To use a model object with EI, include the `accelerator_type` input argument when you use the deploy method\. The estimator returns a predictor object which we call its deploy method as shown in the example code: 

```
# Deploy a model using EI (using the accelerator_type input argument)
predictor = model.deploy(initial_instance_count=1,
                             instance_type='ml.m4.xlarge',
                             accelerator_type='ml.eia1.medium')
```

For a complete example of using EI with MXNet in Amazon SageMaker, see the sample notebook at [https://github\.com/awslabs/amazon\-sagemaker\-examples/blob/master/sagemaker\-python\-sdk/mxnet\_mnist/mxnet\_mnist\_elastic\_inference\.ipynb ](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker-python-sdk/mxnet_mnist/mxnet_mnist_elastic_inference.ipynb)

## Use EI with Your Own Container<a name="ei-endpoints-boto3"></a>

To use EI with a model in a custom container that you build, use the low\-level Amazon SageMaker SDK for Python \(Boto 3\)\. download and import the AWS EI\-enabled versions of TensorFlow or Apache MXNet machine learning frameworks, and write your training script using those frameworks\.

### Import the EI Version of TensorFlow or MXNet into Your Docker Container<a name="ei-docker-container"></a>

To use EI with your own container, you need to import either the Amazon EI TensorFlow Serving library or the Amazon EI Apache MXNet library into your container\. The EI\-enabled versions of TensorFlow and MXNet are currently available as binary files stored in Amazon S3 locations\. You can download the EI\-enabled binary for TensorFlow from the Amazon S3 bucket at [ https://s3\.console\.aws\.amazon\.com/s3/buckets/amazonei\-tensorflow](                         https://s3.console.aws.amazon.com/s3/buckets/amazonei-tensorflow)\. For information about building a container that uses the EI\-enabled version of TensorFlow, see [https://github\.com/aws/sagemaker\-tensorflow\-container\#building\-the\-sagemaker\-elastic\-inference\-tensorflow\-serving\-container](https://github.com/aws/sagemaker-tensorflow-container#building-the-sagemaker-elastic-inference-tensorflow-serving-container)\. You can download the EI\-enabled binary for Apache MXNet from the public Amazon S3 bucket at [https://s3\.console\.aws\.amazon\.com/s3/buckets/amazonei\-apachemxnet](https://s3.console.aws.amazon.com/s3/buckets/amazonei-apachemxnet)\. For information about buidling a container that uses the EI\-enabled version of MXNet, see [https://github\.com/aws/sagemaker\-mxnet\-container\#building\-the\-sagemaker\-elastic\-inference\-mxnet\-container](https://github.com/aws/sagemaker-mxnet-container#building-the-sagemaker-elastic-inference-mxnet-container)\.

### Create an EI Endpoint with Boto 3<a name="ei-create-endpoint-boto"></a>

To create an endpoint by using Boto 3, you first create an endpoint configuration\. The endpoint configuration specifies one or more models \(called production variants\) that you want to host at the endpoint\. To attach EI to one or more of the production variants hosted at the endpoint, you specify one of the EI instance types as the `AcceleratorType` field for that `ProductionVariant`\. You then pass that endpoint configuration when you create the endpoint\.

#### Create an Endpoint Configuration<a name="ei-endpoints-boto3-endpoint-config"></a>

To use EI, you need to specify an accelerator type in the endpoint configuration:

```
# Create Endpoint Configuration
from time import gmtime, strftime

endpoint_config_name = 'ImageClassificationEndpointConfig-' + strftime("%Y-%m-%d-%H-%M-%S", gmtime())
print(endpoint_config_name)
create_endpoint_config_response = sagemaker.create_endpoint_config(
    EndpointConfigName = "MyEIEndpointConfig",
    ProductionVariants=[{
        'InstanceType':'ml.m4.xlarge',
        'InitialInstanceCount':1,
        'ModelName':model_name,
        'VariantName':'AllTraffic',
        'AcceleratorType':'ml.eia1.medium'}])

print("Endpoint Config Arn: " + create_endpoint_config_response['EndpointConfigArn'])
```

#### Create an Endpoint<a name="ei-endpoints-boto3-endpoint"></a>

After you create an endpoint configuration with an accelerator type, you can proceed to create an endpoint\.

```
endpoint_response = sagemaker.create_endpoint("MyEIEndpointConfig")
```

After the endpoint is created you can invoke it using the invoke\_endpoint method in a boto3 runtime object as you would any other endpoint\.