# Amazon SageMaker Inference Recommender<a name="inference-recommender"></a>

Amazon SageMaker Inference Recommender is a new capability of Amazon SageMaker that reduces the time required to get machine learning \(ML\) models in production by automating load testing and model tuning across SageMaker ML instances\. You can use Inference Recommender to deploy your model to a real\-time inference endpoint that delivers the best performance at the lowest cost\. Inference Recommender helps you select the best instance type and configuration \(such as instance count, container parameters, and model optimizations\) for your ML models and workloads\.

Amazon SageMaker Inference Recommender only charges you for the instances used while your jobs are executing\.

## How it Works<a name="inference-recommender-how-it-works"></a>

To use Amazon SageMaker Inference Recommender, you must first register a model to SageMaker model registry with your model artifact and container, use AWS SDK for Python \(Boto3\) to benchmark different SageMaker endpoint configurations, collect metrics, and visualize metrics across performance and resource utilization to help you decide on which instance type to choose\.

## How to Get Started<a name="inference-recommender-get-started"></a>

If you are a first\-time user of Amazon SageMaker Inference Recommender, we recommend that you do the following:

1. Read through the [Prerequisites](inference-recommender-prerequisites.md) section to make sure you have satisfied the requirements to use Amazon SageMaker Inference Recommender\.

1. Read through the [Recommendation jobs](inference-recommender-recommendation-jobs.md) section to launch your first Inference Recommender recommendation jobs\.

1. Explore the introductory Amazon SageMaker Inference Recommender [Jupyter notebook](https://github.com/aws/amazon-sagemaker-examples/blob/master/sagemaker-inference-recommender/inference-recommender.ipynb) example, or review the example notebooks in the following section\.

## Example notebooks<a name="inference-recommender-notebooks"></a>

The following example Jupyter notebooks can help you with the workflows for multiple use cases in Inference Recommender:
+ If you want an introductory notebook that benchmarks a TensorFlow model, see the [SageMaker Inference Recommender TensorFlow](https://github.com/aws/amazon-sagemaker-examples/blob/main/sagemaker-inference-recommender/inference-recommender.ipynb) notebook\.
+ If you want to benchmark a HuggingFace model, see the [SageMaker Inference Recommender for HuggingFace](https://github.com/aws/amazon-sagemaker-examples/blob/main/sagemaker-inference-recommender/huggingface-inference-recommender/huggingface-inference-recommender.ipynb) notebook\.
+ If you want to benchmark an XGBoost model, see the [SageMaker Inference Recommender XGBoost](https://github.com/aws/amazon-sagemaker-examples/blob/main/sagemaker-inference-recommender/xgboost/xgboost-inference-recommender.ipynb) notebook\.
+ If you want to review CloudWatch metrics for your Inference Recommender jobs, see the [SageMaker Inference Recommender CloudWatch metrics](https://github.com/aws/amazon-sagemaker-examples/blob/main/sagemaker-inference-recommender/tensorflow-cloudwatch/tf-cloudwatch-inference-recommender.ipynb) notebook\.