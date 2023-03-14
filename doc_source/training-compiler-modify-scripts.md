# Bring Your Own Deep Learning Model<a name="training-compiler-modify-scripts"></a>

This guide walks you through how to adapt your training script for a compiler\-accelerated training job\. The preparation of your training script depends on the following:
+ Training settings such as single\-core or distributed training\.
+ Frameworks and libraries that you use to create the training script\.

Choose one of the following topics depending on the framework you use\.

**Topics**
+ [PyTorch](training-compiler-pytorch-models.md)
+ [TensorFlow](training-compiler-tensorflow-models.md)

**Note**  
After you finish preparing your training script, you can run a SageMaker training job using the SageMaker framework estimator classes\. For more information, see the previous topic at [Enable SageMaker Training Compiler](training-compiler-enable.md)\.