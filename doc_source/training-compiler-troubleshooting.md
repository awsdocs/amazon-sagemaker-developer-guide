# SageMaker Training Compiler Troubleshooting<a name="training-compiler-troubleshooting"></a>

If you run into an error, you can use the following list to try to troubleshoot your training job\. If you need further support, reach out to the SageMaker team through [AWS Support](https://console.aws.amazon.com/support/) or [AWS Developer Forums for Amazon SageMaker](https://forums.aws.amazon.com/forum.jspa?forumID=285)\.

## Training Job Fails Due to Missing XLA Configuration<a name="training-compiler-troubleshooting-missing-xla-config"></a>

If a training job fails with the `Missing XLA configuration` error message, it might be due to a misconfiguration in the number of GPUs per instance that you use\.

XLA requires additional environment variables to compile the training job\. The most common missing environment variable is `GPU_NUM_DEVICES`\. For the compiler to work properly, you must set this environment variable equal to the number of GPUs per instance\.

There are three approaches to set the `GPU_NUM_DEVICES` environment variable:
+ **Approach 1** – Use the `environment` argument of the SageMaker estimator class\. For example, if you use an `ml.p3.8xlarge` instance that has four GPUs, do the following:

  ```
  # Using the SageMaker Python SDK's HuggingFace estimator
  
  hf_estimator=HuggingFace(
      ...
      instance_type="ml.p3.8xlarge",
      hyperparameters={...},
      environment={
          ...
          "GPU_NUM_DEVICES": "4" # corresponds to number of GPUs on the specified instance
      },
  )
  ```
+ **Approach 2** – Use the `hyperparameters` argument of the SageMaker estimator class and parse it in your training script\.

  1. To specify the number of GPUs, add a key\-value pair to the `hyperparameters` argument\.

     For example, if you use an `ml.p3.8xlarge` instance that has four GPUs, do the following:

     ```
     # Using the SageMaker Python SDK's HuggingFace estimator
     
     hf_estimator=HuggingFace(
         ...
         entry_point = "train.py"
         instance_type= "ml.p3.8xlarge",
         hyperparameters = {
             ...
             "n_gpus": 4 # corresponds to number of GPUs on specified instance
         }
     )
     hf_estimator.fit()
     ```

  1. In your training script, parse the `n_gpus` hyperparameter and specify it as an input for the `GPU_NUM_DEVICES` environment variable\.

     ```
     # train.py
     import os, argparse
     
     if __name__ == "__main__":
         parser = argparse.ArgumentParser()
         ...
         # Data, model, and output directories
         parser.add_argument("--output_data_dir", type=str, default=os.environ["SM_OUTPUT_DATA_DIR"])
         parser.add_argument("--model_dir", type=str, default=os.environ["SM_MODEL_DIR"])
         parser.add_argument("--training_dir", type=str, default=os.environ["SM_CHANNEL_TRAIN"])
         parser.add_argument("--test_dir", type=str, default=os.environ["SM_CHANNEL_TEST"])
         parser.add_argument("--n_gpus", type=str, default=os.environ["SM_NUM_GPUS"])
     
         args, _ = parser.parse_known_args()
     
         os.environ["GPU_NUM_DEVICES"] = args.n_gpus
     ```
+ **Approach 3** – Hard\-code the `GPU_NUM_DEVICES` environment variable in your training script\. For example, add the following to your script if you use an instance that has four GPUs\.

  ```
  # train.py
  
  import os
  os.environ["GPU_NUM_DEVICES"] = 4
  ```

**Tip**  
To find the number of GPU devices on machine learning instances that you want to use, see [Accelerated Computing](https://aws.amazon.com/ec2/instance-types/#Accelerated_Computing) in the *Amazon EC2 Instance Types page*\. 

## Incorrect API Uses for PyTorch without Hugging Face Trainer API<a name="training-compiler-troubleshooting-incorrect-api-use"></a>

PyTorch/XLA defines a set of APIs to replace some of the existing PyTorch training APIs\. Failing to use them properly leads PyTorch training to fail\.
+ One of the most typical errors when compiling a PyTorch model is due to a wrong device type for operators and tensors\. To properly compile a PyTorch model, make sure you use XLA devices \([https://pytorch.org/xla/release/1.9/index.html](https://pytorch.org/xla/release/1.9/index.html)\) instead of using CUDA or mixing CUDA devices and XLA devices\.
+ `mark_step()` is a barrier just for XLA\. Failing to set it correctly causes a training job to stall\. 
+ PyTorch/XLA provides additional distributed training APIs\. Failing to program the APIs properly causes gradients to be collected incorrectly, which causes a training convergence failure\.

To properly set up your PyTorch script and avoid the aforementioned incorrect API uses, see [Using PyTorch without Hugging Face Trainer API](training-compiler-pytorch-models.md#training-compiler-pytorch-models-non-trainer) and [Best Practices to Enable SageMaker Training Compiler for PyTorch without the Hugging Face Trainer API](training-compiler-pytorch-models.md#training-compiler-pytorch-models-best-practices)\.