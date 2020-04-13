# Compile and Deploy Models with Amazon SageMaker Neo<a name="neo"></a>

Neo is a new capability of Amazon SageMaker that enables machine learning models to train once and run anywhere in the cloud and at the edge\.

Ordinarily, optimizing machine learning models for inference on multiple platforms is extremely difficult because you need to hand\-tune models for the specific hardware and software configuration of each platform\. If you want to get optimal performance for a given workload, you need to know the hardware architecture, instruction set, memory access patterns, and input data shapes among other factors\. For traditional software development, tools such as compilers and profilers simplify the process\. For machine learning, most tools are specific to the framework or to the hardware\. This forces you into a manual trial\-and\-error process that is unreliable and unproductive\.

Neo eliminates the time and effort required to do this by automatically optimizing TensorFlow, Apache MXNet, PyTorch, ONNX, and XGBoost models for deployment on ARM, Intel, and Nvidia processors\. Neo consists of a compiler and a runtime\. First, the Neo compilation API reads models exported from various frameworks\. It converts the framework\-specific functions and operations into a framework\-agnostic intermediate representation\. Next, it performs a series of optimizations\. Then it generates binary code for the optimized operations, writes them to a shared object library, and saves the model definition and parameters into separate files\. Neo also provides a runtime for each target platform that loads and executes the compiled model\.

You can create a Neo compilation job from either the Amazon SageMaker console, AWS Command Line Interface \(AWS CLI\), Python notebook, or the Amazon SageMaker SDK\. With a few CLI commands, an API invocation, or a few clicks, you can convert a model for your chosen platform\. You can deploy the model to an Amazon SageMaker endpoint or on an AWS IoT Greengrass device quickly\. Amazon SageMaker provides Neo container images for Amazon SageMaker XGBoost and Image Classification models, and supports Amazon SageMaker\-compatible containers for your own compiled models\.

**Note**  
Neo currently supports image classification models exported as frozen graphs from TensorFlow, MXNet, or PyTorch, and XGBoost models\. Neo is available in the following AWS [Regions](https://docs.aws.amazon.com/general/latest/gr/rande.html#sagemaker_region) where Amazon SageMaker is supported:   
+ **Asia Pacific** \(Hong Kong, Mumbai, Seoul, Singapore, Sydney, Tokyo\)
+ **Canada** \(Central\)
+ **EU** \(Frankfurt, Ireland, London, Paris, Stockholm\)
+ **North America** \(N\. Virginia, Ohio, Oregon, N\. California\)
+ **South America** \(Sao Paulo\)

**Topics**
+ [Amazon SageMaker Neo Sample Notebooks](#neo-sample-notebooks)
+ [Use Neo to Compile a Model](neo-job-compilation.md)
+ [Deploy a Model](neo-deployment.md)
+ [Request Inferences from a Deployed Service](neo-requests.md)
+ [Troubleshooting Neo Compilation Errors](neo-troubleshooting.md)

## Amazon SageMaker Neo Sample Notebooks<a name="neo-sample-notebooks"></a>

For sample notebooks that uses Amazon SageMaker Neo to train, compile, optimize, and deploy machine learning models to make inferences, see: 
+ [MNIST Training, Compilation and Deployment with MXNet Module](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker_neo_compilation_jobs/mxnet_mnist/mxnet_mnist_neo.ipynb)
+ [MNIST Training, Compilation and Deployment with Tensorflow Module](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker_neo_compilation_jobs/tensorflow_distributed_mnist/tensorflow_distributed_mnist_neo.ipynb)
+ [Deploying pre\-trained PyTorch vision models with Amazon SageMaker Neo ](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker_neo_compilation_jobs/pytorch_torchvision/pytorch_torchvision_neo.ipynb)
+ [Model Optimization with an Image Classification Example](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker_neo_compilation_jobs/imageclassification_caltech/Image-classification-fulltraining-highlevel-neo.ipynb)
+ [Model Optimization with XGBoost Example ](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker_neo_compilation_jobs/xgboost_customer_churn/xgboost_customer_churn_neo.ipynb)


For instructions on how to run these example notebooks in Amazon SageMaker, see [Use Example Notebooks](howitworks-nbexamples.md)\. If you need instructions on how to create a notebook instance to run these examples, see Amazon SageMaker, see [Use Amazon SageMaker Notebook Instances](nbi.md)\. To navigate to the relevant example in your notebook instance, choose the **Amazon SageMaker Examples** tab to see a list of all of the Amazon SageMaker samples\. To open a notebook, choose its **Use** tab, then choose **Create copy**\.
