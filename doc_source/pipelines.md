# Amazon SageMaker Model Building Pipelines<a name="pipelines"></a>

Amazon SageMaker Model Building Pipelines is a tool for building machine learning pipelines that take advantage of direct SageMaker integration\. Because of this integration, you can create a pipeline and set up SageMaker Projects for orchestration using a tool that handles much of the step creation and management for you\. SageMaker Pipelines provides the following advantages over other AWS workflow offerings:

**SageMaker Integration**

SageMaker Pipelines is integrated directly with SageMaker, so you don't need to interact with any other AWS services\. You also don't need to manage any resources because SageMaker Pipelines is a fully managed service, which means that it creates and manages resources for you\.

**SageMaker Python SDK Integration**

Because SageMaker Pipelines is integrated with the SageMaker Python SDK, you can create your pipelines programmatically using a high\-level Python interface that you may already be familiar with\. This enables you and members of your team to build your workflow without having to manage your jobs using the AWS console\. To view the SageMaker Python SDK documentation, see [Pipelines](https://sagemaker.readthedocs.io/en/stable/workflows/pipelines/sagemaker.workflow.pipelines.html)\.

**SageMaker Studio Integration**

SageMaker Studio offers an environment to manage the end\-to\-end SageMaker Pipelines experience\. Using Studio, you can bypass the AWS console for your entire workflow management\. For more information on managing SageMaker Pipelines from SageMaker Studio, see [View, Track, and Execute SageMaker Pipelines in SageMaker Studio](pipelines-studio.md)\.

**Data Lineage Tracking**

With SageMaker Pipelines you can track the history of your data within the pipeline execution\. It lets you analyze where the data came from, where it was used as an input, and the outputs that were generated from it\. You can view the models created from an individual dataset and the datasets that went into creating an individual model\. For more information, see [Amazon SageMaker ML Lineage Tracking](lineage-tracking.md)\.

**Step Reuse**

With SageMaker Pipelines, you can designate steps for caching\. When a step is cached, it is indexed for reuse later if the same step is executed again\. As a result, you can reuse the output from previous step executions of the same step in the same pipeline without having to run the step again\. For more information on step caching, see [Caching Pipeline Steps](pipelines-caching.md)\.

**Topics**
+ [SageMaker Pipelines Overview](pipelines-sdk.md)
+ [Create and Manage SageMaker Pipelines](pipelines-build.md)