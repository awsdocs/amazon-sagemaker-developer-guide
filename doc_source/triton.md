# Use Triton Inference Server with Amazon SageMaker<a name="triton"></a>

SageMaker enables customers to deploy a model using custom code with NVIDIA Triton Inference Server\. This functionality is available through the development of [Triton Inference Server Containers](https://docs.aws.amazon.com/deep-learning-containers/latest/devguide/what-is-dlc.html)\. These containers include NVIDIA Triton Inference Server, support for common ML frameworks, and useful environment variables that let you optimize performance on SageMaker\. For a list of all available Deep Learning Containers images, see [Available Deep Learning Containers Images](https://github.com/aws/deep-learning-containers/blob/master/available_images.md)\. Deep Learning Containers images are maintained and regularly updated with security patches\. 

You can use the Triton Inference Server Container with SageMaker Python SDK as you would any other container in your SageMaker models\. However, using the SageMaker Python SDK is optional\. You can use Triton Inference Server Containers with the AWS CLI and AWS SDK for Python \(Boto3\)\. 

For more information on NVIDIA Triton Inference Server see the [Triton documentation](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/index.html)\.

## Inference<a name="triton-inference"></a>

For inference, you can use your trained ML models with Triton Inference Server to deploy an inference job with SageMaker\.

Some of the key features of Triton Inference Server Container are:
+ **Support for multiple frameworks**: Triton can be used to deploy models from all major ML frameworks\. Triton supports TensorFlow GraphDef and SavedModel, ONNX, PyTorch TorchScript, TensorRT, and custom Python/C\+\+ model formats\.
+ **Model pipelines**: Triton model ensemble represents a pipeline of one model with pre/post processing logic and the connection of input and output tensors between them\. A single inference request to an ensemble triggers the execution of the entire pipeline\.
+ **Concurrent model execution**: Multiple instances of the same model can run simultaneously on the same GPU or on multiple GPUs\.
+ **Dynamic batching**: For models that support batching, Triton has multiple built\-in scheduling and batching algorithms that combine individual inference requests together to improve inference throughput\. These scheduling and batching decisions are transparent to the client requesting inference\.
+ **Diverse CPU and GPU support**: The models can be executed on CPUs or GPUs for maximum flexibility and to support heterogeneous computing requirements\.