# SageMaker Training Compiler Troubleshooting<a name="training-compiler-troubleshooting"></a>

If you run into an error, you can use the following list to try to troubleshoot your training job\. If you need further support, reach out to the SageMaker team through [AWS Support](https://console.aws.amazon.com/support/) or [AWS Developer Forums for Amazon SageMaker](https://forums.aws.amazon.com/forum.jspa?forumID=285)\.

## Training Job Fails Due to Missing XLA Configuration<a name="training-compiler-troubleshooting-missing-xla-config"></a>

If the training job fails with the following error message, `Missing XLA configuration`, this might be due to a misconfiguration in sizing the device for distributed training with XLA\. Use one of the following options to solve the XLA configuration error: 
+ **Option 1** – Add a parameter for the number of GPUs to the `hyperparameters` dictionary in the estimator\. By specifying this, you can set the environment variable, `GPU_NUM_DEVICES` , which is required by a training script\. 

  ```
  # Using the SageMaker Python SDK's HuggingFace estimator
  # assuming the training script updates the environment variable
  # GPU_NUM_DEVICES accordingly
  
  hf_estimator=HuggingFace(
      ...
      hyperparameters={`
          ...
          "n_gpus": 4
      }
  )
  ```
**Note**  
The preceding estimator shows an example of how to specify the number of GPUs through the `n_gpus` parameter\. Make sure the parameter name matches the one used in the parser function of your training script\.
+ **Option 2** – Modify your training script where you specify the number of GPUs you want to use\.

  ```
  # In your training script,
  # simply add the following code at the top.
  
  num_gpus=4 # Specify the number of GPUs manually, if needed
  
  if os.getenv("SM_NUM_GPUS")==None:
      print(
          "Explicitly specifying the number of GPUs."
      )
      os.environ["GPU_NUM_DEVICES"]=num_gpus
  else:
      os.environ["GPU_NUM_DEVICES"]=os.environ["SM_NUM_GPUS"]
  ```

## Incorrect API Uses for PyTorch without Hugging Face Trainer API<a name="training-compiler-troubleshooting-incorrect-api-use"></a>

PyTorch/XLA defines a set of APIs to replace some of the existing PyTorch training APIs\. Failing to use them properly leads PyTorch training to fail\.
+ One of the most typical errors when compiling a PyTorch model is due to a wrong device type for operators and tensors\. To properly compile a PyTorch model, make sure you use XLA devices \([https://pytorch.org/xla/release/1.9/index.html](https://pytorch.org/xla/release/1.9/index.html)\) instead of using CUDA or mixing CUDA devices and XLA devices\.
+ `mark_step()` is a barrier just for XLA\. Failing to set it correctly causes a training job to stall\. 
+ PyTorch/XLA provides additional distributed training APIs\. Failing to program the APIs properly causes gradients to be collected incorrectly, which causes a training convergence failure\.

To properly set up your PyTorch script and avoid the aforementioned incorrect API uses, see [Using PyTorch without Hugging Face Trainer API](training-compiler-pytorch-models.md#training-compiler-pytorch-models-non-trainer) and [Best Practices to Enable SageMaker Training Compiler for PyTorch without the Hugging Face Trainer API](training-compiler-pytorch-models.md#training-compiler-pytorch-models-best-practices)\.