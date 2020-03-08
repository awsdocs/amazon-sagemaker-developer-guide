# Known Limitations with Amazon SageMaker Debugger<a name="debugger-known-limitations"></a>

Here are the known limitations for Amazon SageMaker Debugger:
+ **TensorFlow support**:It does not support TensorFlow 2\.0\.
+ **Horovod support**: It does not support training on jobs that use distributed training with the TensorFlow Horovod container\. \(Distributed training with Horovod is supported for MXNet and PyTorch\.\)
+ **Distributed training**: Parameter server\-based distributed training is not supported for MXNet and TensorFlow\.