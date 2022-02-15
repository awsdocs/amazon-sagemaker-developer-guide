# Deploy a Model in Amazon SageMaker<a name="how-it-works-deployment"></a>

After you train your machine learning model, you can deploy it using Amazon SageMaker to get predictions in any of the following ways, depending on your use case:
+ For persistent, real\-time endpoints that make one prediction at a time, use SageMaker real\-time hosting services\. See [Real\-time inference](realtime-endpoints.md)\.
+ Workloads that have idle periods between traffic spurts and can tolerate cold starts, use Serverless Inference\. See [Serverless Inference \(Preview\)](serverless-endpoints.md)\.
+ Requests with large payload sizes up to 1GB, long processing times, and near real\-time latency requirements, use Amazon SageMaker Asynchronous Inference\. See [Asynchronous inference](async-inference.md)\.
+ To get predictions for an entire dataset, use SageMaker batch transform\. See [Use Batch Transform](batch-transform.md)\.

SageMaker also provides features to manage resources and optimize inference performance when deploying machine learning models:
+ To manage models on edge devices so that you can optimize, secure, monitor, and maintain machine learning models on fleets of edge devices such as smart cameras, robots, personal computers, and mobile devices, see [Deploy models at the edge with SageMaker Edge Manager](edge.md)\.
+ To optimize Gluon, Keras, MXNet, PyTorch, TensorFlow, TensorFlow\-Lite, and ONNX models for inference on Android, Linux, and Windows machines based on processors from Ambarella, ARM, Intel, Nvidia, NXP, Qualcomm, Texas Instruments, and Xilinx, see [Optimize model performance using Neo](neo.md)\.

For more information about all deployment options, see [Deploy Models for Inference](deploy-model.md)\.