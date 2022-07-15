# Ranking Mechanism<a name="model-parallel-extended-features-pytorch-ranking-mechanism"></a>

This section explains how the ranking mechanisim of model parallelism works with tensor parallelism\. This is extended from the [Ranking Basics](https://sagemaker.readthedocs.io/en/stable/api/training/smd_model_parallel_general.html#ranking-basics) for [Core Features of the SageMaker Model Parallel Library](model-parallel-core-features.md)\. With tensor parallelism, the library introduces three types of ranking and process group APIs: `smp.tp_rank()` for *tensor parallel rank*, `smp.pp_rank()` for *pipeline parallel rank*, and `smp.rdp_rank()` for *reduced\-data parallel rank*\. The corresponding communication process groups are *tensor parallel group* \(`TP_GROUP`\), *pipeline parallel group* \(`PP_GROUP`\), and *reduced\-data parallel group* \(`RDP_GROUP`\)\. These groups are defined as follows:
+ A tensor parallel group \(`TP_GROUP`\) is an evenly divisible subset of the data parallel group, over which tensor parallel distribution of modules takes place\. When the degree of pipeline parallelism is 1, `TP_GROUP` is the same as *model parallel group* \(`MP_GROUP`\)\. 
+ A pipeline parallel group \(`PP_GROUP`\) is the group of processes over which pipeline parallelism takes place\. When the tensor parallelism degree is 1, `PP_GROUP` is the same as `MP_GROUP`\. 
+ A reduced\-data parallel group \(`RDP_GROUP`\) is a set of processes that hold both the same pipeline parallelism partitions, as well as the same tensor parallel partitions, and perform data parallelism among themselves\. This is called the reduced data parallel group because it is a subset of the entire data parallelism group, `DP_GROUP`\. For the model parameters that are distributed within the `TP_GROUP` , the gradient `allreduce` operation is performed only for reduced\-data parallel group, while for the parameters that are not distributed, the gradient `allreduce` takes place over the entire `DP_GROUP`\. 
+ A model parallel group \(`MP_GROUP`\) refers to a group of processes that collectively store the entire model\. It consists of the union of the `PP_GROUP`s of all the ranks that are in the `TP_GROUP` of the current process\. When the degree of tensor parallelism is 1, `MP_GROUP` is equivalent to `PP_GROUP`\. It is also consistent with the existing definition of `MP_GROUP` from previous `smdistributed` releases\. Note that the current `TP_GROUP` is a subset of both the current `DP_GROUP` and the current `MP_GROUP`\. 

To learn more about the communication process APIs in the SageMaker distributed model parallelism library, see the [Common API](https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/latest/smd_model_parallel_common_api.html#) and the [PyTorch\-specific APIs](https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/latest/smd_model_parallel_pytorch.html) in the *SageMaker Python SDK documentation*\.

The following figure illustrates the ranking mechanism, parameter distribution, and associated `allreduce` operations\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/distributed/model-parallel/tensor-parallel-ranking-mechanism.png)

The preceding figure illustrates process groups for a node with 8 GPUs, where the degree of tensor parallelism is 2, the degree of pipeline parallelism is 2, and the degree of data parallelism is 4\. The first figure shows an example model with 4 layers\. In the other figures, the 4\-layer model is distributed across 4 GPUs using both pipeline parallelism and tensor parallelism, where tensor parallelism is used for the middle two layers\. These figures are simple copies to illustrate different group boundary lines\. The partitioned model is replicated for data parallelism across GPUs 0\-3 and 4\-7\. The lower left figure shows the definitions of `MP_GROUP`, `PP_GROUP`, and `TP_GROUP`\. The lower right figure shows `RDP_GROUP`, `DP_GROUP`, and `WORLD` over the same set of GPUs\. The gradients for the layers and layer slices that have the same color are `allreduce`d together for data parallelism\. For example, the first layer \(light blue\) gets the `allreduce` operations across `DP_GROUP`, whereas the dark orange slice in the second layer only gets the `allreduce` operations within the `RDP_GROUP` of its process\. The bold dark red arrows represent tensors with the batch of its entire `TP_GROUP`\.

```
GPU0: pp_rank 0, tp_rank 0, rdp_rank 0, dp_rank 0, mp_rank 0
GPU1: pp_rank 1, tp_rank 0, rdp_rank 0, dp_rank 0, mp_rank 1
GPU2: pp_rank 0, tp_rank 1, rdp_rank 0, dp_rank 1, mp_rank 2
GPU3: pp_rank 1, tp_rank 1, rdp_rank 0, dp_rank 1, mp_rank 3
GPU4: pp_rank 0, tp_rank 0, rdp_rank 1, dp_rank 2, mp_rank 0
GPU5: pp_rank 1, tp_rank 0, rdp_rank 1, dp_rank 2, mp_rank 1
GPU6: pp_rank 0, tp_rank 1, rdp_rank 1, dp_rank 3, mp_rank 2
GPU7: pp_rank 1, tp_rank 1, rdp_rank 1, dp_rank 3, mp_rank 3
```

In this example, pipeline parallelism occurs across the GPU pairs \(0,1\); \(2,3\); \(4,5\) and \(6,7\)\. In addition, we have data parallelism \(`allreduce`\) taking place across GPUs 0, 2, 4, 6, and independently over GPUs 1, 3, 5, 7\. Tensor parallelism happens over subsets of `dp_group`s, across the GPU pairs \(0,2\); \(1,3\); \(4,6\) and \(5,7\)\.

  For this kind of hybrid pipeline and tensor parallelism, the math for `data_parallel_degree` remains as `data_parallel_degree = number_of_GPUs / pipeline_parallel_degree`\. The library further calculates the reduced data parallel degree from the following relation `reduced_data_parallel_degree * tensor_parallel_degree = data_parallel_degree`\.  