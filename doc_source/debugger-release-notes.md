# Amazon SageMaker Debugger Release Notes<a name="debugger-release-notes"></a>

See the following release notes to track the latest updates for Amazon SageMaker Debugger\.

## Amazon SageMaker Debugger Release Notes: March 16, 2023<a name="debugger-release-notes-20230315"></a>

**Deprecation notes**

SageMaker Debugger deprecates the framework profiling feature starting from TensorFlow 2\.11 and PyTorch 2\.0\. You can still use the feature in the previous versions of the frameworks and SDKs as follows\. 
+ SageMaker Python SDK <= v2\.130\.0
+ PyTorch >= v1\.6\.0, < v2\.0
+ TensorFlow >= v2\.3\.1, < v2\.11

With the deprecation, SageMaker Debugger also discontinues support for the following three `ProfilerRules` for framework profiling\.
+ [MaxInitializationTime](https://docs.aws.amazon.com/sagemaker/latest/dg/debugger-built-in-rules.html#max-initialization-time)
+ [OverallFrameworkMetrics](https://docs.aws.amazon.com/sagemaker/latest/dg/debugger-built-in-rules.html#overall-framework-metrics)
+ [StepOutlier](https://docs.aws.amazon.com/sagemaker/latest/dg/debugger-built-in-rules.html#step-outlier)

## Amazon SageMaker Debugger Release Notes: February 21, 2023<a name="debugger-release-notes-20230221"></a>

**Other changes**
+ The XGBoost report tab has been removed from the SageMaker Debugger's profiler dashboard\. You can still access the XGBoost report by downloading it as a Jupyter notebook or a HTML file\. For more information, see [SageMaker Debugger XGBoost Training Report](https://docs.aws.amazon.com/sagemaker/latest/dg/debugger-training-xgboost-report.html)\.
+ Starting from this release, the built\-in profiler rules are not activated by default\. To use the SageMaker Debugger profiler rules to detect certain computational problems, you need to add the rules when you configure a SageMaker training job launcher\.

## Amazon SageMaker Debugger Release Notes: December 1, 2020<a name="debugger-release-notes-20201201"></a>

Amazon SageMaker Debugger launched deep profiling features at re:Invent 2020\.

## Amazon SageMaker Debugger Release Notes: December 3, 2019<a name="debugger-release-notes-20191203"></a>

Amazon SageMaker Debugger initially launched at re:Invent 2019\.