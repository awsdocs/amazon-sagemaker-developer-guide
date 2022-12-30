# View a Pipeline Execution<a name="pipelines-studio-view-execution"></a>

This procedure shows you how to view a pipeline execution\. For information on how to view a list of pipeline executions, and how to use SageMaker search to narrow the executions in the list, see [View a Pipeline](pipelines-studio-list-pipelines.md)\.

**To view details of a pipeline execution**

1. In the execution list, open an execution to view details about the execution\. The execution details tab opens and displays a graph of the steps in the pipeline\. You can choose one of the other tabs to view information about the pipeline execution, which is similar to that shown when viewing the pipeline details\.

1. Use search to find a step in the graph\. Type characters that match a step name\. You can drag the graph around or use the resizing icons on the lower\-right side of the graph\. The inset on the lower\-right side of the graph displays where you are in the graph\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/yosemite/execution-graph-w-input.png)

1. Choose one of the steps in the graph to see details about the step\. In the preceding screenshot, a training step is chosen and displays the following tabs:
   + **Input** – The training inputs\. If an input source is from Amazon Simple Storage Service \(Amazon S3\), choose the link to view the file in the Amazon S3 console\.
   + **Output** – The training outputs, such as metrics, charts, files, and evaluation outcome\. The graphs were produced using the [Tracker](https://sagemaker-experiments.readthedocs.io/en/latest/tracker.html#smexperiments.tracker.Tracker.log_precision_recall) APIs\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/yosemite/execution-graph-output.png)
   + **Logs** – The Amazon CloudWatch logs produced by the step\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/yosemite/execution-graph-logs.png)
   + **Info** – The parameters and metadata associated with the step\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/yosemite/execution-graph-info.png)