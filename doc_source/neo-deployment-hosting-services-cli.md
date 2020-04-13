# Deploy a Model Compiled with Neo \(AWS CLI\)<a name="neo-deployment-hosting-services-cli"></a>

The deployment of a Neo\-compiled model with the CLI has three steps\.

**Topics**
+ [Create a Model That Was Compiled with Neo \(AWS CLI\)](#neo-deployment-hosting-services-cli-create-model)
+ [Create the Endpoint Configuration \(AWS CLI\)](#neo-deployment-hosting-services-cli-create-endpoint-config)
+ [Create an Endpoint \(AWS CLI\)](#neo-deployment-hosting-services-cli-create-endpoint)

## Create a Model That Was Compiled with Neo \(AWS CLI\)<a name="neo-deployment-hosting-services-cli-create-model"></a>

For the full syntax of the `CreateModel` API, see [ `CreateModel`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html)\.

For Neo\-compiled models, use one of the following values for `PrimaryContainer`/`ContainerHostname`, depending on your region and applications: 
+ **Amazon SageMaker Image Classification**
  + `301217895009.dkr.ecr.us-west-2.amazonaws.com/image-classification-neo:latest`
  + `785573368785.dkr.ecr.us-east-1.amazonaws.com/image-classification-neo:latest`
  + `007439368137.dkr.ecr.us-east-2.amazonaws.com/image-classification-neo:latest`
  + `802834080501.dkr.ecr.eu-west-1.amazonaws.com/image-classification-neo:latest`
+ **Amazon SageMaker XGBoost**
  + `301217895009.dkr.ecr.us-west-2.amazonaws.com/xgboost-neo:latest` 
  + `785573368785.dkr.ecr.us-east-1.amazonaws.com/xgboost-neo:latest` 
  + `007439368137.dkr.ecr.us-east-2.amazonaws.com/xgboost-neo:latest` 
  + `802834080501.dkr.ecr.eu-west-1.amazonaws.com/xgboost-neo:latest` 
+ **TensorFlow **: The TensorFlow version used must be in [TensorFlow SageMaker Estimators](https://github.com/aws/sagemaker-python-sdk#tensorflow-sagemaker-estimators) list\.
  + `301217895009.dkr.ecr.us-west-2.amazonaws.com/sagemaker-neo-tensorflow:[tensorflow-version]-[cpu/gpu]-py3`
  + `785573368785.dkr.ecr.us-east-1.amazonaws.com/sagemaker-neo-tensorflow:[tensorflow-version]-[cpu/gpu]-py3`
  + `007439368137.dkr.ecr.us-east-2.amazonaws.com/sagemaker-neo-tensorflow:[tensorflow-version]-[cpu/gpu]-py3`
  + `802834080501.dkr.ecr.eu-west-1.amazonaws.com/sagemaker-neo-tensorflow:[tensorflow-version]-[cpu/gpu]-py3`
+ **MXNet **The MXNet version used must be in [MXNet SageMaker Estimators](https://github.com/aws/sagemaker-python-sdk#mxnet-sagemaker-estimators) list\.
  + `301217895009.dkr.ecr.us-west-2.amazonaws.com/sagemaker-neo-mxnet:[mxnet-version]-[cpu/gpu]-py3`
  + `785573368785.dkr.ecr.us-east-1.amazonaws.com/sagemaker-neo-mxnet:[mxnet-version]-[cpu/gpu]-py3`
  + `007439368137.dkr.ecr.us-east-2.amazonaws.com/sagemaker-neo-mxnet:[mxnet-version]-[cpu/gpu]-py3`
  + `802834080501.dkr.ecr.eu-west-1.amazonaws.com/sagemaker-neo-mxnet:[mxnet-version]-[cpu/gpu]-py3`
+ **Pytorch **The Pytorch version used must be in [Pytorch SageMaker Estimators](https://github.com/aws/sagemaker-python-sdk#pytorch-sagemaker-estimators) list\.
  + `301217895009.dkr.ecr.us-west-2.amazonaws.com/sagemaker-neo-pytorch:[pytorch-version]-[cpu/gpu]-py3`
  + `785573368785.dkr.ecr.us-east-1.amazonaws.com/sagemaker-neo-pytorch:[pytorch-version]-[cpu/gpu]-py3`
  + `007439368137.dkr.ecr.us-east-2.amazonaws.com/sagemaker-neo-pytorch:[pytorch-version]-[cpu/gpu]-py3`
  + `802834080501.dkr.ecr.eu-west-1.amazonaws.com/sagemaker-neo-pytorch:[pytorch-version]-[cpu/gpu]-py3`

Also, if you are using **TensorFlow**, **Pytorch**, or **MXNet**, add the following key\-value pair to `PrimaryContainer`/`Environment`:

```
"Environment": {
"SAGEMAKER_SUBMIT_DIRECTORY" : "[Full S3 path for *.tar.gz file containing the training script]"
}
```

The script must be packaged as a `*.tar.gz` file\. The `*.tar.gz` file must contain the training script at the root level\. The script must contain two additional functions for Neo serving containers:
+ `neo_preprocess(payload, content_type)`: Function that takes in the payload and `Content-Type` of each incoming request and returns a NumPy array\.
+ `neo_postprocess(result)`: Function that takes the prediction results produced by Deep Learning Runtime and returns the response body\.

Neither of these two functions use any functionalities of MXNet, Pytorch, or Tensorflow\. See the [Amazon SageMaker Neo Sample Notebooks](neo.md#neo-sample-notebooks) for examples using these functions\.

## Create the Endpoint Configuration \(AWS CLI\)<a name="neo-deployment-hosting-services-cli-create-endpoint-config"></a>

For the full syntax of the `CreateEndpointConfig` API, see [ `CreateEndpointConfig`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpointConfig.html)\. You must specify the correct instance type in `ProductionVariants`/`InstanceType`\. It is imperative that this value matches the instance type specified in your compilation job\.

## Create an Endpoint \(AWS CLI\)<a name="neo-deployment-hosting-services-cli-create-endpoint"></a>

For the full syntax of the `CreateEndpoint` API, see [ `CreateEndpoint`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html)\. 