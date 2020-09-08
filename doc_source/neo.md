# Compile and Deploy Models with Amazon SageMaker Neo<a name="neo"></a>

Neo is a capability of Amazon SageMaker that enables machine learning models to train once and run anywhere in the cloud and at the edge\.

Ordinarily, optimizing machine learning models for inference on multiple platforms is extremely difficult because you need to hand\-tune models for the specific hardware and software configuration of each platform\. If you want to get optimal performance for a given workload, you need to know the hardware architecture, instruction set, memory access patterns, and input data shapes among other factors\. For traditional software development, tools such as compilers and profilers simplify the process\. For machine learning, most tools are specific to the framework or to the hardware\. This forces you into a manual trial\-and\-error process that is unreliable and unproductive\.

Neo automatically optimizes Gluon, Keras, MXNet, PyTorch, TensorFlow, TensorFlow\-Lite, and ONNX models for inference on Android, Linux, and Windows machines based on processors from Ambarella, ARM, Intel, Nvidia, NXP, Qualcomm, Texas Instruments, and Xilinx\. Neo is tested with computer vision models available in the model zoos across the frameworks\. 

Neo can optimize models with parameters either in FP32 or quantized to INT8 or FP16 bit\-width\. Neo consists of a compiler and a runtime\. First, the Neo compilation API reads models exported from various frameworks\. It converts the framework\-specific functions and operations into a framework\-agnostic intermediate representation\. Next, it performs a series of optimizations\. Then it generates binary code for the optimized operations, writes them to a shared object library, and saves the model definition and parameters into separate files\. Neo also provides a runtime for each target platform that loads and executes the compiled model\.

You can create a Neo compilation job from either the SageMaker console, AWS Command Line Interface \(AWS CLI\), Python notebook, or the SageMaker SDK\. With a few CLI commands, an API invocation, or a few clicks, you can convert a model for your chosen platform\. You can deploy the model to an SageMaker endpoint or on an AWS IoT Greengrass device quickly\. SageMaker provides Neo container images for SageMaker XGBoost and Image Classification models, and supports SageMaker\-compatible containers for your own compiled models\.

## Neo Available Regions, Frameworks, and Operators<a name="neo-supported"></a>

### Neo Available Regions<a name="neo-supported-regions"></a>

Neo is available in the following [AWS Service Regions](https://docs.aws.amazon.com/general/latest/gr/rande.html#sagemaker_region) where SageMaker is supported: 
+ **Asia Pacific** \(Hong Kong, Mumbai, Seoul, Singapore, Sydney, Tokyo\)
+ **Canada** \(Central\)
+ **China** \(Beijing, Ningxia\)
+ **EU** \(Frankfurt, Ireland, London, Paris, Stockholm\)
+ **Middle East** \(Bahrain\)
+ **North America** \(N\. Virginia, Ohio, Oregon, N\. California\)
+ **South America** \(Sao Paulo\)
+ **AWS GovCloud** \(US\-Gov\-West\)

### Neo\-supported Frameworks and Operators<a name="neo-supported-frame-op"></a>

The image classification model files need to be formatted as a tar file \(tar\.gz\) that includes additional files that depend on the type of framework\.

To find look\-up tables of Neo\-supported frameworks, operators, devices, and platforms, see the following pages:
+ [SageMaker Neo\-supported Frameworks and Operators](https://aws.amazon.com/releasenotes/sagemaker-neo-supported-frameworks-and-operators/)
+ [SageMaker Neo\-supported Devices and Platforms](https://aws.amazon.com/releasenotes/sagemaker-neo-supported-target-platforms-and-compiler-options/)

Neo supports the following deep learning frameworks:
+ **[TensorFlow](https://aws.amazon.com/tensorflow/)**: Neo supports saved models and frozen models\.

  For *saved models*, Neo expects one PB \(\.pb\) or one PBTXT \(\.pbtxt\) file and a variables directory that contains variables\. 

  For *frozen models*, Neo expect only one PB \(\.pb\) or PBTXT \(\.pbtxt\) file\.
+ **[Keras](https://keras.io/)**: Neo expects one H5 file \(\.h5\) containing the model definition\.
+ **[PyTorch](https://pytorch.org/)**: Neo expects one PTH file \(\.pth\) containing the model definition\.
+ **[MXNET](https://aws.amazon.com/mxnet/)**: Neo expects one symbol file \(\.json\) and one parameter file \(\.params\)\.
+ **[XGBoost](https://github.com/dmlc/xgboost)**: Neo expects one XGBoost model file \(\.model\) where the number of nodes in a tree can't exceed 2^31\.
+ **[ONNX](https://github.com/onnx/onnx)**: Neo expects one ONNX file \(\.onnx\)\.
+ **[TFLite](https://www.tensorflow.org/lite/guide/get_started)**: Neo expects one NHWC file \(\.tflite\)\.

**Topics**
+ [Neo Available Regions, Frameworks, and Operators](#neo-supported)
+ [Neo Model Compilation Sample Notebooks](#neo-sample-notebooks)
+ [Use Neo to Compile a Model](neo-job-compilation.md)
+ [Deploy a Model](neo-deployment.md)
+ [Request Inferences from a Deployed Service](neo-requests.md)
+ [Troubleshooting Neo Compilation Errors](neo-troubleshooting.md)

## Neo Model Compilation Sample Notebooks<a name="neo-sample-notebooks"></a>

For sample notebooks that uses SageMaker Neo to train, compile, optimize, and deploy machine learning models to make inferences, see: 
+ [MNIST Training, Compilation and Deployment with MXNet Module](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker_neo_compilation_jobs/mxnet_mnist/mxnet_mnist_neo.ipynb)
+ [MNIST Training, Compilation and Deployment with Tensorflow Module](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker_neo_compilation_jobs/tensorflow_distributed_mnist/tensorflow_distributed_mnist_neo.ipynb)
+ [Deploying pre\-trained PyTorch vision models with SageMaker Neo ](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker_neo_compilation_jobs/pytorch_torchvision/pytorch_torchvision_neo.ipynb)
+ [Model Optimization with an Image Classification Example](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker_neo_compilation_jobs/imageclassification_caltech/Image-classification-fulltraining-highlevel-neo.ipynb)
+ [Model Optimization with XGBoost Example ](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker_neo_compilation_jobs/xgboost_customer_churn/xgboost_customer_churn_neo.ipynb)

For instructions on how to run these example notebooks in SageMaker, see [Example Notebooks](howitworks-nbexamples.md)\. If you need instructions on how to create a notebook instance to run these examples, see SageMaker, see [Use Amazon SageMaker Notebook Instances](nbi.md)\. To navigate to the relevant example in your notebook instance, choose the **Amazon SageMaker Examples** tab to see a list of all of the SageMaker samples\. To open a notebook, choose its **Use** tab, then choose **Create copy**\.