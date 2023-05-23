# Use Triton Inference Server with Amazon SageMaker<a name="triton"></a>

SageMaker enables customers to deploy a model using custom code with NVIDIA Triton Inference Server\. This functionality is available through the development of [Triton Inference Server Containers](https://docs.aws.amazon.com/deep-learning-containers/latest/devguide/what-is-dlc.html)\. These containers include NVIDIA Triton Inference Server, support for common ML frameworks, and useful environment variables that let you optimize performance on SageMaker\. For a list of all available Deep Learning Containers images, see [Available Deep Learning Containers Images](https://github.com/aws/deep-learning-containers/blob/master/available_images.md)\. Deep Learning Containers images are maintained and regularly updated with security patches\. 

You can use the Triton Inference Server Container with SageMaker Python SDK as you would any other container in your SageMaker models\. However, using the SageMaker Python SDK is optional\. You can use Triton Inference Server Containers with the AWS CLI and AWS SDK for Python \(Boto3\)\. 

For more information on NVIDIA Triton Inference Server see the [Triton documentation](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/index.html)\.

## Inference<a name="triton-inference"></a>

**Note**  
The Triton Python backend uses shared memory \(SHMEM\) to connect your code to Triton\. SageMaker Inference provides up to half of the instance memory as SHMEM so you can use an instance with more memory for larger SHMEM size\.

For inference, you can use your trained ML models with Triton Inference Server to deploy an inference job with SageMaker\.

Some of the key features of Triton Inference Server Container are:
+ **Support for multiple frameworks**: Triton can be used to deploy models from all major ML frameworks\. Triton supports TensorFlow GraphDef and SavedModel, ONNX, PyTorch TorchScript, TensorRT, and custom Python/C\+\+ model formats\.
+ **Model pipelines**: Triton model ensemble represents a pipeline of one model with pre/post processing logic and the connection of input and output tensors between them\. A single inference request to an ensemble triggers the execution of the entire pipeline\.
+ **Concurrent model execution**: Multiple instances of the same model can run simultaneously on the same GPU or on multiple GPUs\.
+ **Dynamic batching**: For models that support batching, Triton has multiple built\-in scheduling and batching algorithms that combine individual inference requests together to improve inference throughput\. These scheduling and batching decisions are transparent to the client requesting inference\.
+ **Diverse CPU and GPU support**: The models can be executed on CPUs or GPUs for maximum flexibility and to support heterogeneous computing requirements\.

## What do you want to do?<a name="triton-do"></a>

I want to deploy my trained PyTorch model in SageMaker\.  
For a sample Jupyter Notebook, see the [Deploy your PyTorch Resnet50 model with Triton Inference Server example](https://github.com/aws/amazon-sagemaker-examples/blob/master/sagemaker-triton/resnet50/triton_resnet50.ipynb)\.

I want to deploy my trained Hugging Face model in SageMaker\.  
For a sample Jupyter Notebook, see the [Deploy your PyTorch BERT model with Triton Inference Server example](https://github.com/aws/amazon-sagemaker-examples/blob/master/sagemaker-triton/nlp_bert/triton_nlp_bert.ipynb)\.
