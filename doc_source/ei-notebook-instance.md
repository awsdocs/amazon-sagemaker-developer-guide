# Attach EI to a Notebook Instance<a name="ei-notebook-instance"></a>

To test and evaluate inference performance using EI, you can attach EI to a notebook instance when you create or update a notebook instance\. You can then use EI in local mode to host a model at an endpoint hosted on the notebook instance\. You should test various sizes of notebook instances and EI accelerators to evaluate the configuration that works best for your use case\.

## Set Up to Use EI<a name="ei-notebook-instance-console"></a>

To use EI locally in a notebook instance, create a notebook instance with an EI instance\.

**To create a notebook instance with an EI instance**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. In the navigation pane, choose **Notebook instances**\.

1. Choose **Create notebook instance**\.

1. For **Notebook instance name**, provide a unique name for your notebook instance\.

1. For **notebook instance type**, choose a CPU instance such as ml\.t2\.medium\.

1. For **Elastic Inference \(EI\)**, choose an instance from the list, such as **ml\.eia2\.medium**\.

1. For **IAM role**, choose an IAM role that has the required permissions to use SageMaker and EI\.

1. \(Optional\) For **VPC \- Optional**, if you want the notebook instance to use a VPC, choose one from the available list\. Otherwise, leave it as **No VPC**\. If you use a VPC follow the instructions at [Use a Custom VPC to Connect to EI](ei-setup.md#ei-setup-custom-vpc)\.

1. \(Optional\) For **Lifecycle configuration \- optional**, either leave it as **No configuration** or choose a lifecycle configuration\. For more information, see [Customize a Notebook Instance Using a Lifecycle Configuration Script](notebook-lifecycle-config.md)\.

1. \(Optional\) For **Encryption key \- optional**, Optional\) If you want SageMaker to use an AWS Key Management Service \(AWS KMS\) key to encrypt data in the ML storage volume attached to the notebook instance, specify the key\.

1. \(Optional\) For **Volume Size In GB \- optional**, leave the default value of 5\.

1. \(Optional\) For **Tags**, add tags to the notebook instance\. A tag is a label you assign to help manage your notebook instances\. A tag consists of a key and a value, both of which you define\.

1. Choose **Create Notebook Instance**\.

After you create your notebook instance with EI attached, you can create a Jupyter notebook and set up an EI endpoint that is hosted locally on the notebook instance\.

**Topics**
+ [Set Up to Use EI](#ei-notebook-instance-console)
+ [Use EI in Local Mode in SageMaker](#ei-notebook-instance-local)

## Use EI in Local Mode in SageMaker<a name="ei-notebook-instance-local"></a>

To use EI locally in an endpoint hosted on a notebook instance, use local mode with the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) versions of either the TensorFlow, MXNet, or PyTorch estimators or models\. For more information about local mode support in the SageMaker Python SDK, see [https://github\.com/aws/sagemaker\-python\-sdk\#sagemaker\-python\-sdk\-overview](https://github.com/aws/sagemaker-python-sdk#sagemaker-python-sdk-overview)\.

**Topics**
+ [Use EI in Local Mode with SageMaker TensorFlow Estimators and Models](#ei-notebook-instance-local-tensorflow)
+ [Use EI in Local Mode with SageMaker Apache MXNet Estimators and Models](#ei-notebook-instance-local-mxnet)
+ [Use EI in Local Mode with SageMaker PyTorch Estimators and Models](#ei-notebook-instance-local-pytorch)

### Use EI in Local Mode with SageMaker TensorFlow Estimators and Models<a name="ei-notebook-instance-local-tensorflow"></a>

To use EI with TensorFlow in local mode, specify `local` for `instance_type` and `local_sagemaker_notebook` for `accelerator_type` when you call the `deploy` method of an estimator or a model object\. For more information about [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) TensorFlow estimators and models, see [https://sagemaker\.readthedocs\.io/en/stable/frameworks/tensorflow/index\.html](https://sagemaker.readthedocs.io/en/stable/frameworks/tensorflow/index.html)\.

The following code shows how to use local mode with an estimator object\. To call the `deploy` method, you must have previously either:
+ Trained the model by calling the `fit` method of an estimator\.
+ Pass a model artifact when you initialize the model object\.

```
# Deploys the model to a local endpoint
tf_predictor = tf_model.deploy(initial_instance_count=1,
                               instance_type='local',
                               accelerator_type='local_sagemaker_notebook')
```

### Use EI in Local Mode with SageMaker Apache MXNet Estimators and Models<a name="ei-notebook-instance-local-mxnet"></a>

To use EI with MXNet in local mode, specify `local` for `instance_type` and `local_sagemaker_notebook` for `accelerator_type` when you call the `deploy` method of an estimator or a model object\. For more information about [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) MXNet estimators and models, see [https://sagemaker\.readthedocs\.io/en/stable/frameworks/mxnet/index\.html](https://sagemaker.readthedocs.io/en/stable/frameworks/mxnet/index.html)\. 

The following code shows how to use local mode with an estimator object\. You must have previously called the `fit` method of the estimator to train the model\.

```
# Deploys the model to a local endpoint
mxnet_predictor = mxnet_estimator.deploy(initial_instance_count=1,
                                         instance_type='local',
                                         accelerator_type='local_sagemaker_notebook')
```

For a complete example of using EI in local mode with MXNet, see the sample notebook at [https://github\.com/awslabs/amazon\-sagemaker\-examples/blob/master/sagemaker\-python\-sdk/mxnet\_mnist/mxnet\_mnist\_elastic\_inference\_local\.ipynb ](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker-python-sdk/mxnet_mnist/mxnet_mnist_elastic_inference_local.ipynb)\.

### Use EI in Local Mode with SageMaker PyTorch Estimators and Models<a name="ei-notebook-instance-local-pytorch"></a>

To use EI with PyTorch in local mode, when you call the `deploy` method of an estimator or a model object, specify `local` for `instance_type` and `local_sagemaker_notebook` for `accelerator_type`\. For more information about [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) PyTorch estimators and models, see [SageMaker PyTorch Estimators and Models ](https://sagemaker.readthedocs.io/en/stable/frameworks/pytorch/index.html)\. 

The following code shows how to use local mode with an estimator object\. You must have previously called the `fit` method of the estimator to train the model\.

```
# Deploys the model to a local endpoint
pytorch_predictor = pytorch_estimator.deploy(initial_instance_count=1,
                                             instance_type='local',
                                             accelerator_type='local_sagemaker_notebook')
```