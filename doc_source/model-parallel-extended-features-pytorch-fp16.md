# FP16 Training with Model Parallelism<a name="model-parallel-extended-features-pytorch-fp16"></a>

For FP16 training, apply the following modifications to your training script and estimator\.

**Note**  
This feature is available in the SageMaker model parallelism library v1\.10\.0 and later\.

**Adapt your PyTorch training script**

1. Wrap your model using the [smdistributed\.modelparallel\.torch\.model\_creation\(\)](https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/latest/smd_model_parallel_pytorch.html#smdistributed.modelparallel.torch.model_creation) context manager\.

   ```
   # fp16_training_script.py
   
   import torch
   import smdistributed.modelparallel.torch as smp
   
   with smp.model_creation(
       dtype=torch.float16 if args.fp16 else torch.get_default_dtype()
   ):
       model = ...
   ```
**Tip**  
If you are using tensor parallelism, add `tensor_parallelism=smp.tp_size() > 1` to the `smp.model_creation` context manager\. Adding this line also helps automatically detect whether tensor parallelism is activated or not\.  

   ```
   with smp.model_creation(
       ... ,
       tensor_parallelism=smp.tp_size() > 1
   ):
       model = ...
   ```

1. When you wrap the optimizer with `smdistributed.modelparallel.torch.DistributedOptimizer`, set either the `static_loss_scaling` or `dynamic_loss_scaling` argument\. By default, `static_loss_scaling` is set to `1.0`, and `dynamic_loss_scaling` is set to `False`\. If you set `dynamic_loss_scale=True`, you can feed dynamic loss scaling options as a dictionary through the `dynamic_loss_args` argument\. In most cases, we recommend you use dynamic loss scaling with the default options\. For more information, options, and examples of the optimizer wrapper function, see the [smdistributed\.modelparallel\.torch\.DistributedOptimizer](https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/latest/smd_model_parallel_pytorch.html#smdistributed-modelparallel-torch-distributedoptimizer) API\.

   The following code is an example of wrapping an `Adadelta` optimizer object with dynamic loss scaling for FP16 training\.

   ```
   optimizer = torch.optim.Adadelta(...)
   optimizer = smp.DistributedOptimizer(
       optimizer,
       static_loss_scale=None,
       dynamic_loss_scale=True,
       dynamic_loss_args={
           "scale_window": 1000,
           "min_scale": 1,
           "delayed_shift": 2
       }
   )
   ```

**Configure a SageMaker PyTorch estimator**

Add the FP16 parameter \(`"fp16"`\) to the distribution configuration for model parallelism when creating a SageMaker PyTorch estimator object\. For a complete list of the configuration parameters for model parallelism, see [Parameters for `smdistributed`](https://sagemaker.readthedocs.io/en/stable/api/training/smd_model_parallel_general.html#parameters-for-smdistributed)\.

```
from sagemaker.pytorch import PyTorch

smp_options = {
    "enabled": True,
    "parameters":  {
        "microbatches":  4,
        "pipeline_parallel_degree":  2,
        "tensor_parallel_degree":  2,
        ...,

        "fp16": True
    }
}

fp16_estimator = PyTorch(
    entry_point="fp16_training_script.py", # Specify your train script
    ...,

    distribution={
        "smdistributed": {"modelparallel": smp_options},
        "mpi": {...}
    }
)

fp16_estimator.fit(...)
```

When FP16 training starts, the model and the optimizer are wrapped by `FP16_Module` and `FP16_Optimizer` respectively, which are modified `smdistributed` versions of the [Apex utils](https://nvidia.github.io/apex/fp16_utils.html#apex-fp16-utils)\. `FP16_Module` converts the model to FP16 dtype and deals with the forward pass in FP16\.

**Tip**  
You can apply gradient clipping by calling `clip_master_grads` before `optimizer.step`\.  

```
optimizer.clip_master_grads(max_norm)     # max_norm(float or int): max norm of the gradients
```

**Tip**  
When using `torch.optim.lr_scheduler` and FP16 training, you need to pass `optimizer.optimizer` to the LR scheduler rather than the optimizer\. See the following example code\.  

```
from torch.optim.lr_scheduler import StepLR

scheduler = StepLR(
    optimizer.optimizer if smp.state.cfg.fp16 else optimizer,
    step_size=1,
    gamma=args.gamma
)
```