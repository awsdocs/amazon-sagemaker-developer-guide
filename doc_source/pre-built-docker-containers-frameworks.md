# Prebuilt Amazon SageMaker Docker Images for Scikit\-learn and Spark ML<a name="pre-built-docker-containers-frameworks"></a>

Amazon SageMaker provides prebuilt Docker images that install the scikit\-learn and Spark ML libraries and the dependencies they need to build Docker images that are compatible with SageMaker using the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\. With the SDK, you can use scikit\-learn for machine learning tasks and use Spark ML to create and tune machine learning pipelines\. For instructions on installing and using the SDK, see [SageMaker Python SDK](https://github.com/aws/sagemaker-python-sdk#installing-the-sagemaker-python-sdk)\. The following table contains links to the GitHub repositories with the source code and the Dockerfiles for scikit\-learn and Spark ML frameworks and to instructions that show how use the Python SDK estimators to run your own training algorithms on SageMaker Learner and your own models on SageMaker Hosting\.


| Library | Prebuilt Docker Image Source Code | Instructions | 
| --- | --- | --- | 
| scikit\-learn |  [SageMaker Scikit\-learn Containers](https://github.com/aws/sagemaker-scikit-learn-container)  |  [Using Scikit\-learn with the Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/using_sklearn.html)  | 
| Spark ML |  [SageMaker Spark ML Serving Containers](https://github.com/aws/sagemaker-sparkml-serving-container)  |  [SparkML Python SDK Documentation](https://sagemaker.readthedocs.io/en/stable/sagemaker.sparkml.html)  | 

If you are not using the SM Python SDK and one of its estimators to manage the container, you have to retrieve the relevant pre\-build container\. The SageMaker prebuilt Docker images are stored in Amazon Elastic Container Registry \(Amazon ECR\)\. You can push or pull them using their fullname registry addresses\. SageMaker uses the following Docker Image URI patterns for scikit\-learn and Spark ML:
+ `<ACCOUNT_ID>.dkr.ecr.<REGION_NAME>.amazonaws.com/sagemaker-scikit-learn:<SCIKIT-LEARN_VERSION>-cpu-py<PYTHON_VERSION>`

  For example, `746614075791.dkr.ecr.us-west-1.amazonaws.com/sagemaker-scikit-learn:0.23-1-cpu-py3`
+ `<ACCOUNT_ID>.dkr.ecr.<REGION_NAME>.amazonaws.com/sagemaker-sparkml-serving:<SPARK-ML_VERSION>`

  For example, `341280168497.dkr.ecr.ca-central-1.amazonaws.com/sagemaker-sparkml-serving:2.4`

The following table lists the supported values for account IDs and corresponding AWS Region names\.


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

A full list of the Spark ML image URIs are also available at the [Using the Docker image for performing inference with SageMaker](https://github.com/aws/sagemaker-sparkml-serving-container#using-the-docker-image-for-performing-inference-with-sagemaker) markdown page in the [SageMaker Spark ML Serving Container GitHub repository](https://github.com/aws/sagemaker-sparkml-serving-container#using-the-docker-image-for-performing-inference-with-sagemaker)\.

## Finding the Images<a name="pre-built-docker-containers-frameworks-finding"></a>

To find out which versions of the image are available, use the following AWS CLI command\. For example, use the following command to find the `sagemaker-scikit-learn` image repository names in the `us-east-1` region:

```
aws ecr describe-images --region us-east-1 --registry-id 683313688378 --repository-name sagemaker-scikit-learn
```

SageMaker also provides prebuilt Docker images for popular deep learning frameworks\. For information about Docker images that enable using deep learning frameworks in SageMaker, see [Prebuilt Amazon SageMaker Docker Images for TensorFlow, MXNet, Chainer, and PyTorch](pre-built-containers-frameworks-deep-learning.md)\.

For information on Docker images for developing reinforcement learning \(RL\) solutions in SageMaker, see [SageMaker RL Containers](https://github.com/aws/sagemaker-rl-container)\.