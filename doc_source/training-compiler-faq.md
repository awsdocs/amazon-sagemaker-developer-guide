# SageMaker Training Compiler FAQ<a name="training-compiler-faq"></a>

Use the following FAQ items to find answers to commonly asked questions about SageMaker Training Compiler\.

**Q\. How do I know SageMaker Training Compiler is working?**

If you successfully launched your training job with SageMaker Training Compiler, you receive the following log messages:
+ With `TrainingCompilerConfig(debug=False)`

  ```
  Found configuration for Training Compiler
  Configuring SM Training Compiler...
  ```
+ With `TrainingCompilerConfig(debug=True)`

  ```
  Found configuration for Training Compiler
  Configuring SM Training Compiler...
  Training Compiler set to debug mode
  ```

**Q\. Which models does SageMaker Training Compiler accelerate?**

SageMaker Training Compiler supports the most popular deep learning models from the Hugging Face transformers library\. With most of the operators that the compiler supports, these models can be trained faster with SageMaker Training Compiler\. Compilable models include but are not limited to the following: `bert-base-cased`, `bert-base-chinese`, `bert-base-uncased`, `distilbert-base-uncased`, `distilbert-base-uncased-finetuned-sst-2-english`, `gpt2`, `roberta-base`, `roberta-large`, `t5-base`, and `xlm-roberta-base`\. The compiler works with most DL operators and data structures and can accelerate many other DL models beyond those that have been tested\.

**Q\. What happens if I enable SageMaker Training Compiler with a model that isn't tested?**

For an untested model, you might need to first modify the training script to be compatible with SageMaker Training Compiler\. For more information, see [Bring Your Own Deep Learning Model](training-compiler-modify-scripts.md) and follow the instructions on how to prepare your training script\.

Once you have updated your training script, you can start the training job\. The compiler proceeds to compile the model\. However, training speed may not increase and might even decrease relative to the baseline with an untested model\. You might need to retune training parameters such as `batch_size` and `learning_rate` to achieve any speedup benefits\.

If compilation of the untested model fails, the compiler returns an error\. See [SageMaker Training Compiler Troubleshooting](training-compiler-troubleshooting.md) for detailed information about the failure types and error messages\.

**Q\. Will I always get a faster training job with SageMaker Training Compiler? **

No, not necessarily\. First, SageMaker Training Compiler adds some compilation overhead before the ongoing training process can be accelerated\. The optimized training job must run sufficiently long to amortize and make up for this incremental compilation overhead at the beginning of the training job\.

Additionally, as with any model training process, training with suboptimal parameters can increase training time\. SageMaker Training Compiler can change the characteristics of the training job by, for example, changing the memory footprint of the job\. Because of these differences, you might need to retune your training job parameters to speed up training\. A reference table specifying the best performing parameters for training jobs with different instance types and models can be found at [Tested Models](training-compiler-support.md#training-compiler-tested-models)\.

Finally, some code in a training script might add additional overhead or disrupt the compiled computation graph and slow training\. If working with a customized or untested model, see the instructions at [Best Practices to Enable SageMaker Training Compiler for PyTorch without the Hugging Face Trainer API](training-compiler-pytorch-models.md#training-compiler-pytorch-models-best-practices)\.

**Q\. Can I always use a larger batch size with SageMaker Training Compiler? **

Batch size increases in most, but not all, cases\. The optimizations made by SageMaker Training Compiler can change the characteristics of your training job, such as the memory footprint\. Typically, a Training Compiler job occupies less memory than an uncompiled training job with the native framework, which allows for a larger batch size during training\. A larger batch size, and a corresponding adjustment to the learning rate, increases training throughput and can decrease total training time\.

However, there could be cases where SageMaker Training Compiler might actually increase memory footprint based on its optimization scheme\. The compiler uses an analytical cost model to predict the execution schedule with the lowest cost of execution for any compute\-intensive operator\. This model could find an optimal schedule that increases memory use\. In this case, you won’t be able to increase batch sizes, but your sample throughput is still higher\.

**Q\. Does SageMaker Training Compiler work with other SageMaker training features, such as the SageMaker distributed training libraries and SageMaker Debugger?**

SageMaker Training Compiler is currently not compatible with SageMaker’s distributed training libraries\.

SageMaker Training Compiler is compatible with SageMaker Debugger, but Debugger might degrade computational performance by adding overhead\.

**Q\. Does SageMaker Training Compiler support custom containers \(bring your own container\)?**

No, SageMaker Training Compiler is provided through AWS Deep Learning Containers with Training Compiler implemented\. For more information, see [Supported Frameworks](training-compiler-support.md#training-compiler-supported-frameworks)\. If you need further support, reach out to the SageMaker team through [AWS Support](https://console.aws.amazon.com/support/) or [AWS Developer Forums for Amazon SageMaker](https://forums.aws.amazon.com/forum.jspa?forumID=285)\.