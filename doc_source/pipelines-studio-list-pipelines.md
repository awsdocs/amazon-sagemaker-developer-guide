# View a Pipeline<a name="pipelines-studio-list-pipelines"></a>

This procedure shows you how to directly find a pipeline and view its details page\. You can also find pipelines that are part of a project listed in the project's details page\. For information on finding a pipeline that is part of a project, see [Automate MLOps with SageMaker Projects](sagemaker-projects.md)\.

**To view a list of pipelines**

1. In the left sidebar of Studio, choose the **SageMaker resources** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Components_registries.png)\)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/yosemite/components-registries.png)

1. Select **Pipelines** from the dropdown list\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/yosemite/select-pipelines.png)

1. Drag the right border of the **Components and registries** pane to the right to view all the columns\. Use search to narrow the list of pipelines\. 

   Search is a two\-step process\. First, you enter characters that match the column name you want to search\. Second, you enter characters to match the item in that column\. 

   You can have multiple search filters\. An an example, the following screenshot limits the displayed pipelines to those with a name that starts with `aba` and created by `Me`\. For more information on searching in Studio, see [Search Experiments Using Amazon SageMaker Studio](experiments-search-studio.md)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/yosemite/pipelines-search.png)

1. Open a pipeline to view details about the pipeline\. The pipeline details tab opens and displays a list of pipeline executions\. You can start an execution or choose one of the other tabs for more information about the pipeline\. Use the **Settings** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Settings_squid.png) \) to choose which columns to display\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/yosemite/execution-list.png)

1. From the pipeline details page, choose one of the following tabs to view details about the pipeline:
   + **Executions** – Details about the executions\. You can start an execution from this tab or the **Graph** tab\.
   + **Graph** – The DAG for the pipeline\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/yosemite/pipeline-graph.png)
   + **Parameters** – Includes the model approval status\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/yosemite/pipeline-parameters.png)
   + **Settings** – The metadata associated with the pipeline\. You can download the pipeline definition file and edit the pipeline name and description from this tab\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/yosemite/pipeline-settings.png)