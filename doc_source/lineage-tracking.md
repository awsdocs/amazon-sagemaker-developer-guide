# Amazon SageMaker ML Lineage Tracking<a name="lineage-tracking"></a>

Amazon SageMaker ML Lineage Tracking creates and stores information about the steps of a machine learning \(ML\) workflow from data preparation to model deployment\. With the tracking information, you can reproduce the workflow steps, track model and dataset lineage, and establish model governance and audit standards\.

With SageMaker Lineage Tracking data scientists and model builders can do the following:
+ Keep a running history of model discovery experiments\.
+ Establish model governance by tracking model lineage artifacts for auditing and compliance verification\.
+ Clone and rerun ML workflows to experiment with what\-if scenarios while developing models\.
+ Share a workflow that colleagues can reproduce and enhance \(for example, while collaborating on solving a business problem\)\.
+ Clone and rerun ML workflows with additional debugging or logging routines, or new input variations for troubleshooting issues in production models\.

**Topics**
+ [Lineage Tracking Entities](lineage-tracking-entities.md)
+ [Amazon SageMaker–Created Tracking Entities](lineage-tracking-auto-creation.md)
+ [Manually Create Tracking Entities](lineage-tracking-manual-creation.md)
+ [Querying Lineage Entities](querying-lineage-entities.md)
+ [Cross\-Account Lineage Tracking](xaccount-lineage-tracking.md)

The following diagram shows an example lineage graph that Amazon SageMaker automatically creates in an end\-to\-end model training and deployment ML workflow\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pipelines/PipelineLineageWorkflow.png)