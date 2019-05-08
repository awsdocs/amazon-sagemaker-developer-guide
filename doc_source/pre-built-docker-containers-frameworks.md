# Pre\-built Amazon SageMaker Docker Images for Scikit\-learn and Spark ML<a name="pre-built-docker-containers-frameworks"></a>

Amazon SageMaker provides *pre\-built Dockerfiles* that install the scikit\-learn and Spark ML frameworks and the dependencies they need to build SageMaker\-compatible Docker images using the Amazon SageMaker Python SDK\. With the SDK, you can use scikit\-learn for machine learning tasks and Spark ML to create and tune machine learning pipelines\. For instructions on installing and using the SDK, see [SageMaker Python SDK](https://github.com/aws/sagemaker-python-sdk#installing-the-sagemaker-python-sdk)\. The following table contains links to the github repositories with the pre\-built Dockerfiles for scikit\-learn and Spark ML frameworks, and to instructions that show how use them\.


| Framework | Pre\-built Dockerfiles | Instructions | 
| --- | --- | --- | 
| Scikit\-learn |  [SageMaker Scikit\-learn Containers](https://github.com/aws/sagemaker-scikit-learn-container)  |  [Using Scikit\-learn with the SageMaker Python](https://sagemaker.readthedocs.io/en/stable/using_sklearn.html)  | 
| Spark ML |  [SageMaker SparkML Serving Containers](https://github.com/aws/sagemaker-sparkml-serving-container)  |  [SparkML Serving](https://sagemaker.readthedocs.io/en/stable/sagemaker.sparkml.html)  | 

The Amazon SageMaker pre\-built Docker images are stored in the Amazon ECR\. They be pushed or pulled using their fullname registry addresses\. Here are the Docker Image URL Patterns used by Amazon SageMaker for scikit\-learn and Spark ML:
+ `<ACCOUNT_ID>.dkr.ecr.<REGION_NAME>.amazonaws.com/sagemaker-scikit-learn`

  For example, `746614075791.dkr.ecr.us-west-1.amazonaws.com/sagemaker-scikit-learn`
+ `<ACCOUNT_ID>.dkr.ecr.<REGION_NAME>.amazonaws.com/sagemaker-sparkml-serving`

  For example, `341280168497.dkr.ecr.ca-central-1.amazonaws.com/sagemaker-sparkml-serving`

Here are the supported values for the account IDs and region names are provided in the following table\.


| ACCOUNT\_ID | REGION\_NAME | 
| --- | --- | 
| 746614075791 | us\-west\-1 | 
| 246618743249 | us\-west\-2 | 
| 683313688378 | us\-east\-1 | 
| 257758044811 | us\-east\-2 | 
| 354813040037 | ap\-northeast\-1 | 
| 366743142698 | ap\-northeast\-2 | 
| 121021644041 | ap\-southeast\-1 | 
| 783357654285 | ap\-southeast\-2 | 
| 720646828776 | ap\-south\-1 | 
| 141502667606 | eu\-west\-1 | 
| 764974769150 | eu\-west\-2 | 
| 492215442770 | eu\-central\-1 | 
| 341280168497 | ca\-central\-1 | 
| 414596584902 | us\-gov\-west\-1 | 

These supported values itemized in the table are also provided on the [fw\_registry\.py](https://github.com/aws/sagemaker-python-sdk/blob/master/src/sagemaker/fw_registry.py) page\.

Amazon SageMaker also provides pre\-built Docker images for popular deep learning frameworks\. For information on Docker images that enable deep learning frameworks to be used in Amazon SageMaker, see [Pre\-built Amazon SageMaker Docker Images for Tensorflow, MXNet, Chainer,and PyTorch](pre-built-docker-containers-frameworks-deep-learning.md)\.

For information on Docker images that enable Reinforcement Learning \(RL\) solutions to be used in Amazon SageMaker, see [Amazon SageMaker RL Containers](https://github.com/aws/sagemaker-rl-container)\.