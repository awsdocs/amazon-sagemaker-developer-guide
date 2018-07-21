# Using TensorFlow with Amazon SageMaker<a name="tf"></a>

You can use Amazon SageMaker to train a model using custom TensorFlow code\. If you choose to deploy your code using Amazon SageMaker hosting services, you can also provide custom TensorFlow inference code\. This section provides guidelines for writing custom TensorFlow code for both model training and inference, and an example that includes sample TensorFlow code and instructions for model training and deployment\.

Amazon SageMaker supports using pipe mode in TensorFlow containers\. For information, see [https://github\.com/aws/sagemaker\-tensorflow\-extensions/blob/master/README\.rst](https://github.com/aws/sagemaker-tensorflow-extensions/blob/master/README.rst)

For information about TensorFlow supported versions, see [https://github\.com/aws/sagemaker\-python\-sdk\#tensorflow\-sagemaker\-estimators](https://github.com/aws/sagemaker-python-sdk#tensorflow-sagemaker-estimators)\. The container source code can be found at the GitHub repository at [https://github\.com/aws/sagemaker\-tensorflow\-containers](https://github.com/aws/sagemaker-tensorflow-containers)\.

**Topics**
+ [Writing Custom TensorFlow Model Training and Inference Code](tf-training-inference-code-template.md)
+ [Examples: Using Amazon SageMaker with TensorFlow](tf-examples.md)