# View a Pipeline Execution<a name="pipelines-studio-view-execution"></a>

This procedure shows you how to view a pipeline execution\. For information on how to view a list of pipeline executions, and how to use SageMaker search to narrow the executions in the list, see [View a Pipeline](pipelines-studio-list-pipelines.md)\.

**To view details of a pipeline execution**

1. In the execution list, double\-click an execution to view details about the execution\. The execution details tab opens and displays a graph of the steps in the pipeline\. You can choose one of the other tabs to view information about the pipeline execution, which is similar to that shown when viewing the pipeline details\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/yosemite/execution-list.png)

1. Use search to find a step in the graph\. Type characters that match a step name\. You can drag the graph around or use the resizing icons on the lower\-left side of the graph\. The inset on the lower\-right side of the graph displays where you are in the graph\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/yosemite/graph-choose-step.png)

1. Choose one of the steps in the graph to see details about the step\. The following tabs are displayed:
   + **Output** – The metrics, files, and evaluation outcome of the step, dependent on the step type\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/yosemite/graph-step-training.png)
   + **Logs** – The Amazon CloudWatch logs produced by the step\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/yosemite/execution-graph-logs.png)
   + **Info** – The parameters and metadata associated with the step\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/yosemite/execution-graph-info.png)