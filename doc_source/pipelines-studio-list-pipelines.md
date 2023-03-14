# View a Pipeline<a name="pipelines-studio-list-pipelines"></a>

This procedure shows you how to directly find a pipeline and view its details page\. You can also find pipelines that are part of a project listed in the project's details page\. For information on finding a pipeline that is part of a project, see [Automate MLOps with SageMaker Projects](sagemaker-projects.md)\.

**To view a list of pipelines**

1. In the Studio sidebar, choose the **Home** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/icons/house.png)\)\.

1. Select **Pipelines** from the menu\.

1. Use search to narrow the list of pipelines by name\.

1. Select a pipeline name to view details about the pipeline\. The pipeline details tab opens and displays a list of pipeline executions\. You can start an execution or choose one of the other tabs for more information about the pipeline\. Use the **Property Inspector** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/icons/gears.png) \) to choose which columns to display\.

1. From the pipeline details page, choose one of the following tabs to view details about the pipeline:
   + **Executions** – Details about the executions\. You can create an execution from this tab or the **Graph** tab\.
   + **Graph** – The DAG for the pipeline\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/yosemite/pipeline-graph.png)
   + **Parameters** – Includes the model approval status\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/yosemite/pipeline-parameters.png)
   + **Settings** – The metadata associated with the pipeline\. You can download the pipeline definition file and edit the pipeline name and description from this tab\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/yosemite/pipeline-settings.png)