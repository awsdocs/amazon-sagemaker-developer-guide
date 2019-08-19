# Use TensorFlow with Amazon SageMaker<a name="tf"></a>

You can use Amazon SageMaker to train and deploy a model using custom TensorFlow code\. The Amazon SageMaker Python SDK TensorFlow estimators and models and the Amazon SageMaker open\-source TensorFlow containers make writing a TensorFlow script and running it in Amazon SageMaker easier\.

## Use TensorFlow Version 1\.11 and Later<a name="tf-script-mode"></a>

For TensorFlow versions 1\.11 and later, the Amazon SageMaker Python SDK supports script mode training scripts\.

What do you want to do?

I want to train a custom TensorFlow model in Amazon SageMaker\.  
For a sample Jupyter notebook, see [https://github\.com/awslabs/amazon\-sagemaker\-examples/tree/master/sagemaker\-python\-sdk/tensorflow\_distributed\_mnist](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-python-sdk/tensorflow_distributed_mnist)\.  
For documentation, see [Train a Model with TensorFlow](https://sagemaker.readthedocs.io/en/stable/using_tf.html#train-a-model-with-tensorflow)\.

I have a TensorFlow model that I trained in Amazon SageMaker, and I want to deploy it to a hosted endpoint\.  
[Deploy TensorFlow Serving models](https://sagemaker.readthedocs.io/en/stable/using_tf.html#deploy-tensorflow-serving-models)\.

I have a TensorFlow model that I trained outside of Amazon SageMaker, and I want to deploy it to an Amazon SageMaker endpoint  
[Deploying directly from model artifacts](https://sagemaker.readthedocs.io/en/stable/using_tf.html#deploying-directly-from-model-artifacts)\.

I want to see the API documentation for Amazon SageMaker Python SDK TensorFlow classes\.  
[TensorFlow Estimator](https://sagemaker.readthedocs.io/en/stable/sagemaker.tensorflow.html)

I want to see information about Amazon SageMaker TensorFlow containers\.  
[https://github\.com/aws/sagemaker\-tensorflow\-container](https://github.com/aws/sagemaker-tensorflow-container)\.

 For general information about writing TensorFlow script mode training scripts and using TensorFlow script mode estimators and models with Amazon SageMaker, see [Using TensorFlow with the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/using_tf.html)\.

For information about TensorFlow versions supported by the Amazon SageMaker TensorFlow container, see [https://github\.com/aws/sagemaker\-python\-sdk/blob/master/src/sagemaker/tensorflow/README\.rst](https://github.com/aws/sagemaker-python-sdk/blob/master/src/sagemaker/tensorflow/README.rst)\.

## Use TensorFlow Legacy Mode for Versions 1\.11 and Earlier<a name="tf-legacy-mode"></a>

The Amazon SageMaker Python SDK provides a legacy mode that supports TensorFlow versions 1\.11 and earlier\. Use legacy mode TensorFlow training scripts to run TensorFlow jobs in Amazon SageMaker if:
+ You have existing legacy mode scripts that you do not want to convert to script mode\.
+ You want to use a TensorFlow version earlier than 1\.11\.

For information about writing legacy mode TensorFlow scipts to use with the Amazon SageMaker Python SDK, see [https://github\.com/aws/sagemaker\-python\-sdk/tree/v1\.12\.0/src/sagemaker/tensorflow\#tensorflow\-sagemaker\-estimators\-and\-models](https://github.com/aws/sagemaker-python-sdk/tree/v1.12.0/src/sagemaker/tensorflow#tensorflow-sagemaker-estimators-and-models)\.