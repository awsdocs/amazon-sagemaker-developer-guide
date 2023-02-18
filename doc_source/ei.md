# Use Amazon SageMaker Elastic Inference \(EI\)<a name="ei"></a>

Starting April 15, 2023, AWS will not onboard new customers to Amazon Elastic Inference \(EI\), and will help current customers migrate their workloads to options that offer better price and performance\. After April 15, 2023, new customers will not be able to launch instances with Amazon EI accelerators in Amazon SageMaker, Amazon ECS, or Amazon EC2\. However, customers who have used Amazon EI at least once during the past 30\-day period are considered current customers and will be able to continue using the service\.

Machine learning \(ML\) on AWS helps you innovate faster with the most comprehensive set of ML services and infrastructure made available in a low\-cost, pay as\-you\-go usage model\. AWS continuously delivers better performing and lower cost infrastructure for ML inference workloads\. AWS launched Amazon Elastic Inference \(EI\) in 2018 to enable customers to attach low\-cost GPU\-powered acceleration to Amazon EC2, Amazon SageMaker instances, or Amazon Elastic Container Service \(ECS\) tasks to reduce the cost of running deep learning inference by up to 75% compared to standalone GPU based instances such as Amazon EC2 P4d and Amazon EC2 G5\. In 2019, AWS launched AWS Inferentia, Amazon's first custom silicon designed to accelerate deep learning workloads by providing high performance inference in the cloud\. Amazon EC2 Inf1 instances based on AWS Inferentia chips deliver up 2\.3x higher throughput and up to 70% lower cost per inference than comparable current generation GPU\-based Amazon EC2 instances\. With the availability of new accelerated compute options such as AWS Inferentia and Amazon EC2 G5 instances, the benefit of attaching a fractional GPU to a CPU host instance using Amazon EI has diminished\. For example, customers hosting models on Amazon EI who move to `ml.inf1.xlarge` instances can get up to 56% in cost savings and 2x performance improvement\.

Customers can use Amazon SageMaker Inference Recommender to help them choose the best alternative instances to Amazon EI for deploying their ML models\.

**Frequently asked questions**

