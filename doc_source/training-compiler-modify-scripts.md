# Bring Your Own Deep Learning Model<a name="training-compiler-modify-scripts"></a>

Bring your own deep learning models to Amazon SageMaker, and train with Amazon SageMaker Training Compiler\. 

Choose one of the following topics depending on the DL framework of your choice\. This guide walks you through how to prepare a training script for your own DL model and run a compiler\-accelerated training job\. The preparation of your training script depends on the following:
+ Training settings such as single\-core or distributed training\.
+ The framework APIs that you use to create the training script\.

To successfully run a compiler\-accelerated training job, go through the preparation steps for one of the cases that matches with your TensorFlow or PyTorch model\.

**Note**  
After you finish preparing your training script, you can run a SageMaker training job using the SageMaker `Estimator` class\. For more information, see the previous topic at [Enable SageMaker Training Compiler Using the SageMaker Python SDK](training-compiler-enable.md#training-compiler-enable-pysdk)\.

**Topics**
+ [TensorFlow Models](training-compiler-tensorflow-models.md)
+ [PyTorch Models](training-compiler-pytorch-models.md)