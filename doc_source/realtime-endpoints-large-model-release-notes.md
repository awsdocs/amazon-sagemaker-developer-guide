# Release notes for large model inference deep learning containers<a name="realtime-endpoints-large-model-release-notes"></a>

 You can find a list of deep learning containers \(DLCs\) supported by SageMaker on the [AWS DLC GitHub repository](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#large-model-inference-containers)\. Below are the associated release notes\. 

**Topics**
+ [LMI DLC Release Notes: December 16, 2022](#realtime-endpoints-large-model-release-notes-20221216)
+ [LMI DLC Release Notes: November 4, 2022](#realtime-endpoints-large-model-release-notes-20221104)

## LMI DLC Release Notes: December 16, 2022<a name="realtime-endpoints-large-model-release-notes-20221216"></a>

**Container URI**
+  `763104351884.dkr.ecr.region.amazonaws.com/djl-inference:0.20.0-deepspeed0.7.5-cu116` 

**Highlights**
+  Added BF16 precision support for models supported by DeepSpeed inference \(Bloom, GPT, OPT, etc\.\)\. Training in BF16 does not require any extra transformations to the model weights before hosting with DeepSpeed\. 
+  Added support for Hugging Face Diffusers and StableDiffusion models\. 
+  Includes latest versions of DJL Serving, DeepSpeed, Hugging Face Transformers, and CUDA drivers, which bring stability improvement, better error handling, improved model coverage, and latency\. 

**Version updates**
+ DJL Serving 0\.20\.0
+ DeepSpeed 0\.7\.5
+ Hugging Face Accelerate 0\.13\.2
+ Hugging Face Transformers 4\.23\.1
+ Hugging Face Diffusers 0\.7\.2
+ CUDA 11\.6

## LMI DLC Release Notes: November 4, 2022<a name="realtime-endpoints-large-model-release-notes-20221104"></a>

**Hightlights**
+  Launched LMI DLCs on AWS\. SageMaker and Amazon EC2 support LMI DLCs for inference using model parallelism\. 

**Container URI**
+  `763104351884.dkr.ecr.region.amazonaws.com/djl-inference:0.19.0-deepspeed0.7.3-cu113` 

**Blogs**
+  For more information, see [Deploy BLOOM\-176B and OPT\-30B on Amazon SageMaker with large model inference Deep Learning Containers and DeepSpeed](http://aws.amazon.com/blogs/machine-learning/deploy-bloom-176b-and-opt-30b-on-amazon-sagemaker-with-large-model-inference-deep-learning-containers-and-deepspeed/)\. 