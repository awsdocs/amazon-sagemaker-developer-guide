# PyTorch<a name="training-compiler-pytorch-models"></a>

Bring your own PyTorch model to SageMaker, and run the training job with SageMaker Training Compiler\.

**Topics**
+ [PyTorch Models with Hugging Face Transformers](#training-compiler-pytorch-models-transformers)

## PyTorch Models with Hugging Face Transformers<a name="training-compiler-pytorch-models-transformers"></a>

PyTorch models with [Hugging Face Transformers](https://huggingface.co/docs/transformers/index) are based on PyTorch's [torch\.nn\.Module](https://pytorch.org/docs/stable/nn.html#torch.nn.Module) API\. Hugging Face Transformers also provides [Trainer](https://huggingface.co/transformers/main_classes/trainer.html#transformers.Trainer) and pretrained model classes for PyTorch to help reduce the effort for configuring natural language processing \(NLP\) models\. After preparing your training script, you can launch a training job using the SageMaker `PyTorch` or `HuggingFace` estimator with the SageMaker Training Compiler configuration when you'll proceed to the next topic at [Enable SageMaker Training Compiler](training-compiler-enable.md)\.

**Tip**  
When you create a tokenizer for an NLP model using Transformers in your training script, make sure that you use a static input tensor shape by specifying `padding='max_length'`\. Do not use `padding='longest'` because padding to the longest sequence in the batch can change the tensor shape for each training batch\. The dynamic input shape can trigger recompilation of the model and might increase total training time\. For more information about padding options of the Transformers tokenizers, see [Padding and truncation](https://huggingface.co/docs/transformers/pad_truncation) in the *Hugging Face Transformers documentation*\.

**Topics**
+ [Large Language Models Using the Hugging Face Transformers `Trainer` Class](#training-compiler-pytorch-models-transformers-trainer)
+ [Large Language Models Using PyTorch Directly \(without the Hugging Face Transformers Trainer API\)](#training-compiler-pytorch-models-non-trainer)

### Large Language Models Using the Hugging Face Transformers `Trainer` Class<a name="training-compiler-pytorch-models-transformers-trainer"></a>

If you use the transformers library’s Trainer class, you don’t need to make any additional changes to your training script\. SageMaker Training Compiler automatically compiles your Trainer model if you enable it through the estimator class\. The following code shows the basic form of a PyTorch training script with Hugging Face Trainer API\.

```
from transformers import Trainer, TrainingArguments

training_args=TrainingArguments(**kwargs)
trainer=Trainer(args=training_args, **kwargs)
```

**Topics**
+ [For single GPU training](#training-compiler-pytorch-models-transformers-trainer-single-gpu)
+ [For distributed training](#training-compiler-pytorch-models-transformers-trainer-distributed)
+ [Best Practices to Use SageMaker Training Compiler with `Trainer`](#training-compiler-pytorch-models-transformers-trainer-best-practices)

#### For single GPU training<a name="training-compiler-pytorch-models-transformers-trainer-single-gpu"></a>

You don't need to change your code when you use the [https://huggingface.co/docs/transformers/v4.23.1/en/main_classes/trainer#transformers.Trainer](https://huggingface.co/docs/transformers/v4.23.1/en/main_classes/trainer#transformers.Trainer) class\. 

#### For distributed training<a name="training-compiler-pytorch-models-transformers-trainer-distributed"></a>

**PyTorch v1\.11\.0 and later**

To run distributed training with SageMaker Training Compiler, you must add the following `_mp_fn()` function in your training script and wrap the `main()` function\. It redirects the `_mp_fn(index)` function calls from the SageMaker distributed runtime for PyTorch \(`pytorchxla`\) to the `main()` function of your training script\. 

```
def _mp_fn(index):
    main()
```

This function accepts the `index` argument to indicate the rank of the current GPU in the cluster for distributed training\. To find more example scripts, see the [Hugging Face Transformers language modeling example scripts](https://github.com/huggingface/transformers/blob/v4.21.1/examples/pytorch/language-modeling)\.

**For Transformers v4\.17 and before with PyTorch v1\.10\.2 and before**

SageMaker Training Compiler uses an alternate mechanism for launching a distributed training job, and you don't need to make any modification in your training script\. Instead, SageMaker Training Compiler requires you to pass a SageMaker distributed training launcher script to the `entry_point` argument and pass your training script to the `hyperparameters` argument in the SageMaker Hugging Face estimator\.

#### Best Practices to Use SageMaker Training Compiler with `Trainer`<a name="training-compiler-pytorch-models-transformers-trainer-best-practices"></a>
+ Make sure that you use SyncFree optimizers by setting the `optim` argument to `adamw_torch_xla` while setting up [transformers\.TrainingArgument](https://huggingface.co/docs/transformers/main_classes/trainer#transformers.TrainingArguments)\. See also [Optimizer](https://huggingface.co/docs/transformers/v4.23.1/en/perf_train_gpu_one#optimizer) in the *Hugging Face Transformers documentation*\.
+ Ensure that the throughput of the data processing pipeline is higher than the training throughput\. You can tweak the `dataloader_num_workers` and `preprocessing_num_workers` arguments of the [transformers\.TrainingArgument](https://huggingface.co/docs/transformers/main_classes/trainer#transformers.TrainingArguments) class to achieve this\. Typically, these need to be greater than or equal to the number of GPUs but less than the number of CPUs\.

After you have completed adapting your training script, proceed to [Run PyTorch Training Jobs with SageMaker Training Compiler](training-compiler-enable-pytorch.md)\.

### Large Language Models Using PyTorch Directly \(without the Hugging Face Transformers Trainer API\)<a name="training-compiler-pytorch-models-non-trainer"></a>

If you have a training script that uses PyTorch directly, you need to make additional changes to your PyTorch training script to implement PyTorch/XLA\. Follow the instructions to modify your script to properly set up the PyTorch/XLA primatives\.

**Topics**
+ [For single GPU training](#training-compiler-pytorch-models-non-trainer-single-gpu)
+ [For distributed training](#training-compiler-pytorch-models-non-trainer-distributed)
+ [Best Practices to Use SageMaker Training Compiler with PyTorch/XLA](#training-compiler-pytorch-models-best-practices)

#### For single GPU training<a name="training-compiler-pytorch-models-non-trainer-single-gpu"></a>

1. Import the optimization libraries\.

   ```
   import torch_xla
   import torch_xla.core.xla_model as xm
   ```

1. Change the target device to be XLA instead of `torch.device("cuda")`

   ```
   device=xm.xla_device()
   ```

1. If you're using PyTorch's [Automatic Mixed Precision](https://pytorch.org/docs/stable/amp.html) \(AMP\), do the following:

   1. Replace `torch.cuda.amp` with the following:

      ```
      import torch_xla.amp
      ```

   1. Replace `torch.optim.SGD` and `torch.optim.Adam` with the following:

      ```
      import torch_xla.amp.syncfree.Adam as adam
      import torch_xla.amp.syncfree.SGD as SGD
      ```

   1. Replace `torch.cuda.amp.GradScaler` with the following:

      ```
      import torch_xla.amp.GradScaler as grad_scaler
      ```

1. If you're not using AMP, replace `optimizer.step()` with the following:

   ```
   xm.optimizer_step(optimizer)
   ```

1. If you're using a distributed dataloader, wrap your dataloader in the PyTorch/XLA's `ParallelLoader` class:

   ```
   import torch_xla.distributed.parallel_loader as pl
   parallel_loader=pl.ParallelLoader(dataloader, [device]).per_device_loader(device)
   ```

1. Add `mark_step` at the end of the training loop when you're not using `parallel_loader`:

   ```
   xm.mark_step()
   ```

1. To checkpoint your training, use the PyTorch/XLA's model checkpoint method:

   ```
   xm.save(model.state_dict(), path_to_save)
   ```

After you have completed adapting your training script, proceed to [Run PyTorch Training Jobs with SageMaker Training Compiler](training-compiler-enable-pytorch.md)\.

#### For distributed training<a name="training-compiler-pytorch-models-non-trainer-distributed"></a>

In addition to the changes listed in the previous [For single GPU training](#training-compiler-pytorch-models-non-trainer-single-gpu) section, add the following changes to properly distribute workload across GPUs\.

1. If you're using AMP, add `all_reduce` after `scaler.scale(loss).backward()`:

   ```
   gradients=xm._fetch_gradients(optimizer)
   xm.all_reduce('sum', gradients, scale=1.0/xm.xrt_world_size())
   ```

1. If you need to set variables for `local_ranks` and `world_size`, use similar code to the following:

   ```
   local_rank=xm.get_local_ordinal()
   world_size=xm.xrt_world_size()
   ```

1. For any `world_size` \(`num_gpus_per_node*num_nodes`\) greater than `1`, you must define a train sampler which should look similar to the following:

   ```
   import torch_xla.core.xla_model as xm
   
   if xm.xrt_world_size() > 1:
       train_sampler=torch.utils.data.distributed.DistributedSampler(
           train_dataset,
           num_replicas=xm.xrt_world_size(),
           rank=xm.get_ordinal(),
           shuffle=True
       )
   
   train_loader=torch.utils.data.DataLoader(
       train_dataset, 
       batch_size=args.batch_size,
       sampler=train_sampler,
       drop_last=args.drop_last,
       shuffle=False if train_sampler else True,
       num_workers=args.num_workers
   )
   ```

1. Make the following changes to make sure you use the `parallel_loader` provided by the `torch_xla distributed` module\. 

   ```
   import torch_xla.distributed.parallel_loader as pl
   train_device_loader=pl.MpDeviceLoader(train_loader, device)
   ```

   The `train_device_loader` functions like a regular PyTorch loader as follows: 

   ```
   for step, (data, target) in enumerate(train_device_loader):
       optimizer.zero_grad()
       output=model(data)
       loss=torch.nn.NLLLoss(output, target)
       loss.backward()
   ```

   With all of these changes, you should be able to launch distributed training with any PyTorch model without the Transformer Trainer API\. Note that these instructions can be used for both single\-node multi\-GPU and multi\-node multi\-GPU\.

1. **For PyTorch v1\.11\.0 and later**

   To run distributed training with SageMaker Training Compiler, you must add the following `_mp_fn()` function in your training script and wrap the `main()` function\. It redirects the `_mp_fn(index)` function calls from the SageMaker distributed runtime for PyTorch \(`pytorchxla`\) to the `main()` function of your training script\. 

   ```
   def _mp_fn(index):
       main()
   ```

   This function accepts the `index` argument to indicate the rank of the current GPU in the cluster for distributed training\. To find more example scripts, see the [Hugging Face Transformers language modeling example scripts](https://github.com/huggingface/transformers/blob/v4.21.1/examples/pytorch/language-modeling)\.

   **For Transformers v4\.17 and before with PyTorch v1\.10\.2 and before**

   SageMaker Training Compiler uses an alternate mechanism for launching a distributed training job and requires you to pass a SageMaker distributed training launcher script to the `entry_point` argument and pass your training script to the `hyperparameters` argument in the SageMaker Hugging Face estimator\.

After you have completed adapting your training script, proceed to [Run PyTorch Training Jobs with SageMaker Training Compiler](training-compiler-enable-pytorch.md)\.

#### Best Practices to Use SageMaker Training Compiler with PyTorch/XLA<a name="training-compiler-pytorch-models-best-practices"></a>

If you want to leverage the SageMaker Training Compiler on your native PyTorch training script, you may want to first get familiar with [PyTorch on XLA devices](https://pytorch.org/xla/release/1.9/index.html)\. The following sections list some best practices to enable XLA for PyTorch\.

**Note**  
This section for best practices assumes that you use the following PyTorch/XLA modules:  

```
import torch_xla.core.xla_model as xm
import torch_xla.distributed.parallel_loader as pl
```

##### Understand the lazy mode in PyTorch/XLA<a name="training-compiler-pytorch-models-best-practices-lazy-mode"></a>

One significant difference between PyTorch/XLA and native PyTorch is that the PyTorch/XLA system runs in lazy mode while the native PyTorch runs in eager mode\. Tensors in lazy mode are placeholders for building the computational graph until they are materialized after the compilation and evaluation are complete\. The PyTorch/XLA system builds the computational graph on the fly when you call PyTorch APIs to build the computation using tensors and operators\. The computational graph gets compiled and executed when `xm.mark_step()` is called explicitly or implicitly by `pl.MpDeviceLoader/pl.ParallelLoader`, or when you explicitly request the value of a tensor such as by calling `loss.item()` or `print(loss)`\. 

##### Minimize the number of *compilation\-and\-executions* using `pl.MpDeviceLoader/pl.ParallelLoader` and `xm.step_closure`<a name="training-compiler-pytorch-models-best-practices-minimize-comp-exec"></a>

For best performance, you should keep in mind the possible ways to initiate *compilation\-and\-executions* as described in [Understand the lazy mode in PyTorch/XLA](#training-compiler-pytorch-models-best-practices-lazy-mode) and should try to minimize the number of compilation\-and\-executions\. Ideally, only one compilation\-and\-execution is necessary per training iteration and is initiated automatically by `pl.MpDeviceLoader/pl.ParallelLoader`\. The `MpDeviceLoader` is optimized for XLA and should always be used if possible for best performance\. During training, you might want to examine some intermediate results such as loss values\. In such case, the printing of lazy tensors should be wrapped using `xm.add_step_closure()` to avoid unnecessary compilation\-and\-executions\.

##### Use AMP and `syncfree` optimizers<a name="training-compiler-pytorch-models-best-practices-amp-optimizers"></a>

Training in Automatic Mixed Precision \(AMP\) mode significantly accelerates your training speed by leveraging the Tensor cores of NVIDIA GPUs\. SageMaker Training Compiler provides `syncfree` optimizers that are optimized for XLA to improve AMP performance\. Currently, the following three `syncfree` optimizers are available and should be used if possible for best performance\.

```
torch_xla.amp.syncfree.SGD
torch_xla.amp.syncfree.Adam
torch_xla.amp.syncfree.AdamW
```

These `syncfree` optimizers should be paired with `torch_xla.amp.GradScaler` for gradient scaling/unscaling\.

**Tip**  
Starting PyTorch 1\.13\.1, SageMaker Training Compiler improves performance by letting PyTorch/XLA to automatically override the optimizers \(such as SGD, Adam, AdamW\) in `torch.optim` or `transformers.optimization` with the syncfree versions of them in `torch_xla.amp.syncfree` \(such as `torch_xla.amp.syncfree.SGD`, `torch_xla.amp.syncfree.Adam`, `torch_xla.amp.syncfree.AdamW`\)\. You don't need to change those code lines where you define optimizers in your training script\.