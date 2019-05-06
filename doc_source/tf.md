# Use TensorFlow with Amazon SageMaker<a name="tf"></a>

You can use Amazon SageMaker to train and deploy a model using custom TensorFlow code\. The Amazon SageMaker Python SDK TensorFlow estimators and models and the Amazon SageMaker open\-source TensorFlow containers make writing a TensorFlow script and running it in Amazon SageMaker easier\.

The Amazon SageMaker Python SDK supports two formats for TensorFlow training scripts\. For TensorFlow versions 1\.11 and later, you can use script mode TensorFlow training scripts\. For TensorFlow versions 1\.12 and earlier, you can use legacy mode TensorFlow training scripts\.

For API reference for the Amazon SageMaker Python SDK TensorFlow classes, see [https://sagemaker\.readthedocs\.io/en/stable/sagemaker\.tensorflow\.html](https://sagemaker.readthedocs.io/en/stable/sagemaker.tensorflow.html)\.

For information about how to build and contribute to the Amazon SageMaker TensorFlow container, see the GitHub repository at [https://github\.com/aws/sagemaker\-tensorflow\-container](https://github.com/aws/sagemaker-tensorflow-container)\.

## Use TensorFlow Script Mode<a name="tf-script-mode"></a>

For TensorFlow versions 1\.11 and later, the Amazon SageMaker Python SDK supports script mode training scripts\. Script mode has the following advantages over legacy mode training scripts:
+ Script mode training scripts are more similar to training scripts you write for TensorFlow in general, so it is easier to modify your existing TensorFlow training scripts to work with Amazon SageMaker\.
+ Script mode supports both Python 2\.7\- andPython 3\.6\-compatible source files\.
+ Script mode supports Horovod for distributed training\.

 For information about writing TensorFlow script mode training scripts and using TensorFlow script mode estimators and models with Amazon SageMaker, see [https://github\.com/aws/sagemaker\-python\-sdk\#tensorflow\-sagemaker\-estimators](https://github.com/aws/sagemaker-python-sdk#tensorflow-sagemaker-estimators)\.

For information about TensorFlow versions supported by the Amazon SageMaker TensorFlow container, see [https://github\.com/aws/sagemaker\-python\-sdk/blob/master/src/sagemaker/tensorflow/README\.rst](https://github.com/aws/sagemaker-python-sdk/blob/master/src/sagemaker/tensorflow/README.rst)\.

## Use TensorFlow Legacy Mode<a name="tf-legacy-mode"></a>

Use legacy mode TensorFlow training scripts to run TensorFlow jobs in Amazon SageMaker if:
+ You have existing legacy mode scripts that you do not want to convert to script mode\.
+ You want to use a TensorFlow version earlier than 1\.11\.
+ You need to deploy your model to a Python\-based endpoint\. For information, see [https://github\.com/aws/sagemaker\-python\-sdk/blob/master/src/sagemaker/tensorflow/deploying\_python\.rst](https://github.com/aws/sagemaker-python-sdk/blob/master/src/sagemaker/tensorflow/deploying_python.rst)\.

For information about writing legacy mode TensorFlow scipts to use with the Amazon SageMaker Python SDK, see [https://github\.com/aws/sagemaker\-python\-sdk/tree/v1\.12\.0/src/sagemaker/tensorflow\#tensorflow\-sagemaker\-estimators\-and\-models](https://github.com/aws/sagemaker-python-sdk/tree/v1.12.0/src/sagemaker/tensorflow#tensorflow-sagemaker-estimators-and-models)\.
