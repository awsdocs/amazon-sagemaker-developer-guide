# Using Apache MXNet with Amazon SageMaker<a name="mxnet"></a>

You can use Amazon SageMaker to train a model using your own custom Apache MXNet training code\. If you choose to use Amazon SageMaker hosting services, you can also provide your own custom Apache MXNet inference code\. This section provides guidelines for writing custom Apache MXNet code for both model training and inference and an example that includes sample Apache MXNet code and instructions for model training and deployment\. 

For information about Apache MXNet supported versions, see [https://github\.com/aws/sagemaker\-python\-sdk\#mxnet\-sagemaker\-estimators](https://github.com/aws/sagemaker-python-sdk#mxnet-sagemaker-estimators)\. The container source code can be found at the GitHub repository at [https://github\.com/aws/sagemaker\-mxnet\-containers](https://github.com/aws/sagemaker-mxnet-containers)\. 

**Topics**
+ [Writing Custom Apache MXNet Model Training and Inference Code](mxnet-training-inference-code-template.md)
+ [Examples: Using Amazon SageMaker with Apache MXNet](sagemaker-examples.md)