# Data Processing with Framework Processors<a name="processing-job-frameworks"></a>

A `FrameworkProcessor` can run Processing jobs with a specified machine learning framework, providing you with an Amazon SageMaker–managed container for whichever machine learning framework you choose\. `FrameworkProcessor` provides premade containers for the following machine learning frameworks: Hugging Face, MXNet, PyTorch, TensorFlow, and XGBoost\.

The `FrameworkProcessor` class also provides you with customization over the container configuration\. The `FrameworkProcessor` class supports specifying a source directory `source_dir` for your processing scripts and dependencies\. With this capability, you can give the processor access to multiple scripts in a directory instead of only specifying one script\. `FrameworkProcessor` also supports including a `requirements.txt` file in the `source_dir` for customizing the Python libraries to install in the container\.

For more information on the `FrameworkProcessor` class and its methods and parameters, see [FrameworkProcessor](https://sagemaker.readthedocs.io/en/stable/api/training/processing.html#sagemaker.processing.FrameworkProcessor) in the *Amazon SageMaker Python SDK*\.

To see examples of using a `FrameworkProcessor` for each of the supported machine learning frameworks, see the following topics\.

**Topics**
+ [Hugging Face Framework Processor](processing-job-frameworks-hugging-face.md)
+ [MXNet Framework Processor](processing-job-frameworks-mxnet.md)
+ [PyTorch Framework Processor](processing-job-frameworks-pytorch.md)
+ [TensorFlow Framework Processor](processing-job-frameworks-tensorflow.md)
+ [XGBoost Framework Processor](processing-job-frameworks-xgboost.md)