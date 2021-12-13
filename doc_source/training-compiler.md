# Amazon SageMaker Training Compiler<a name="training-compiler"></a>

Use Amazon SageMaker Training Compiler to train deep learning \(DL\) models faster on scalable GPU instances managed by SageMaker\.

## What Is SageMaker Training Compiler?<a name="training-compiler-what-is"></a>

State\-of\-the\-art deep learning \(DL\) models consist of complex multi\-layered neural networks with billions of parameters that can take thousands of GPU hours to train\. Optimizing such models on training infrastructure requires extensive knowledge of DL and systems engineering; this is challenging even for narrow use cases\. Although there are open\-source implementations of compilers that optimize the DL training process, they can lack the flexibility to integrate DL frameworks with some hardware such as GPU instances\.

SageMaker Training Compiler is a capability of SageMaker that makes these hard\-to\-implement optimizations to reduce training time on GPU instances\. The compiler optimizes DL models to accelerate training by more efficiently using SageMaker machine learning \(ML\) GPU instances\. SageMaker Training Compiler is available at no additional charge within SageMaker and can help reduce total billable time as it accelerates training\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/training-compiler-marketing-diagram.png)

SageMaker Training Compiler is integrated into the AWS Deep Learning Containers \(DLCs\)\. Using the SageMaker Training Compiler–enabled AWS DLCs, you can compile and optimize training jobs on GPU instances with minimal changes to your code\. Bring your deep learning models to SageMaker and enable SageMaker Training Compiler to accelerate the speed of your training job on SageMaker ML instances for accelerated computing\.

## How It Works<a name="training-compiler-how-it-works"></a>

SageMaker Training Compiler accelerates training jobs by converting DL models from their high\-level language representation to hardware\-optimized instructions that train faster than the ordinary frameworks\. Specifically, SageMaker Training Compiler compilation uses graph\-level optimization, dataflow\-level optimizations, and backend optimizations to produce an optimized model that efficiently uses hardware resources and, as a result, trains the compiled model faster\.

It is a two\-step process to enable SageMaker Training Compiler with your DL model training jobs:

1. Use the SageMaker Python SDK or SageMaker API to construct a SageMaker estimator\.

   1. Enable SageMaker Training Compiler by adding `compiler_config=TrainingCompilerConfig()` as a parameter\.

   1. Adjust hyperparameters \(`batch_size` and `learning_rate`\) to maximize the benefit that SageMaker Training Compiler provides through its optimization and efficient usage of the GPU memory\.

1. Bring your own DL script and pass it to the SageMaker estimator\. By running the `estimator.fit()` class method, SageMaker compiles your model and starts training\.

SageMaker Training Compiler does not alter the final trained model, while allowing you to accelerate the training job by more efficiently using the GPU memory and fitting a larger batch size per iteration\. The final trained model from the compiler\-accelerated training job is identical to the one from the ordinary training job\.

**Tip**  
SageMaker Training Compiler only compiles DL models for training on ML GPU instances managed by SageMaker\. To compile your model for inference and deploy it to run anywhere in the cloud and at the edge, use [SageMaker Neo compiler](https://docs.aws.amazon.com/sagemaker/latest/dg/neo.html)\.

**Topics**
+ [What Is SageMaker Training Compiler?](#training-compiler-what-is)
+ [How It Works](#training-compiler-how-it-works)
+ [Supported Frameworks, AWS Regions, Tested Instances, and Tested Models](training-compiler-support.md)
+ [Enable SageMaker Training Compiler](training-compiler-enable.md)
+ [Bring Your Own Deep Learning Model](training-compiler-modify-scripts.md)
+ [SageMaker Training Compiler Example Notebooks and Blogs](training-compiler-examples-and-blogs.md)
+ [SageMaker Training Compiler Best Practices](training-compiler-tips-pitfalls.md)
+ [SageMaker Training Compiler FAQ](training-compiler-faq.md)
+ [SageMaker Training Compiler Troubleshooting](training-compiler-troubleshooting.md)