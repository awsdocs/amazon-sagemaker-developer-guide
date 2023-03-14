# Model parallelism and large model inference<a name="realtime-endpoints-large-model-inference"></a>

 State\-of\-the\-art deep learning models for applications such as natural language processing \(NLP\) are large, typically with tens or hundreds of billions of parameters\. Larger models are often more accurate, which makes them attractive to machine learning practitioners\. However, these models are often too large to fit on a single accelerator or GPU device, making it difficult to achieve low\-latency inference\. You can avoid this memory bottleneck by using model parallelism techniques to partition a model across multiple accelerators or GPUs\. 

 Amazon SageMaker includes specialized deep learning containers \(DLCs\), libraries, and tooling for model parallelism and large model inference \(LMI\)\. In the following sections, you can find resources to get started with LMI on SageMaker\. 

**Topics**
+ [Deep learning containers for large model inference](realtime-endpoints-large-model-dlc.md)
+ [SageMaker endpoint parameters for large model inference](realtime-endpoints-large-model-hosting.md)
+ [Large model inference tutorials](realtime-endpoints-large-model-tutorials.md)
+ [Configurations and settings](realtime-endpoints-large-model-configuration.md)
+ [Large model inference FAQs](realtime-endpoints-large-model-faqs.md)
+ [Large model inference troubleshooting](realtime-endpoints-large-model-troubleshooting.md)
+ [Release notes for large model inference deep learning containers](realtime-endpoints-large-model-release-notes.md)