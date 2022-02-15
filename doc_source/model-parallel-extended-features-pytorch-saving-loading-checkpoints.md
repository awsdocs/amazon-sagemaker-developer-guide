# Instructions for Checkpointing with Tensor Parallelism<a name="model-parallel-extended-features-pytorch-saving-loading-checkpoints"></a>

The SageMaker model parallel library supports saving partial or full checkpoints with tensor parallelism\. The following guide shows how to modify your script to save and load a checkpoint when you use tensor parallelism\.

1. Prepare a model object and wrap it with the library's wrapper function `smp.DistributedModel()`\.

   ```
   model = MyModel(...)
   model = smp.DistributedModel(model)
   ```

1. Prepare an optimizer for the model\. A set of model parameters is an iterable argument required by optimizer functions\. To prepare a set of model parameters, you must process `model.parameters()` to assign unique IDs to individual model parameters\. 

   If there are parameters with duplicated IDs in the model parameter iterable, loading the checkpointed optimizer state fails\. To create an iterable of model parameters with unique IDs for your optimizer, see the following:

   ```
   unique_params = []
   unique_params_set = set()
   for p in model.parameters():
     if p not in unique_params_set:
       unique_params.append(p)
       unique_params_set.add(p)
   del unique_params_set
   
   optimizer = MyOpt(unique_params, ...)
   ```

1. Wrap the optimizer using the library's wrapper function `smp.DistributedOptimizer()`\.

   ```
   optimizer = smp.DistributedOptimizer(optimizer)
   ```

1. Save the model and the optimizer state using [https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/latest/smd_model_parallel_pytorch.html#smp.save](https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/latest/smd_model_parallel_pytorch.html#smp.save)\. Depending on how you want to save checkpoints, choose one of the following two options:
   + **Option 1:** Save a partial model on each `mp_rank` for a single `MP_GROUP`\.

     ```
     model_dict = model.local_state_dict() # save a partial model
     opt_dict = optimizer.local_state_dict() # save a partial optimizer state
     # Save the dictionaries at rdp_rank 0 as a checkpoint
     if smp.rdp_rank() == 0:
         smp.save(
             {"model_state_dict": model_dict, "optimizer_state_dict": opt_dict},
             f"/checkpoint.pt",
             partial=True,
         )
     ```

     With tensor parallelism, the library saves checkpointed files named in the following format: `checkpoint.pt_{pp_rank}_{tp_rank}`\.
**Note**  
With tensor parallelism, make sure you set the if statement as `if smp.rdp_rank() == 0` instead of `if smp.dp_rank() == 0`\. When the optimizer state is sharded with tensor parallelism, all reduced\-data parallel ranks must save their own partition of the optimizer state\. Using a wrong *if* statement for checkpointing might result in a stalling training job\. For more information about using `if smp.dp_rank() == 0` without tensor parallelism, see [General Instruction for Saving and Loading](https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/latest/smd_model_parallel_pytorch.html#general-instruction-for-saving-and-loading) in the *SageMaker Python SDK documentation*\. 
   + **Option 2:** Save the full model\.

     ```
     if smp.rdp_rank() == 0:
         model_dict = model.state_dict(gather_to_rank0=True) # save the full model
         if smp.rank() == 0:
             smp.save(
                 {"model_state_dict": model_dict},
                 "/checkpoint.pt",
                 partial=False,
             )
     ```
**Note**  
Consider the following for full checkpointing:   
If you set `gather_to_rank0=True`, all ranks other than `0` return empty dictionaries\.
For full checkpointing, you can only checkpoint the model\. Full checkpointing of optimizer states is currently not supported\.
The full model only needs to be saved at `smp.rank() == 0`\.

1. Load the checkpoints using [https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/latest/smd_model_parallel_pytorch.html#smp.load](https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/latest/smd_model_parallel_pytorch.html#smp.load)\. Depending on how you checkpointed in the previous step, choose one of the following two options:
   + **Option 1:** Load the partial checkpoints\.

     ```
     checkpoint = smp.load("/checkpoint.pt", partial=True)
     model.load_state_dict(checkpoint["model_state_dict"], same_partition_load=False)
     optimizer.load_state_dict(checkpoint["optimizer_state_dict"])
     ```

     You can set `same_partition_load=True` in `model.load_state_dict()` for a faster load, if you know that the partition will not change\.
   + **Option 2:** Load the full checkpoints\.

     ```
     if smp.rdp_rank() == 0:
         checkpoint = smp.load("/checkpoint.pt", partial=False)
         model.load_state_dict(checkpoint["model_state_dict"])
     ```

     The `if smp.rdp_rank() == 0` condition is not required, but it can help avoid redundant loading among different `MP_GROUP`s\. Full checkpointing optimizer state dict is currently not supported with tensor parallelism\.