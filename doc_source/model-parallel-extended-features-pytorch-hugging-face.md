# Support for Hugging Face Transformer Models<a name="model-parallel-extended-features-pytorch-hugging-face"></a>

The SageMaker model parallel library's tensor parallelism offers out\-of\-the\-box support for the following Hugging Face Transformer models:
+ GPT\-2, BERT, and RoBERTa \(Available in the SageMaker model parallel library v1\.7\.0 and later\)
+ GPT\-J \(Available in the SageMaker model parallel library v1\.8\.0 and later\)
+ GPT\-Neo \(Available in the SageMaker model parallel library v1\.10\.0 and later\)

**Note**  
For any other Transformers models, you need to use the [smdistributed\.modelparallel\.torch\.tp\_register\_with\_module\(\)](https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/latest/smd_model_parallel_pytorch_tensor_parallel.html#smdistributed.modelparallel.torch.tp_register_with_module) API to apply tensor parallelism\.

**Note**  
To use tensor parallelism for training Hugging Face Transformer models, make sure you use Hugging Face Deep Learning Containers for PyTorch that has the SageMaker model parallel library v1\.7\.0 and later\. For more information, see the [SageMaker model parallel library release notes](https://sagemaker.readthedocs.io/en/stable/api/training/smd_model_parallel_release_notes/smd_model_parallel_change_log.html)\.

## Supported Models Out of the Box<a name="model-parallel-extended-features-pytorch-hugging-face-out-of-the-box"></a>

For the Hugging Face transformer models supported by the library out of the box, you don't need to manually implement hooks to translate Transformer APIs to `smdistributed` transformer layers\. You can activate tensor parallelism by using the context manager [smdistributed\.modelparallel\.torch\.tensor\_parallelism\(\)](https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/latest/smd_model_parallel_pytorch_tensor_parallel.html#smdistributed.modelparallel.torch.tensor_parallelism) and wrapping the model by [smdistributed\.modelparallel\.torch\.DistributedModel\(\)](https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/latest/smd_model_parallel_pytorch.html#smdistributed.modelparallel.torch.DistributedModel)\. You don't need to manually register hooks for tensor parallelism using the `smp.tp_register` API\.

The `state_dict` translation functions between Hugging Face Transformers and `smdistributed.modelparallel` can be accessed as follows\.
+  `smdistributed.modelparallel.torch.nn.huggingface.gpt2.translate_state_dict_to_hf_gpt2(state_dict, max_seq_len=None)`
+  `smdistributed.modelparallel.torch.nn.huggingface.gpt2.translate_hf_state_dict_to_smdistributed_gpt2(state_dict)` 
+  `smdistributed.modelparallel.torch.nn.huggingface.bert.translate_state_dict_to_hf_bert(state_dict, max_seq_len=None)` 
+  `smdistributed.modelparallel.torch.nn.huggingface.bert.translate_hf_state_dict_to_smdistributed_bert(state_dict)` 
+  `smdistributed.modelparallel.torch.nn.huggingface.roberta.translate_state_dict_to_hf_roberta(state_dict, max_seq_len=None)` 
+  `smdistributed.modelparallel.torch.nn.huggingface.roberta.translate_hf_state_dict_to_smdistributed_roberta(state_dict)` 
+ `smdistributed.modelparallel.torch.nn.huggingface.gptj.translate_state_dict_to_hf_gptj(state_dict, max_seq_len=None)` \(Available in the SageMaker model parallel library v1\.8\.0 and later\)
+ `smdistributed.modelparallel.torch.nn.huggingface.gptj.translate_hf_gptj_state_dict_to_smdistributed_gptj` \(Available in the SageMaker model parallel library v1\.8\.0 and later\)
+ `smdistributed.modelparallel.torch.nn.huggingface.gptneo.translate_state_dict_to_hf_gptneo(state_dict, max_seq_len=None)` \(Available in the SageMaker model parallel library v1\.10\.0 and later\)
+ `smdistributed.modelparallel.torch.nn.huggingface.gptneo.translate_hf_state_dict_to_smdistributed_gptneo(state_dict)` \(Available in the SageMaker model parallel library v1\.10\.0 and later\)

**Example usage of the GPT\-2 translation function**

Start with wrapping the model as shown in the following code\.

```
from transformers import AutoModelForCausalLM

with smp.tensor_parallelism():
    model = AutoModelForCausalLM.from_config(hf_gpt2_config)

model = smp.DistributedModel(model)
```

Given a `state_dict` from the `DistributedModel` object, you can load the weights into the original Hugging Face GPT\-2 model using the `translate_state_dict_to_hf_gpt2` function as shown in the following code\.

```
from smdistributed.modelparallel.torch.nn.huggingface.gpt2 \
                                      import translate_state_dict_to_hf_gpt2
max_seq_len = 1024

# [... code block for training ...]

if smp.rdp_rank() == 0:
    state_dict = dist_model.state_dict()
    hf_state_dict = translate_state_dict_to_hf_gpt2(state_dict, max_seq_len)

    # can now call model.load_state_dict(hf_state_dict) to the original HF model
```

**Example usage of the RoBERTa translation function**

Similarly, given a supported HuggingFace model `state_dict`, you can use the `translate_hf_state_dict_to_smdistributed` function to convert it to a format readable by `smp.DistributedModel`\. This can be useful in transfer learning use cases, where a pre\-trained model is loaded into a `smp.DistributedModel` for model\-parallel fine\-tuning:

```
from smdistributed.modelparallel.torch.nn.huggingface.roberta \
                                      import translate_state_dict_to_smdistributed

model = AutoModelForMaskedLM.from_config(roberta_config)
model = smp.DistributedModel(model)

pretrained_model = AutoModelForMaskedLM.from_pretrained("roberta-large")
translated_state_dict =
        translate_state_dict_to_smdistributed(pretrained_model.state_dict())

# load the translated pretrained weights into the smp.DistributedModel
model.load_state_dict(translated_state_dict)

# start fine-tuning...
```