1. **Why is Amazon encouraging customers to move workloads from Amazon Elastic Inference \(EI\) to newer hardware acceleration options such as AWS Inferentia?**

   Customers get better performance at a much better price than Amazon EI with new hardware accelerator options such as [AWS Inferentia](http://aws.amazon.com/machine-learning/inferentia/) for their inference workloads\. AWS Inferentia is designed to provide high performance inference in the cloud, to drive down the total cost of inference, and to make it easy for developers to integrate machine learning into their business applications\. To enable customers to benefit from such newer generation hardware accelerators, we will not onboard new customers to Amazon EI after April 15, 2023\.

1. **Which AWS services are impacted by the move to stop onboarding new customers to Amazon Elastic Inference \(EI\)?**

   This announcement will affect Amazon EI accelerators attached to any Amazon EC2, Amazon SageMaker instances, or Amazon Elastic Container Service \(ECS\) tasks\. In Amazon SageMaker, this applies to both endpoints and notebook kernels using Amazon EI accelerators\.

1. **Will I be able to create a new Amazon Elastic Inference \(EI\) accelerator after April 15, 2023?**

   No, if you are a new customer and have not used Amazon EI in the past 30 days, then you will not be able create a new Amazon EI instance in your AWS account after April 15, 2023\. However, if you have used an Amazon EI accelerator at least once in the past 30 days, you can attach a new Amazon EI accelerator to your instance\.

1. **We currently use Amazon Elastic Inference \(EI\) accelerators\. Will we be able to continue using them after April 15, 2023?**

   Yes, you will be able use Amazon EI accelerators\. We recommend that you migrate your current ML Inference workloads running on Amazon EI to other hardware accelerator options at your earliest convenience\. 

1. **How do I evaluate alternative instance options for my current Amazon SageMaker Inference Endpoints?**

   [Amazon SageMaker Inference Recommender](https://docs.aws.amazon.com/sagemaker/latest/dg/inference-recommender.html) can help you identify cost\-effective deployments to migrate existing workloads from Amazon Elastic Inference \(EI\) to an appropriate ML instance supported by SageMaker\.

1. **How do I change the instance type for my existing endpoint in Amazon SageMaker?**

   You can change the instance type for your existing endpoint by doing the following:

   1. First, [create a new EndpointConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpointConfig.html) that uses the new instance type\. If you have an autoscaling policy, [delete the existing autoscaling policy](https://docs.aws.amazon.com/sagemaker/latest/dg/endpoint-auto-scaling-delete.html)\.

   1. Call [UpdateEndpoint](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateEndpoint.html) while specifying your newly created EndpointConfig\.

   1. Wait for your endpoint to change status to `InService`\. This will take approximately 10\-15 minutes\.

   1. Finally, if you need autoscaling for your new endpoint, create a new autoscaling policy for this new endpoint and ProductionVariant\.

1. **How do I change the instance type for my existing [Amazon SageMaker Notebook Instance](https://docs.aws.amazon.com/sagemaker/latest/dg/nbi.html) using Amazon Elastic Inference \(EI\)?**

   Choose **Notebook instances** in the SageMaker console, and then choose the Notebook Instance you want to update\. Make sure the Notebook Instance has a `Stopped` status\. Finally, you can choose **Edit** and change your instance type\. Make sure that, when your Notebook Instance starts up, you select the right kernel for your new instance\.

1. **Is there a specific instance type which is a good alternative to Amazon Elastic Inference \(EI\)?**

   Every machine learning workload is unique\. We recommend using [Amazon SageMaker Inference Recommender](https://docs.aws.amazon.com/sagemaker/latest/dg/inference-recommender.html) to help you identify the right instance type for your ML workload, performance requirements, and budget\. [AWS Inferentia](http://aws.amazon.com/machine-learning/inferentia/), specifically `inf1.xlarge`, is the best high performance and low\-cost alternative for Amazon EI customers\.

**Topics**
+ [How EI Works](#ei-how-it-works)
+ [Choose an EI Accelerator Type](#ei-choose-type)
+ [Use EI in a SageMaker Notebook Instance](#ei-intro-notebook)
+ [Use EI on a Hosted Endpoint](#ei-intro-endpoint)
+ [Frameworks that Support EI](#ei-supported-frameworks)
+ [Use EI with SageMaker Built\-in Algorithms](#ei-built-in)
+ [EI Sample Notebooks](#ei-intro-sample-nb)
+ [Set Up to Use EI](ei-setup.md)
+ [Attach EI to a Notebook Instance](ei-notebook-instance.md)
+ [Use EI on Amazon SageMaker Hosted Endpoints](ei-endpoints.md)

## How EI Works<a name="ei-how-it-works"></a>

Amazon Elastic Inference accelerators are network attached devices that work along with SageMaker instances in your endpoint to accelerate your inference calls\. Elastic Inference accelerates inference by allowing you to attach fractional GPUs to any SageMaker instance\. You can select the client instance to run your application and attach an Elastic Inference accelerator to use the right amount of GPU acceleration for your inference needs\. Elastic Inference helps you lower your cost when not fully utilizing your GPU instance for inference\. We recommend trying Elastic Inference with your model using different CPU instances and accelerator sizes\.

The following EI accelerator types are available\. You can configure your endpoints or notebook instances with any EI accelerator type\.

In the table, the throughput in teraflops \(TFLOPS\) is listed for both single\-precision floating\-point \(F32\) and half\-precision floating\-point \(F16\) operations\. The memory in GB is also listed\.


| Accelerator Type | F32 Throughput in TFLOPS | F16 Throughput in TFLOPS | Memory in GB | 
| --- | --- | --- | --- | 
| ml\.eia2\.medium | 1 | 8 | 2 | 
| ml\.eia2\.large | 2 | 16 | 4 | 
| ml\.eia2\.xlarge | 4 | 32 | 8 | 
| ml\.eia1\.medium | 1 | 8 | 1 | 
| ml\.eia1\.large | 2 | 16 | 2 | 
| ml\.eia1\.xlarge | 4 | 32 | 4 | 

## Choose an EI Accelerator Type<a name="ei-choose-type"></a>

Consider the following factors when choosing an accelerator type for a hosted model:
+ Models, input tensors and batch sizes influence the amount of accelerator memory you need\. Start with an accelerator type that provides at least as much memory as the file size of your trained model\. Factor in that a model might use significantly more memory than the file size at runtime\.
+ Demands on CPU compute resources, main system memory, and GPU\-based acceleration and accelerator memory vary significantly between different kinds of deep learning models\. The latency and throughput requirements of the application also determine the amount of compute and acceleration you need\. Thoroughly test different configurations of instance types and EI accelerator sizes to make sure you choose the configuration that best fits the performance needs of your application\.

For more information on selecting an EI accelerator, see:
+ [Amazon Elastic Inference Overview ](https://aws.amazon.com/machine-learning/elastic-inference/)
+ [Choosing an Instance and Accelerator Type for Your Model](https://docs.aws.amazon.com/elastic-inference/latest/developerguide/before.html#getting-started-choosing)
+ [Optimizing costs in Amazon Elastic Inference with TensorFlow](https://aws.amazon.com/blogs/machine-learning/optimizing-costs-in-amazon-elastic-inference-with-amazon-tensorflow/)

## Use EI in a SageMaker Notebook Instance<a name="ei-intro-notebook"></a>

Typically, you build and test machine learning models in a SageMaker notebook before you deploy them for production\. You can attach EI to your notebook instance when you create the notebook instance\. You can set up an endpoint that is hosted locally on the notebook instance by using the local mode supported by TensorFlow, MXNet, and PyTorch estimators and models in the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) to test inference performance\. Elastic Inference enabled PyTorch is not currently supported on notebook instances\. For instructions on how to attach EI to a notebook instance and set up a local endpoint for inference, see [Attach EI to a Notebook Instance](ei-notebook-instance.md)\. There are also Elastic Inference\-enabled SageMaker Notebook Jupyter kernels for Elastic Inference\-enabled versions of TensorFlow and Apache MXNet\. For information about using SageMaker notebook instances, see [Use Amazon SageMaker Notebook Instances](https://docs.aws.amazon.com/sagemaker/latest/dg/nbi.html)

## Use EI on a Hosted Endpoint<a name="ei-intro-endpoint"></a>

When you are ready to deploy your model for production to provide inferences, you create a SageMaker hosted endpoint\. You can attach EI to the instance where your endpoint is hosted to increase its performance at providing inferences\. For instructions on how to attach EI to a hosted endpoint instance, see [Use EI on Amazon SageMaker Hosted Endpoints](ei-endpoints.md)\.

## Frameworks that Support EI<a name="ei-supported-frameworks"></a>

Amazon Elastic Inference is designed to be used with AWS enhanced versions of TensorFlow, Apache MXNet, or PyTorch machine learning frameworks\. These enhanced versions of the frameworks are automatically built into containers when you use the Amazon SageMaker Python SDK, or you can download them as binary files and import them in your own Docker containers\. 

You can download the EI\-enabled TensorFlow binary files from the public [amazonei\-tensorflow](https://console.aws.amazon.com/s3/buckets/amazonei-tensorflow) Amazon S3 bucket to the TensorFlow serving containers\. For more information about building a container that uses the EI\-enabled version of TensorFlow, see [ Amazon Elastic Inference with TensorFlow in SageMaker](https://github.com/aws/sagemaker-tensorflow-serving-container#sagemaker-tensorflow-serving-container)\.

You can download the EI\-enabled MXNet binary files from the public [amazonei\-apachemxnet](https://console.aws.amazon.com/s3/buckets/amazonei-apachemxnet) Amazon S3 bucket to the MXNet serving containers\. For more information about building a container that uses the EI\-enabled version of MXNet, see [ Amazon Elastic Inference with MXNet in SageMaker](https://github.com/aws/sagemaker-mxnet-serving-container#amazon-elastic-inference-with-mxnet-in-sagemaker)\.

You can download the EI\-enabled PyTorch binary files from the public [amazonei\-pytorch](https://console.aws.amazon.com/s3/buckets/amazonei-pytorch) Amazon S3 bucket to the PyTorch serving containers\. For more information about building a container that uses the EI\-enabled version of PyTorch, see [ Amazon Elastic Inference with PyTorch in SageMaker](https://github.com/aws/sagemaker-pytorch-serving-container/#amazon-elastic-inference-with-pytorch-in-sagemaker)\.

To use Elastic Inference in a hosted endpoint, you can choose any of the following frameworks depending on your needs\.
+ [ SageMaker Python SDK \- Deploy TensorFlow models ](https://sagemaker.readthedocs.io/en/stable/frameworks/tensorflow/using_tf.html#deploy-tensorflow-serving-models)
+ [ SageMaker Python SDK \- Deploy MXNet models ](https://sagemaker.readthedocs.io/en/stable/frameworks/mxnet/using_mxnet.html#deploy-mxnet-models)
+ [ SageMaker Python SDK \- Deploy PyTorch models ](https://sagemaker.readthedocs.io/en/stable/frameworks/pytorch/using_pytorch.html#deploy-pytorch-models)

If you need to create a custom container for deploying your model that is complex and requires extensions to a framework that the SageMaker pre\-built containers do not support, use [ the low\-level AWS SDK for Python \(Boto 3\) ](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/ec2-example-managing-instances.html)\.

## Use EI with SageMaker Built\-in Algorithms<a name="ei-built-in"></a>

Currently, the [Image Classification \- MXNet](image-classification.md) and [Object Detection \- MXNet](object-detection.md) built\-in algorithms support EI\. For an example that uses the Image Classification algorithm with EI, see [End\-to\-End Multiclass Image Classification Example](https://sagemaker-examples.readthedocs.io/en/latest/introduction_to_amazon_algorithms/imageclassification_caltech/Image-classification-fulltraining.html)\.

## EI Sample Notebooks<a name="ei-intro-sample-nb"></a>

The following Sample notebooks provide examples of using EI in SageMaker:
+ [Using Amazon Elastic Inference with MXNet on Amazon SageMaker ](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-python-sdk/mxnet_mnist/mxnet_mnist_elastic_inference.html)
+ [Using Amazon Elastic Inference with MXNet on an Amazon SageMaker Notebook Instance ](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-python-sdk/mxnet_mnist/mxnet_mnist_elastic_inference_local.html) 
+ [Using Amazon Elastic Inference with Neo\-compiled TensorFlow model on SageMaker](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-python-sdk/tensorflow_serving_using_elastic_inference_with_your_own_model/tensorflow_neo_compiled_model_elastic_inference.html)
+ [Using Amazon Elastic Inference with a pre\-trained TensorFlow Serving model on SageMaker](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-python-sdk/tensorflow_serving_using_elastic_inference_with_your_own_model/tensorflow_serving_pretrained_model_elastic_inference.html)