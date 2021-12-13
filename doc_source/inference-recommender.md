# Amazon SageMaker Inference Recommender<a name="inference-recommender"></a>

Amazon SageMaker Inference Recommender is a new capability of Amazon SageMaker that reduces the time required to get machine learning \(ML\) models in production by automating load testing and model tuning across SageMaker ML instances\. You can use Inference Recommender to deploy your model to a real\-time inference endpoint that delivers the best performance at the lowest cost\. Inference Recommender helps you select the best instance type and configuration \(such as instance count, container parameters, and model optimizations\) for your ML models and workloads\.

Amazon SageMaker Inference Recommender only charges you for the instances used while your jobs are executing\. For more information, see [Pricing](inference-recommender-pricing.md)\.

## How it Works<a name="inference-recommender-how-it-works"></a>

To use Amazon SageMaker Inference Recommender, you must first register a model to SageMaker model registry with your model artifact and container, use AWS SDK for Python \(Boto3\) to benchmark different SageMaker endpoint configurations, collect metrics, and visualize metrics across performance and resource utilization to help you decide on which instance type to choose\.

## How to Get Started<a name="inference-recommender-get-started"></a>

If you are a first\-time user of Amazon SageMaker Inference Recommender, we recommend that you do the following:

1. Read through the [Prerequisites](inference-recommender-prerequisites.md) section to make sure you have satisfied the requirements to use Amazon SageMaker Inference Recommender\.

1. Read through the [Recommendation Jobs](inference-recommender-recommendation-jobs.md) section to launch your first Inference Recommender recommendation jobs\.

1. Explore the Amazon SageMaker Inference Recommender [Jupyter notebook](https://github.com/aws/amazon-sagemaker-examples/blob/master/sagemaker-inference-recommender/inference-recommender.ipynb) example\.