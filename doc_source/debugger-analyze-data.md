# Analyze Data Using the SMDebug Client Library<a name="debugger-analyze-data"></a>

**Important**  
To use new features with an existing notebook instance or Studio app, you will need to restart the notebook instance or Studio app to get the latest updates\.

While your training job is running or after it has completed, you can access the training data collected by Debugger using the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) and the [SMDebug client library](https://github.com/awslabs/sagemaker-debugger/)\. The SMDebug library provides analysis and visualization tools that enable you to drill down into your training job data\.

**To install the library and use the SMDebug analysis tools \(in a JupyterLab notebook or an iPython kernel\)**

```
! pip install smdebug
```

The following topics walk you through how to use the SMDebug tools to visualize and analyze the training data collected by Debugger\.

**Analyze system and framework metrics**
+ [Access the Monitoring and Profiling Data](debugger-analyze-data-profiling.md)
+ [Plot the System Metrics and Framework Metrics Data](debugger-access-data-profiling-default-plot.md)
+ [Access the Profiling Data Using the Pandas Data Parsing Tool](debugger-access-data-profiling-pandas-frame.md)
+ [Access the Python Profiling Stats Data](debugger-access-data-python-profiling.md)
+ [Merge Timelines of Different Profiling Trace Files](debugger-merge-timeline.md)
+ [Profiling Data Loader](debugger-data-loading-time.md)