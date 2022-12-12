# SageMaker Training Compiler Best Practices and Considerations<a name="training-compiler-tips-pitfalls"></a>

Review the following best practices and considerations when using SageMaker Training Compiler\.

## Best Practices<a name="training-compiler-tips-pitfalls-best-practices"></a>

Use the following guidelines to achieve the best results when you run training jobs with SageMaker Training Compiler\.

**General Best Practices**
+ Make sure that you use one of the [Supported Instance Types](training-compiler-support.md#training-compiler-supported-instance-types) and [Tested Models](training-compiler-support.md#training-compiler-tested-models)\. 
+ When you create a tokenizer for an NLP model using the Hugging Face Transformers library in your training script, make sure that you use a static input tensor shape by specifying `padding='max_length'`\. Do not use `padding='longest'` because padding to the longest sequence in the batch can change the tensor shape for each training batch\. The dynamic input shape can initiate recompilation of the model and might increase total training time\. For more information about padding options of the Transformers tokenizers, see [Padding and truncation](https://huggingface.co/docs/transformers/pad_truncation) in the *Hugging Face Transformers documentation*\.
+ Measure GPU memory utilization to make sure that you use the maximum batch size that can fit into the GPU memory\. Amazon SageMaker Training Compiler reduces the memory footprint of your model during training, which typically allows you to fit a larger `batch_size` in the GPU memory\. Using a larger `batch_size` results in a better GPU utilization and reduces the total training time\. 

  When you adjust the batch size, you also have to adjust the `learning_rate` appropriately\. For example, if you increased the batch size by a factor of `k`, you need to adjust `learning_rate` linearly \(simple multiplication by `k`\) or multiply by the square root of `k`\. This is to achieve the same or similar convergence behavior in the reduced training time\. For reference of `batch_size` tested for popular models, see [Tested Models](training-compiler-support.md#training-compiler-tested-models)\.
+ To debug the compiler\-accelerated training job, enable the `debug` flag in the `compiler_config` parameter\. This enables SageMaker to put the debugging logs into SageMaker training job logs\.

  ```
  huggingface_estimator=HuggingFace(
      ...
      compiler_config=TrainingCompilerConfig(debug=True)
  )
  ```

  Note that if you enable full debugging of the training job with the compiler, this might add some overhead\.

**Best Practices for PyTorch**
+ If you bring a PyTorch model and want to checkpoint it, make sure you use PyTorch/XLA's model save function to properly checkpoint your model\. For more information about the function, see [https://pytorch.org/xla/release/1.9/index.html#torch_xla.core.xla_model.save](https://pytorch.org/xla/release/1.9/index.html#torch_xla.core.xla_model.save) in the *PyTorch on XLA Devices documentation*\. 

  To learn how to add the modifications to your PyTorch script, see [Large Language Models Using PyTorch Directly \(without the Hugging Face Transformers Trainer API\)](training-compiler-pytorch-models.md#training-compiler-pytorch-models-non-trainer)\.

  For more information about the actual application of using the model save function, see [Checkpoint Writing and Loading](https://huggingface.co/blog/pytorch-xla#checkpoint-writing-and-loading) in the *Hugging Face on PyTorch/XLA TPUs: Faster and cheaper training blog*\.
+ To achieve the most optimal training time for distributed training, consider the following\.
  + Use instances with multiple GPUs instead of using single\-gpu instances\. For example, a single `ml.p3dn.24xlarge` instance has faster training time compared to 8 x `ml.p3.2xlarge` instances\.
  + Use instances with EFA support such as `ml.p3dn.24xlarge` and `ml.p4d.24xlarge`\. These instance types have accelerated networking speed and reduce training time\.
  + Tune the `preprocessing_num_workers` parameter for datasets, so that model training is not delayed by slow preprocessing\.

## Considerations<a name="training-compiler-tips-pitfalls-considerations"></a>

Consider the following when using SageMaker Training Compiler\.

### Performance degradation due to logging, checkpointing, and profiling<a name="training-compiler-considerations-performance-degradation"></a>
+ Avoid logging, checkpointing, and profiling model tensors that lead to explicit evaluations\. To understand what an explicit evaluation is, consider the following code compiling example\.

  ```
  a = b+c
  e = a+d
  ```

  A compiler interprets the code as follows and reduces the memory footprint for the variable `a`:

  ```
  e = b+c+d
  ```

  Now consider the following case in which the code is changed to add a print function for the variable `a`\.

  ```
  a = b+c
  e = a+d
  print(a)
  ```

  The compiler makes an explicit evaluation of the variable `a` as follows\.

  ```
  e = b+c+d
  a = b+c    # Explicit evaluation
  print(a)
  ```

  In PyTorch, for example, avoid using [torch\.tensor\.items\(\)](https://pytorch.org/docs/stable/generated/torch.Tensor.item.html), which might introduce explicit evaluations\. In deep learning, such explicit evaluations can cause overhead because they break fused operations in a compilation graph of a model and lead to recomputation of the tensors\. 

  If you still want to periodically evaluate the model during training while using SageMaker Training Compiler, we recommend logging and checkpointing at a lower frequency to reduce overhead due to explicit evaluations\. For example, log every 10 epochs instead of every epoch\.
+ Graph compilation runs during the first few steps of training\. As a result, the first few steps are expected to be exceptionally slow\. However, this is a one\-time compilation cost and can be amortized by training for a longer duration because compilation makes future steps much faster\. The initial compilation overhead depends on the size of the model, the size of the input tensors, and the distribution of input tensor shapes\.

### Incorrect use of the PyTorch/XLA APIs when using PyTorch directly<a name="training-compiler-considerations-incorrect-api-use"></a>

PyTorch/XLA defines a set of APIs to replace some of the existing PyTorch training APIs\. Failing to use them properly leads PyTorch training to fail\.
+ One of the most typical errors when compiling a PyTorch model is due to a wrong device type for operators and tensors\. To properly compile a PyTorch model, make sure you use XLA devices \([https://pytorch.org/xla/release/1.9/index.html](https://pytorch.org/xla/release/1.9/index.html)\) instead of using CUDA or mixing CUDA devices and XLA devices\.
+ `mark_step()` is a barrier just for XLA\. Failing to set it correctly causes a training job to stall\.
+ PyTorch/XLA provides additional distributed training APIs\. Failing to program the APIs properly causes gradients to be collected incorrectly, which causes a training convergence failure\.

To properly set up your PyTorch script and avoid the aforementioned incorrect API uses, see [Large Language Models Using PyTorch Directly \(without the Hugging Face Transformers Trainer API\)](training-compiler-pytorch-models.md#training-compiler-pytorch-models-non-trainer)\.