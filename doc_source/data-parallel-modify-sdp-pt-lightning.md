# Modify a PyTorch Lightning Script<a name="data-parallel-modify-sdp-pt-lightning"></a>

If you want to bring your [PyTorch Lightning](https://pytorch-lightning.readthedocs.io/en/latest/starter/introduction.html) training script and run a distributed data parallel training job in SageMaker, you can run the training job with minimal changes in your training script\. The necessary changes include the following: import the `smdistributed.dataparallel` library’s PyTorch modules, set up the environment variables for PyTorch Lightning to accept the SageMaker environment variables that are preset by the SageMaker training toolkit, and activate the [SageMaker data parallel library](https://docs.aws.amazon.com/sagemaker/latest/dg/data-parallel-intro.html) by setting the process group backend to `"smddp"`\. To learn more, walk through the following instructions that break down the steps with code examples\.

**Note**  
The PyTorch Lightning support is available in the SageMaker data parallel library v1\.5\.0 and later\.

1. Import the `pytorch_lightning` library and the `smdistributed.dataparallel.torch` modules\.

   ```
   import pytorch_lightning as pl
   import smdistributed.dataparallel.torch.torch_smddp
   ```

1. Set the world size and the rank for the [https://pytorch-lightning.readthedocs.io/en/stable/api/pytorch_lightning.plugins.environments.LightningEnvironment.html?highlight=LightningEnvironment](https://pytorch-lightning.readthedocs.io/en/stable/api/pytorch_lightning.plugins.environments.LightningEnvironment.html?highlight=LightningEnvironment) class object\. When launching a training job in SageMaker, the SageMaker training toolkit sets up the environment variables `"RANK"`, `"LOCAL_RANK"`, and `"WORLD_SIZE"`\. These environment variables represent the processes' global ranks, their local ranks, and the world size, respectively\. Use these SageMaker environment variables to configure the `LightningEnvironment`\.

   ```
   import os
   from pytorch_lightning.plugins.environments.lightning_environment \
     import LightningEnvironment
   
   env = LightningEnvironment()
   env.world_size = lambda: int(os.environ["WORLD_SIZE"])
   env.global_rank = lambda: int(os.environ["RANK"])
   ```

1. Set distributed training strategy using the [PyTorch Lightning `DDPStrategy`](https://pytorch-lightning.readthedocs.io/en/stable/api/pytorch_lightning.strategies.DDPStrategy.html?highlight=ddpstrategy) module, create a [PyTorch Lightning `Trainer`](https://pytorch-lightning.readthedocs.io/en/stable/api/pytorch_lightning.trainer.trainer.Trainer.html#pytorch_lightning.trainer.trainer.Trainer) object, and adapt them to use the [SageMaker data parallel library](https://docs.aws.amazon.com/sagemaker/latest/dg/data-parallel-intro.html)\.

   **\(Recommended\) For PyTorch Lightning v1\.6\.0 and later**

   Create an object \(`ddp` in the following code example\) of the [https://pytorch-lightning.readthedocs.io/en/stable/api/pytorch_lightning.strategies.DDPStrategy.html?highlight=ddpstrategy](https://pytorch-lightning.readthedocs.io/en/stable/api/pytorch_lightning.strategies.DDPStrategy.html?highlight=ddpstrategy) class, and specify `"smddp"` to the `process_group_backend` parameter\. When configuring a PyTorch Lightning `Trainer` object, use the SageMaker environment variables to specify the scale of the GPU cluster and the `ddp` object to set up the distributed training strategy\.
**Note**  
We recommend that you check the versions of PyTorch Lightning tested for compatibility with the SageMaker data parallel library in the [Supported Frameworks and AWS Regions](distributed-model-parallel-support.md) page\.

   ```
   from pytorch_lightning.strategies import DDPStrategy
   
   ddp = DDPStrategy(
     cluster_environment=env, 
     process_group_backend="smddp", 
     accelerator="gpu"
   )
     
   world_size = int(os.environ["WORLD_SIZE"])
   num_gpus = int(os.environ["SM_NUM_GPUS"])
   num_nodes = int(world_size/num_gpus)
   
   trainer = pl.Trainer(
     devices=num_gpus, 
     num_nodes=num_nodes,
     max_epochs=10,
     strategy=ddp
   )
   ```

   **\(Optional\) For PyTorch Lightning v1\.5\.10**

   If you are using [https://github.com/Lightning-AI/lightning/blob/1.5.10/pytorch_lightning/plugins/training_type/ddp.py#L78](https://github.com/Lightning-AI/lightning/blob/1.5.10/pytorch_lightning/plugins/training_type/ddp.py#L78), which is a deprecated functionality, set the distributed strategy as shown in the following code example\.

   ```
   from pytorch_lightning.plugins.training_type.ddp import DDPPlugin
   
   os.environ["PL_TORCH_DISTRIBUTED_BACKEND"] = "smddp"
   
   ddp = DDPPlugin(
     parallel_devices=[torch.device("cuda", d) for d in range(num_gpus)],
     cluster_environment=env
   )
     
   world_size = int(os.environ["WORLD_SIZE"])
   num_gpus = int(os.environ["SM_NUM_GPUS"])
   num_nodes = int(world_size/num_gpus)
   
   trainer = pl.Trainer(
     gpus=num_gpus, 
     num_nodes=num_nodes, 
     max_epochs=10, 
     strategy=ddp
   )
   ```

1. Run `trainer.fit` to start the training job of a PyTorch model\. The following code example shows a PyTorch model object wrapped by the [PyTorch Lightning Trainer](https://pytorch-lightning.readthedocs.io/en/stable/api/pytorch_lightning.trainer.trainer.Trainer.html#pytorch_lightning.trainer.trainer.Trainer)’s fit method with the [PyTorch Lightning MNIST data module](https://pytorch-lightning.readthedocs.io/en/latest/data/datamodule.html)\.

   ```
   from pl_bolts.datamodules import MNISTDataModule
   
   trainer.fit(model, datamodule=MNISTDataModule(batch_size=32))
   ```

After you have completed adapting your training script, proceed to [Step 2: Launch a SageMaker Distributed Training Job Using the SageMaker Python SDK](data-parallel-use-api.md)\. 

**Note**  
When you construct a SageMaker PyTorch estimator and submit a training job request in [Step 2](https://docs.aws.amazon.com/sagemaker/latest/dg/data-parallel-use-api.html#data-parallel-framework-estimator), you need to provide `requirements.txt` to install `pytorch-lightning` and `lightning-bolts` in the SageMaker PyTorch training container\.  

```
# requirements.txt
pytorch-lightning
lightning-bolts
```
For more information about specifying the source directory to place the `requirements.txt` file along with your training script and a job submission, see [Using third\-party libraries](https://sagemaker.readthedocs.io/en/stable/frameworks/pytorch/using_pytorch.html#id12) in the *Amazon SageMaker Python SDK documentation*\.