# SageMaker Training Compiler Best Practices<a name="training-compiler-tips-pitfalls"></a>

Review the following tips and pitfalls when using SageMaker Training Compiler\.
+ If you are not getting any performance improvement after enabling SageMaker Training Compiler, make sure you chose the right instance type, `batch_size`, and `learning_rate` for your training job\. SageMaker Training Compiler is only tested on GPU instances \(the `ml.p3` instances and `ml.p4` instances\)\. For more information, see [Tested Models](training-compiler-support.md#training-compiler-tested-models)\.
+ If you bring a PyTorch model, make sure you use PyTorch/XLA's model save function to properly checkpoint your model\. For more information about the function, see [https://pytorch.org/xla/release/1.9/index.html#torch_xla.core.xla_model.save](https://pytorch.org/xla/release/1.9/index.html#torch_xla.core.xla_model.save) in the *PyTorch on XLA Devices documentation*\. 

  To learn how to add the modifications to your PyTorch script, see [Using PyTorch without Hugging Face Trainer API](training-compiler-pytorch-models.md#training-compiler-pytorch-models-non-trainer)\.

  For more information about the actual application of using the model save function, see [Checkpoint Writing and Loading](https://huggingface.co/blog/pytorch-xla#checkpoint-writing-and-loading) in the *Hugging Face on PyTorch/XLA TPUs: Faster and cheaper training blog*\.
+ To debug the compiler\-accelerated training job, enable the `debug` flag in the `compiler_config` parameter\. This enables SageMaker to put the debugging logs into SageMaker training job logs\.

  ```
  huggingface_estimator=HuggingFace(
      ...
      compiler_config=TrainingCompilerConfig(debug=True)
  )
  ```

  Note that if you enable the full debugging of the training job with the compiler, this might add some overhead\.