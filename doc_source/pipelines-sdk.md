# SageMaker Pipelines Overview<a name="pipelines-sdk"></a>

 An Amazon SageMaker Model Building Pipelines pipeline is a series of interconnected steps that is defined by a JSON pipeline definition\. This pipeline definition encodes a pipeline using a directed acyclic graph \(DAG\)\. This DAG gives information on the requirements for and relationships between each step of your pipeline\. The structure of a pipeline's DAG is determined by the data dependencies between steps\. These data dependencies are created when the properties of a step's output are passed as the input to another step\. The following image is an example of a pipeline DAG:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pipeline-full.png)

The following topics describe fundamental SageMaker Pipelines concepts\. For a tutorial describing the implementation of these concepts, see [Create and Manage SageMaker Pipelines](pipelines-build.md)\.

**Topics**
+ [Pipeline Structure](build-and-manage-pipeline.md)
+ [Access Management](build-and-manage-access.md)
+ [Pipeline Parameters](build-and-manage-parameters.md)
+ [Pipeline Steps](build-and-manage-steps.md)
+ [Property Files and `JsonGet`](build-and-manage-propertyfile.md)
+ [Caching Pipeline Steps](pipelines-caching.md)
+ [Amazon EventBridge Integration](pipeline-eventbridge.md)
+ [Amazon SageMaker Experiments Integration](pipelines-experiments.md)
+ [Troubleshooting Amazon SageMaker Model Building Pipelines](pipelines-troubleshooting.md)