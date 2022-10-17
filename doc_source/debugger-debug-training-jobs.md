# Debug Training Jobs Using Amazon SageMaker Debugger<a name="debugger-debug-training-jobs"></a>

To prepare your training script and run training jobs with SageMaker Debugger to debug model training progress, you follow the typical two\-step process: modify your training script using the `sagemaker-debugger` Python SDK, and construct a SageMaker estimator using the SageMaker Python SDK\. Go through the following topics to learn how to use SageMaker Debugger's debugging functionality\.

**Topics**
+ [Step 1: Adapt Your Training Script to Register a Hook](debugger-modify-script.md)
+ [Step 2: Launch and Debug Training Jobs Using SageMaker Python SDK](debugger-configuration-for-debugging.md)
+ [Action on Amazon SageMaker Debugger Rules](debugger-action-on-rules.md)
+ [Visualize Amazon SageMaker Debugger Output Tensors in TensorBoard](debugger-enable-tensorboard-summaries.md)