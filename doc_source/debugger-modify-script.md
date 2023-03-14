# Step 1: Adapt Your Training Script to Register a Hook<a name="debugger-modify-script"></a>

Amazon SageMaker Debugger comes with a client library called the [`sagemaker-debugger` Python SDK](https://sagemaker-debugger.readthedocs.io/en/website)\. The `sagemaker-debugger` Python SDK provides tools for adapting your training script before training and analysis tools after training\. In this page, you'll learn how to adapt your training script using the client library\. 

The `sagemaker-debugger` Python SDK provides wrapper functions that help register a hook to extract model tensors, without altering your training script\. To get started with collecting model output tensors and debug them to find training issues, make the following modifications in your training script\.

**Tip**  
While you're following this page, use the [`sagemaker-debugger` open source SDK documentation](https://sagemaker-debugger.readthedocs.io/en/website/index.html) for API references\.

**Topics**
+ [Adapt Your PyTorch Training Script](debugger-modify-script-pytorch.md)
+ [Adapt Your TensorFlow Training Script](debugger-modify-script-tensorflow.md)