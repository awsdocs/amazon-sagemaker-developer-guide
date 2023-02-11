# Optimize model performance using Neo<a name="neo"></a>

Neo is a capability of Amazon SageMaker that enables machine learning models to train once and run anywhere in the cloud and at the edge\. 

If you are a first time user of SageMaker Neo, we recommend you check out the [Getting Started with Edge Devices](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-getting-started-edge.html) section to get step\-by\-step instructions on how to compile and deploy to an edge device\. 

## What is SageMaker Neo?<a name="neo-what-it-is"></a>

Generally, optimizing machine learning models for inference on multiple platforms is difficult because you need to hand\-tune models for the specific hardware and software configuration of each platform\. If you want to get optimal performance for a given workload, you need to know the hardware architecture, instruction set, memory access patterns, and input data shapes, among other factors\. For traditional software development, tools such as compilers and profilers simplify the process\. For machine learning, most tools are specific to the framework or to the hardware\. This forces you into a manual trial\-and\-error process that is unreliable and unproductive\.

Neo automatically optimizes Gluon, Keras, MXNet, PyTorch, TensorFlow, TensorFlow\-Lite, and ONNX models for inference on Android, Linux, and Windows machines based on processors from Ambarella, ARM, Intel, Nvidia, NXP, Qualcomm, Texas Instruments, and Xilinx\. Neo is tested with computer vision models available in the model zoos across the frameworks\. SageMaker Neo supports compilation and deployment for two main platforms: cloud instances \(including Inferentia\) and edge devices\.

For more information about supported frameworks and cloud instance types you can deploy to, see [Supported Instance Types and Frameworks](neo-supported-cloud.md) for cloud instances\.

For more information about supported frameworks, edge devices, operating systems, chip architectures, and common machine learning models tested by SageMaker Neo for edge devices, see [Supported Frameworks, Devices, Systems, and Architectures](neo-supported-devices-edge.md) for edge devices\.

## How it Works<a name="neo-how-it-works"></a>

Neo consists of a compiler and a runtime\. First, the Neo compilation API reads models exported from various frameworks\. It converts the framework\-specific functions and operations into a framework\-agnostic intermediate representation\. Next, it performs a series of optimizations\. Then it generates binary code for the optimized operations, writes them to a shared object library, and saves the model definition and parameters into separate files\. Neo also provides a runtime for each target platform that loads and executes the compiled model\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/neo/neo_how_it_works.png)

You can create a Neo compilation job from either the SageMaker console, the AWS Command Line Interface \(AWS CLI\), a Python notebook, or the SageMaker SDK\.For information on how to compile a model, see [Use Neo to Compile a Model](neo-job-compilation.md)\. With a few CLI commands, an API invocation, or a few clicks, you can convert a model for your chosen platform\. You can deploy the model to a SageMaker endpoint or on an AWS IoT Greengrass device quickly\.

Neo can optimize models with parameters either in FP32 or quantized to INT8 or FP16 bit\-width\.

## Neo Sample Notebooks<a name="neo-sample-notebooks"></a>

For sample notebooks that use SageMaker Neo to train, compile, optimize, and deploy machine learning models to make inferences, see: 
+ [MNIST Training, Compilation and Deployment with MXNet Module](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker_neo_compilation_jobs/mxnet_mnist/mxnet_mnist_neo.html)
+ [MNIST Training, Compilation and Deployment with Tensorflow Module](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker_neo_compilation_jobs/tensorflow_distributed_mnist/tensorflow_distributed_mnist_neo.html)
+ [Deploying pre\-trained PyTorch vision models with SageMaker Neo](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker_neo_compilation_jobs/pytorch_torchvision/pytorch_torchvision_neo.html)
+ [Model Optimization with an Image Classification Example](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker_neo_compilation_jobs/imageclassification_caltech/Image-classification-fulltraining-highlevel-neo.html)
+ [Model Optimization with XGBoost Example](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker_neo_compilation_jobs/xgboost_customer_churn/xgboost_customer_churn_neo.html)

For instructions on how to run these example notebooks in SageMaker, see [Example Notebooks](howitworks-nbexamples.md)\. If you need instructions on how to create a notebook instance to run these examples, see [Amazon SageMaker Notebook Instances](nbi.md)\. To navigate to the relevant example in your notebook instance, choose the **Amazon SageMaker Examples** tab to see a list of all of the SageMaker samples\. To open a notebook, choose its **Use** tab, then choose **Create copy**\.