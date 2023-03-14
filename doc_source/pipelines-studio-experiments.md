# View Experiment Entities Created by SageMaker Pipelines<a name="pipelines-studio-experiments"></a>

When you create a pipeline and specify [pipeline\_experiment\_config](https://sagemaker.readthedocs.io/en/stable/workflows/pipelines/sagemaker.workflow.pipelines.html#sagemaker.workflow.pipeline.Pipeline.pipeline_experiment_config), SageMaker Pipelines creates the following SageMaker Experiments entities by default if they don't exist:
+ An experiment for the pipeline
+ A run group for every execution of the pipeline
+ A run for each SageMaker job created in a pipeline step

For information on how experiments are integrated with pipelines, see [Amazon SageMaker Experiments Integration](pipelines-experiments.md)\. For more information on SageMaker Experiments, see [Manage Machine Learning with Amazon SageMaker ExperimentsSupported AWS Regions](experiments.md)\.

You can get to the list of runs associated with a pipeline from either the pipeline executions list or the experiments list\.

**To view the runs list from the pipeline executions list**

1. To view the pipeline executions list, follow the first four steps in [View a Pipeline](pipelines-studio-list-pipelines.md)\.

1. On the top right of the screen, choose the **Filter** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-filter-icon.png) \)\.

1. Select **Experiment**\. If experiment integration wasn't deactivated when the pipeline was created, the experiment name is displayed in the executions list\. 
**Note**  
Experiments integration was introduced in v2\.41\.0 of the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\. Pipelines created with an earlier version of the SDK aren't integrated with experiments by default\.

1. Select the experiment of your choice to view run groups and runs related to that experiment\.

**To view the runs list from the experiments list**

1. In the left sidebar of Studio, choose the **Home** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/icons/house.png)\)\.

1. Select **Experiments** from the menu\.

1. Use search bar or **Filter** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-filter-icon.png) \) to filter the list to experiments created by a pipeline\.

1. Open an experiment name and view a list of runs created by the pipeline\.