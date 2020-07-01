# Develop Algorithms and Models in Amazon SageMaker<a name="sagemaker-marketplace-develop"></a>

Before you can create algorithm and model package resources to use in Amazon SageMaker or list on AWS Marketplace, you have to develop them and package them in Docker containers\.

**Note**  
When algorithms and model packages are created for listing on AWS Marketplace, Amazon SageMaker scans the containers for security vulnerabilities on supported operating systems\.   
Only the following operating system versions are supported:  
Debian: 6\.0, 7, 8, 9, 10
Ubuntu: 12\.04, 12\.10, 13\.04, 14\.04, 14\.10, 15\.04, 15\.10, 16\.04, 16\.10, 17\.04, 17\.10, 18\.04, 18\.10
CentOS: 5, 6, 7
Oracle Linux: 5, 6, 7
Alpine: 3\.3, 3\.4, 3\.5
Amazon Linux

**Topics**
+ [Develop Algorithms in Amazon SageMaker](#sagmeaker-mkt-develop-algo)
+ [Develop Models in Amazon SageMaker](#sagemaker-mkt-develop-model)

## Develop Algorithms in Amazon SageMaker<a name="sagmeaker-mkt-develop-algo"></a>

An algorithm should be packaged as a docker container and stored in Amazon ECR to use it in Amazon SageMaker\. The Docker container contains the training code used to run training jobs and, optionally, the inference code used to get inferences from models trained by using the algorithm\.

For information about developing algorithms in Amazon SageMaker and packaging them as containers, see [Use Your Own Algorithms or Models with Amazon SageMaker ](your-algorithms.md)\. For a complete example of how to create an algorithm container, see the sample notebook at [https://github\.com/awslabs/amazon\-sagemaker\-examples/blob/master/advanced\_functionality/scikit\_bring\_your\_own/scikit\_bring\_your\_own\.ipynb](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/advanced_functionality/scikit_bring_your_own/scikit_bring_your_own.ipynb)\. You can also find the sample notebook in an Amazon SageMaker notebook instance\. The notebook is in the **Advanced Functionality** section, and is named `scikit_bring_your_own.ipynb`\. For information about using the sample notebooks in a notebook instance, see [Use Example Notebooks](howitworks-nbexamples.md)\.

Always thoroughly test your algorithms before you create algorithm resources to publish on AWS Marketplace\.

**Note**  
When a buyer subscribes to your containerized product, the Docker containers run in an isolated \(internet\-free\) environment\. When you create your containers, do not rely on making outgoing calls over the internet\. Calls to AWS services are also not allowed\.

## Develop Models in Amazon SageMaker<a name="sagemaker-mkt-develop-model"></a>

A deployable model in Amazon SageMaker consists of inference code, model artifacts, an IAM role that is used to access resources, and other information required to deploy the model in Amazon SageMaker\. Model artifacts are the results of training a model by using a machine learning algorithm\. The inference code must be packaged in a Docker container and stored in Amazon ECR\. You can either package the model artifacts in the same container as the inference code, or store them in Amazon S3\. 

You create a model by running a training job in Amazon SageMaker, or by training a machine learning algorithm outside of Amazon SageMaker\. If you run a training job in Amazon SageMaker, the resulting model artifacts are available in the `ModelArtifacts` field in the response to a call to the [DescribeTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeTrainingJob.html) operation\. For information about how to develop an Amazon SageMaker model container, see [Use Your Own Inference Code](your-algorithms-inference-main.md)\. For a complete example of how to create a model container from a model trained outside of Amazon SageMaker, see the sample notebook at [https://github\.com/awslabs/amazon\-sagemaker\-examples/blob/master/advanced\_functionality/xgboost\_bring\_your\_own\_model/xgboost\_bring\_your\_own\_model\.ipynb](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/advanced_functionality/xgboost_bring_your_own_model/xgboost_bring_your_own_model.ipynb)\. You can also find the sample notebook in an Amazon SageMaker notebook instance\. The notebook is in the **Advanced Functionality** section, and is named `xgboost_bring_your_own_model.ipynb`\. For information about using the sample notebooks in a notebook instance, see [Use Example Notebooks](howitworks-nbexamples.md)\.

Always thoroughly test your models before you create model packages to publish on AWS Marketplace\.

**Note**  
When a buyer subscribes to your containerized product, the Docker containers run in an isolated \(internet\-free\) environment\. When you create your containers, do not rely on making outgoing calls over the internet\. Calls to AWS services are also not allowed\.