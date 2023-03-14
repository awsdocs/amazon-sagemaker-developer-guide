# Large model inference FAQs<a name="realtime-endpoints-large-model-faqs"></a>

 Refer to the following FAQ items for answers to commonly asked questions about large model inference \(LMI\) with SageMaker\. 

## Q: When should I use LMI DLCs?<a name="realtime-endpoints-large-model-faqs-1"></a>

 A: Large model inference deep learning containers \(LMI DLCs\) include tested versions of popular model parallelization and optimization libraries for hosting models with tens or hundreds of billions of parameters\. You should use an LMI DLC if your deep learning model requires multiple accelerators for hosting or if you want to accelerate inference with a popular, supported model such as GPT, Bloom, and OPT\. 

## Q: Who can I contact if something doesn't work?<a name="realtime-endpoints-large-model-faqs-2"></a>

 A: If, after reviewing the documentation, you continue to have problems hosting your large model, contact us at [LMI\-DLC\-feedback@amazon\.com](mailto:LMI-DLC-feedback@amazon.com)\. 

## Q: Can I use large model inference deep learning containers \(LMI DLCs\) outside of SageMaker?<a name="realtime-endpoints-large-model-faqs-3"></a>

 A: AWS LMI DLCs have been tested on and designed for SageMaker, but can be used on Amazon EC2 with [supported instance types](realtime-endpoints-large-model-dlc.md#realtime-endpoints-large-model-dlc-instance)\. 

## Q: What model formats can I use with LMI DLCs?<a name="realtime-endpoints-large-model-faqs-4"></a>

 AWS LMI DLCs include a convenience function to load a Hugging Face model format\. You can bring models in another format, such as Megatron checkpoint, but you need to write logic to load that format or convert to a Hugging Face format\. 

## Q: How can I switch between model parallelization libraries such as Hugging Face Accelerate and DeepSpeed?<a name="realtime-endpoints-large-model-faqs-5"></a>

 If you're using the DJL handler \(like in the [tutorial](realtime-endpoints-large-model-tutorials-deepspeed-djl.md) example\), then switch the default engine and handler in the `serving.properties` file\. For example, modify the `engine` and `option.entryPoint` settings to switch from DeepSpeed to Hugging Face\. In general, you can switch the engine and use the same tensor parallel degree\. 