# PyTorch Models<a name="training-compiler-pytorch-models"></a>

Bring your own PyTorch model to SageMaker, and run the training job with SageMaker Training Compiler\.

## PyTorch Models with Hugging Face Transformers<a name="training-compiler-pytorch-models-transformers"></a>

PyTorch models with Hugging Face Transformers are based on PyTorch's [https://pytorch.org/docs/stable/nn.html#torch.nn.Module](https://pytorch.org/docs/stable/nn.html#torch.nn.Module)\. The Hugging Face Transformers library also provides the [https://huggingface.co/transformers/main_classes/trainer.html#transformers.Trainer](https://huggingface.co/transformers/main_classes/trainer.html#transformers.Trainer) class for PyTorch to help reduce the effort to configure distributed training, mixed precision, and so on\. After creating your own training script, you can use SageMaker's script mode to directly inject the training script to the SageMaker HuggingFace estimator with SageMaker Training Compiler configuration\.

**Topics**
+ [Using Hugging Face Trainer API](#training-compiler-pytorch-models-transformers-trainer)
+ [Using PyTorch without Hugging Face Trainer API](#training-compiler-pytorch-models-non-trainer)
+ [Best Practices to Enable SageMaker Training Compiler for PyTorch without the Hugging Face Trainer API](#training-compiler-pytorch-models-best-practices)

### Using Hugging Face Trainer API<a name="training-compiler-pytorch-models-transformers-trainer"></a>

If you use the transformers library’s Trainer class, you don’t need to make any additional changes to your training script\. SageMaker Training Compiler automatically compiles your Trainer model if you enable it through the estimator class\. The following code shows the basic form of a PyTorch training script with Hugging Face Trainer API\.

```
from transformers import Trainer, TrainingArguments

training_args=TrainingArguments(**kwargs)
trainer=Trainer(args=training_args, **kwargs)
```

#### For single GPU training<a name="training-compiler-pytorch-models-transformers-trainer-single-gpu"></a>

You don't need to change your code when you use the Transformers Trainer API\. 

#### For distributed training<a name="training-compiler-pytorch-models-transformers-trainer-distributed"></a>

SageMaker Training Compiler uses an alternate mechanism for distributed training, and you don’t need to specify the `distribution` parameter\. The compiler requires you to pass a SageMaker distributed training launcher script to the entry point and pass your training script to the `hyperparameters` argument\. For more information, choose **For distributed training** and the **Hugging Face Estimator for PyTorch** tab in the [Enable SageMaker Training Compiler Using the SageMaker Python SDK](training-compiler-enable.md#training-compiler-enable-pysdk) page\.

### Using PyTorch without Hugging Face Trainer API<a name="training-compiler-pytorch-models-non-trainer"></a>

If you have a training script that uses PyTorch directly, you need to make additional changes to your PyTorch training script\. For example, you need to modify the script using PyTorch/XLA as shown in the following code examples:

**Topics**
+ [For single GPU training](#training-compiler-pytorch-models-non-trainer-single-gpu)
+ [For distributed training](#training-compiler-pytorch-models-non-trainer-distributed)

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

### Best Practices to Enable SageMaker Training Compiler for PyTorch without the Hugging Face Trainer API<a name="training-compiler-pytorch-models-best-practices"></a>

If you want to leverage the SageMaker Training Compiler on your native PyTorch training script, you may want to first get familiar with [PyTorch on XLA devices](https://pytorch.org/xla/release/1.9/index.html)\. The following sections list some best practices to enable XLA for PyTorch\.

**Note**  
This guide assumes that you use the following Python modules:  

```
import torch_xla.core.xla_model as xm
import torch_xla.distributed.parallel_loader as pl
```

#### Understand the lazy mode in PyTorch/XLA<a name="training-compiler-pytorch-models-best-practices-lazy-mode"></a>

One significant difference between PyTorch/XLA and native PyTorch is that the PyTorch/XLA system runs in lazy mode while the native PyTorch runs in eager mode\. Tensors in lazy mode are placeholders for building the computational graph until they are materialized after the compilation and evaluation are complete\. The PyTorch/XLA system builds the computational graph on the fly when you call PyTorch APIs to build the computation using tensors and operators\. The computational graph gets compiled and executed when `xm.mark_step()` is called explicitly or implicitly by `pl.MpDeviceLoader/pl.ParallelLoader`, or when you explicitly request the value of a tensor such as by calling `loss.item()` or `print(loss)`\. 

#### Minimize the number of *compilation\-and\-executions* using `pl.MpDeviceLoader/pl.ParallelLoader` and `xm.step_closure`<a name="training-compiler-pytorch-models-best-practices-minimize-comp-exec"></a>

For best performance, you should keep in mind the possible ways to initiate *compilation\-and\-executions* as described in [Understand the lazy mode in PyTorch/XLA](#training-compiler-pytorch-models-best-practices-lazy-mode) and should try to minimize the number of compilation\-and\-executions\. Ideally, only one compilation\-and\-execution is necessary per training iteration and is initiated automatically by `pl.MpDeviceLoader/pl.ParallelLoader`\. The `MpDeviceLoader` is optimized for XLA and should always be used if possible for best performance\. During training, you might want to examine some intermediate results such as loss values\. In such case, the printing of lazy tensors should be wrapped using `xm.add_step_closure()` to avoid unnecessary compilation\-and\-executions\.

#### Use AMP and `syncfree` optimizers<a name="training-compiler-pytorch-models-best-practices-amp-optimizers"></a>

Training in Automatic Mixed Precision \(AMP\) mode significantly accelerates your training speed by leveraging the Tensor cores of NVIDIA GPUs\. SageMaker Training Compiler provides `syncfree` optimizers that are optimized for XLA to improve AMP performance\. Currently, the following three `syncfree` optimizers are available and should be used if possible for best performance\.

```
torch_xla.amp.syncfree.SGD
torch_xla.amp.syncfree.Adam
torch_xla.amp.syncfree.AdamW
```

These `syncfree` optimizers should be paired with `torch_xla.amp.GradScaler` for gradient scaling/unscaling\.