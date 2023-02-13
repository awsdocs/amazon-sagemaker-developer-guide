# Configurations and settings<a name="realtime-endpoints-large-model-configuration"></a>

 The following sections describe the configuration options you can use inside the `serving.properties` file\. 

## DJL Serving general settings<a name="realtime-endpoints-large-model-configuration-general"></a>

 These are the general DJL Serving configuration options that you can use in `serving.properties`, irrespective of whatever handler you choose\. 


| Item | Required | Description | Example value | 
| --- | --- | --- | --- | 
| engine | Yes | The execution engine of the code | Python, DeepSpeed | 
| option\.entryPoint | Yes |  Required if you use one of the provided handlers\. Otherwise DJL Serving will look for model\.py in the model directory\.  | djl\_python\.deepspeed, djl\_python\.huggingface | 
| option\.s3url | No |  Download model artifacts from an Amazon S3 bucket using built\-in s5cmd\.  | s3:/djl\-llm/gpt\-j\-6b | 
| option\.tensor\_parallel\_degree | No |  If you plan to execute in multiple GPUs, use this to set the number of GPUs per worker\.  | 2, 4, 8 | 

## Hugging Face handler settings<a name="realtime-endpoints-large-model-huggingface"></a>

 If you use the Hugging Face handler provided by DJL Serving, you can configure these options in `serving.properties`\. 


| Item | Required | Description | Example value | 
| --- | --- | --- | --- | 
| option\.task | No | The task used in Hugging Face for different pipelines\. | text\-generation | 
| option\.model\_dir | No |  The directory path to load the model\. Default is set to the current path with model files\.  | /opt/djl/ml | 
| option\.dtype | No | Datatype that you plan to cast the model default to\. | float16, float32, bfloat16 | 
| option\.model\_id | No |  This will override option\.model\_dir if set\. Used to download model from Hugging Face\.  | Any ID used in Hugging Face | 
| option\.device\_id | No |  Load model on a specific device\. Do not set this if you use option\.tensor\_parallel\_device\.  | 0, 1 | 
| option\.device\_map | No |  Hugging Face Accelerate options to choose device mapping config\.  |  balanced, auto, sequential, or any custom dictionary  | 
| option\.load\_in\_8bit | No |  Use bitsandbytes quantization\. Supported only on certain models\.  | TRUE | 
| option\.low\_cpu\_mem\_usage | No |  Reduce CPU memory usage when loading models\. We recommend that you set this to TRUE\.  | TRUE | 

## DeepSpeed handler settings<a name="realtime-endpoints-large-model-deepspeed"></a>

 If you use the DeepSpeed handler provided by DJL Serving, you can configure these options in `serving.properties`\. 


| Item | Required | Description | Example value | 
| --- | --- | --- | --- | 
| option\.task | No | The task used in Hugging Face for different pipelines\. | text\-generation | 
| option\.model\_dir | No |  The directory path to load the model\. Default is set to the current path with model files\.  | /opt/djl/ml | 
| option\.dtype | No | Datatype that you plan to cast the model default to\. | fp16, fp32, bf16, int8 | 
| option\.model\_id | No |  This will override option\.model\_dir if set\. Used to download model from Hugging Face\.  | Any ID used in Hugging Face | 
| option\.max\_tokens | No |  Total number of tokens \(input and output\) that DeepSpeed can work with\. The number of output tokens in the difference between the total number of tokens and the number of input tokens\.  | 1024 | 
| option\.low\_cpu\_mem\_usage | No |  Reduce CPU memory usage when loading models\. We recommend that you set this to TRUE\.  | TRUE | 
| option\.enable\_cuda\_graph | No |  Enables capturing the CUDA graph of the forward pass to accelerate subsequent inference passes\. Cannot be used with tensor parallelism\.  | TRUE | 
| option\.triangular\_masking | No |  Whether to use triangular masking for the attention mask\. This is application/model specific\.  | FALSE | 
| option\.return\_tuple | No | Whether transformer layers need to return a tuple or a tensor\. | FALSE | 
| option\.training\_mp\_size | No |  If the model was trained with DeepSpeed, this indicates the tensor parallelism degree the model was trained with\. Can be different than the tensor parallel degree desired for inference\.  | 2 | 
| option\.checkpoint | No | Path to DeepSpeed compatible checkpoint file | ds\_inference\_checkpoint\.json | 