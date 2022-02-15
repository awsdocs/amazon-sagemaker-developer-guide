# Support for Hugging Face Transformer Models<a name="model-parallel-extended-features-pytorch-hugging-face"></a>

The SageMaker model parallel library's tensor parallelism offers out\-of\-the\-box support for Hugging Face Transformer models, such as GPT\-2, BERT, and RoBERTa\.

**Note**  
To use tensor parallelism for training Hugging Face Transformer models, you should add `transformers==4.4.2` to `requirements.txt` for installing the Transformers library in Deep Learning Container for PyTorch 1\.8\.1 with tensor parallelism implemented\.

If you use one of the Hugging Face Transformer models, you do not need to manually implement hooks to translate Transformer APIs to `smdistributed` transformer layers\. You also do not need to manually register hooks for tensor parallelism using the `smp.tp_register` API\. You can activate tensor parallelism by using the context manager `smp.tensor_parallelism()` and wrapping the model by [https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/latest/smd_model_parallel_pytorch.html#smp.DistributedModel](https://sagemaker.readthedocs.io/en/stable/api/training/smp_versions/latest/smd_model_parallel_pytorch.html#smp.DistributedModel)\. For example, see the following code:

```
from transformers import AutoModelForCausalLM

with smp.tensor_parallelism():
    model = AutoModelForCausalLM.from_config(hf_gpt2_config)

model = smp.DistributedModel(model)
```

Further, given a `state_dict` from the `DistributedModel` object, you can load the weights into the original HuggingFace GPT\-2 model using the `translate_state_dict_to_hf_gpt2` API:

```
from smdistributed.modelparallel.torch.nn.huggingface.gpt2 \
                                      import translate_state_dict_to_hf_gpt2
max_seq_len = 1024

with smp.tensor_parallelism():
    model = AutoModelForCausalLM.from_config(hf_gpt2_config)

model = smp.DistributedModel(model)

# [...run training...]

if smp.rdp_rank() == 0:
    state_dict = dist_model.state_dict()
    hf_state_dict = translate_state_dict_to_hf_gpt2(state_dict, max_seq_len)
    
    # can now call model.load_state_dict(hf_state_dict) to the original HF model
```

Similarly, given a supported HuggingFace model `state_dict`, you can use `translate_hf_state_dict_to_smdistributed` API to convert it to a format readable by `smp.DistributedModel`\. This can be useful in transfer learning use cases, where a pre\-trained model is loaded into a `smp.DistributedModel` for model\-parallel fine\-tuning:

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

The relevant `state_dict` translation functions between Hugging Face and `smp` can be accessed as follows\.
+  `from smdistributed.modelparallel.torch.nn.huggingface.gpt2 import translate_state_dict_to_hf_gpt2` 
+  `from smdistributed.modelparallel.torch.nn.huggingface.gpt2 import translate_hf_state_dict_to_smdistributed` 
+  `from smdistributed.modelparallel.torch.nn.huggingface.bert import translate_state_dict_to_hf_bert` 
+  `from smdistributed.modelparallel.torch.nn.huggingface.bert import translate_hf_state_dict_to_smdistributed` 
+  `from smdistributed.modelparallel.torch.nn.huggingface.roberta import translate_state_dict_to_hf_roberta` 
+  `from smdistributed.modelparallel.torch.nn.huggingface.roberta import translate_hf_state_dict_to_smdistributed` 