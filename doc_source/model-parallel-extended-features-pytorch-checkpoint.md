# Checkpointing Distributed Models and Optimizer States<a name="model-parallel-extended-features-pytorch-checkpoint"></a>

The SageMaker model parallel library provides checkpoint APIs to save and load distributed models and optimizer states\.

**Note**  
This feature is available in the SageMaker model parallel library v1\.10\.0 and later\.

To save a checkpoint of a model trained with model parallelism, use the [https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/latest/smd_model_parallel_pytorch.html#smdistributed.modelparallel.torch.save_checkpoint](https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/latest/smd_model_parallel_pytorch.html#smdistributed.modelparallel.torch.save_checkpoint) API with the partial checkpointing \(`partial=True`\) that saves each model partition individually\. In addition to the model and the optimizer state, you can also save any additional custom data through the `user_content` argument\. The checkpointed model, optimizer, and user content are saved as separate files\. The `save_checkpoint` API call creates checkpoint folders in the following structure\. 

```
- path
  - ${tag}_partial (folder for partial checkpoints)
    - model_rankinfo.pt
    - optimizer_rankinfo.pt
    - fp16_states_rankinfo.pt
    - user_content.pt
  - $tag (checkpoint file for full checkpoints)
  - user_content_$tag (user_content file for full checkpoints)
  - newest (a file that indicates the newest checkpoint)
```

To resume training from a checkpoint, use the [https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/latest/smd_model_parallel_pytorch.html#smdistributed.modelparallel.torch.resume_from_checkpoint](https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/latest/smd_model_parallel_pytorch.html#smdistributed.modelparallel.torch.resume_from_checkpoint) API with `partial=True`, using the checkpoint directory and the tag used while saving the checkpoint\. Note that the actual loading of model weights happens after model partitioning, during the first run of the `smdistributed.modelparallel.torch.step`\-decorated training step function\.

When saving a partial checkpoint, the library also saves the model partition decision as files with `.pt` file extension\. Conversely, when resuming from the partial checkpoint, the library loads the partition decision files together\. Once the partition decision is loaded, you can't change the partition\.

To save the final model artifact for inference purposes, use the `smdistributed.modelparallel.torch.save_checkpoint` API with `partial=False`, which combines the model partitions to create a single model artifact\. Note that this does not combine the optimizer states\.

To initialize training with particular weights, given a full model checkpoint, you can use the `smdistributed.modelparallel.torch.resume_from_checkpoint` API with `partial=False`\. Note that this does not load optimizer states\.

**Note**  
With tensor parallelism, in general, the `state_dict` must be translated between the original model implementation and the `DistributedModel` implementation\. Optionally, you can provide the `state_dict` translation function as an argument to the `smdistributed.modelparallel.torch.resume_from_checkpoint`\. However, for [Supported Models Out of the Box](model-parallel-extended-features-pytorch-hugging-face.md#model-parallel-extended-features-pytorch-hugging-face-out-of-the-box), the library takes care of this translation automatically\.

The following code shows an example of how to use the checkpoint APIs for saving and loading a model trained with model parallelism\.

```
import smdistributed.modelparallel.torch as smp

model = ...
model = smp.DistributedModel(model)
optimizer = ...
optimizer = smp.DistributedOptimizer(optimizer)
user_content = ...     # additional custom data
checkpoint_path = "/opt/ml/checkpoint/model_parallel"

# Save a checkpoint.
smp.save_checkpoint(
    path=checkpoint_path,
    tag=f"total_steps{total_steps}",
    partial=True,
    model=model,
    optimizer=optimizer,
    user_content=user_content
    num_kept_partial_checkpoints=5
)

# Load a checkpoint.
# This automatically loads the most recently saved checkpoint.
smp_checkpoint = smp.resume_from_checkpoint(path=checkpoint_path, partial=True)
```