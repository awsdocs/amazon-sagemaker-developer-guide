# Use TensorFlow with Amazon SageMaker<a name="tf"></a>

You can use Amazon SageMaker to train and deploy a model using custom TensorFlow code\. The SageMaker Python SDK TensorFlow estimators and models and the SageMaker open\-source TensorFlow containers make writing a TensorFlow script and running it in SageMaker easier\.

## Use TensorFlow Version 1\.11 and Later<a name="tf-script-mode"></a>

For TensorFlow versions 1\.11 and later, the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) supports script mode training scripts\.

### What do you want to do?<a name="tf-intent"></a>

I want to train a custom TensorFlow model in SageMaker\.  
For a sample Jupyter notebook, see [TensorFlow script mode training and serving](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker-python-sdk/tensorflow_script_mode_training_and_serving/tensorflow_script_mode_training_and_serving.ipynb)\.  
For documentation, see [Train a Model with TensorFlow](https://sagemaker.readthedocs.io/en/stable/using_tf.html#train-a-model-with-tensorflow)\.

I have a TensorFlow model that I trained in SageMaker, and I want to deploy it to a hosted endpoint\.  
For more information, see [Deploy TensorFlow Serving models](https://sagemaker.readthedocs.io/en/stable/using_tf.html#deploy-tensorflow-serving-models)\.

I have a TensorFlow model that I trained outside of SageMaker, and I want to deploy it to a SageMaker endpoint  
For more information, see [Deploying directly from model artifacts](https://sagemaker.readthedocs.io/en/stable/using_tf.html#deploying-directly-from-model-artifacts)\.

I want to see the API documentation for [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) TensorFlow classes\.  
For more information, see [TensorFlow Estimator](https://sagemaker.readthedocs.io/en/stable/sagemaker.tensorflow.html)\.

I want to find the SageMaker TensorFlow container repository\.  
For more information, see [SageMaker TensorFlow Container GitHub repository](https://github.com/aws/sagemaker-tensorflow-container)\.

I want to find information about TensorFlow versions supported by AWS Deep Learning Containers\.  
For more information, see [Available Deep Learning Container Images](https://github.com/aws/deep-learning-containers/blob/master/available_images.md)\.

 For general information about writing TensorFlow script mode training scripts and using TensorFlow script mode estimators and models with SageMaker, see [Using TensorFlow with the SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/using_tf.html)\.

## Use TensorFlow Legacy Mode for Versions 1\.11 and Earlier<a name="tf-legacy-mode"></a>

The [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) provides a legacy mode that supports TensorFlow versions 1\.11 and earlier\. Use legacy mode TensorFlow training scripts to run TensorFlow jobs in SageMaker if:
+ You have existing legacy mode scripts that you do not want to convert to script mode\.
+ You want to use a TensorFlow version earlier than 1\.11\.

For information about writing legacy mode TensorFlow scripts to use with the SageMaker Python SDK, see [TensorFlow SageMaker Estimators and Models](https://github.com/aws/sagemaker-python-sdk/tree/v1.12.0/src/sagemaker/tensorflow#tensorflow-sagemaker-estimators-and-models)\.