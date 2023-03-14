# Amazon SageMaker Experiments Integration<a name="pipelines-experiments"></a>

Amazon SageMaker Model Building Pipelines is closely integrated with Amazon SageMaker Experiments\. By default, when SageMaker Pipelines creates and executes a pipeline, the following SageMaker Experiments entities are created if they don't exist:
+ An experiment for the pipeline
+ A run group for every execution of the pipeline
+ A run that's added to the run group for each SageMaker job created in a pipeline execution step

You can compare metrics such as model training accuracy across multiple pipeline executions just as you can compare such metrics across multiple run groups of a SageMaker model training experiment\.

The following sample shows the relevant parameters of the [Pipeline](https://github.com/aws/sagemaker-python-sdk/blob/v2.41.0/src/sagemaker/workflow/pipeline.py) class in the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\.

```
Pipeline(
    name="MyPipeline",
    parameters=[...],
    pipeline_experiment_config=PipelineExperimentConfig(
      ExecutionVariables.PIPELINE_NAME,
      ExecutionVariables.PIPELINE_EXECUTION_ID
    ),
    steps=[...]
)
```

If you don't want an experiment and run group created for the pipeline, set `pipeline_experiment_config` to `None`\.

**Note**  
Experiments integration was introduced in the Amazon SageMaker Python SDK v2\.41\.0\.

The following naming rules apply based on what you specify for the `ExperimentName` and `TrialName` parameters of `pipeline_experiment_config`:
+ If you don't specify `ExperimentName`, the pipeline `name` is used for the experiment name\.

  If you do specify `ExperimentName`, it's used for the experiment name\. If an experiment with that name exists, the pipeline\-created run groups are added to the existing experiment\. If an experiment with that name doesn't exist, a new experiment is created\.
+ If you don't specify `TrialName`, the pipeline execution ID is used for the run group name\.

  If you do specify `TrialName`, it's used for the run group name\. If a run group with that name exists, the pipeline\-created runs are added to the existing run group\. If a run group with that name doesn't exist, a new run group is created\.

**Note**  
The experiment entities aren't deleted when the pipeline that created the entities is deleted\. You can use the SageMaker Experiments API to delete the entities\. For more information, see [Clean Up Amazon SageMaker Experiment Resources](experiments-cleanup.md)\.

For information about how to view the SageMaker Experiment entities associated with a pipeline, see [View Experiment Entities Created by SageMaker Pipelines](pipelines-studio-experiments.md)\. For more information on SageMaker Experiments, see [Manage Machine Learning with Amazon SageMaker ExperimentsSupported AWS Regions](experiments.md)\.

The following sections show examples of the previous rules and how they are represented in the pipeline definition file\. For more information on pipeline definition files, see [SageMaker Pipelines Overview](pipelines-sdk.md)\.

**Topics**
+ [Default Behavior](#pipelines-experiments-default)
+ [Disable Experiments Integration](#pipelines-experiments-none)
+ [Specify a Custom Experiment Name](#pipelines-experiments-custom-experiment)
+ [Specify a Custom Run Group Name](#pipelines-experiments-custom-trial)

## Default Behavior<a name="pipelines-experiments-default"></a>

**Create a pipeline**

The `pipeline_experiment_config` is omitted\. `ExperimentName` defaults to the pipeline `name`\. `TrialName` defaults to the execution ID\.

```
pipeline_name = f"MyPipeline"
pipeline = Pipeline(
    name=pipeline_name,
    parameters=[...],
    steps=[step_train]
)
```

**Pipeline definition file**

```
{
  "Version": "2020-12-01",
  "Parameters": [
    {
      "Name": "InputDataSource"
    },
    {
      "Name": "InstanceCount",
      "Type": "Integer",
      "DefaultValue": 1
    }
  ],
  "PipelineExperimentConfig": {
    "ExperimentName": {"Get": "Execution.PipelineName"},
    "TrialName": {"Get": "Execution.PipelineExecutionId"}
  },
  "Steps": [...]
}
```

## Disable Experiments Integration<a name="pipelines-experiments-none"></a>

**Create a pipeline**

The `pipeline_experiment_config` is set to `None`\.

```
pipeline_name = f"MyPipeline"
pipeline = Pipeline(
    name=pipeline_name,
    parameters=[...],
    pipeline_experiment_config=None,
    steps=[step_train]
)
```

**Pipeline definition file**

This is the same as the preceding default example, without the `PipelineExperimentConfig`\.

## Specify a Custom Experiment Name<a name="pipelines-experiments-custom-experiment"></a>

A custom experiment name is used\. The run group name is set to the execution ID, as with the default behavior\.

**Create a pipeline**

```
pipeline_name = f"MyPipeline"
pipeline = Pipeline(
    name=pipeline_name,
    parameters=[...],
    pipeline_experiment_config=PipelineExperimentConfig(
      "CustomExperimentName",
      ExecutionVariables.PIPELINE_EXECUTION_ID
    ),
    steps=[step_train]
)
```

**Pipeline definition file**

```
{
  ...,
  "PipelineExperimentConfig": {
    "ExperimentName": "CustomExperimentName",
    "TrialName": {"Get": "Execution.PipelineExecutionId"}
  },
  "Steps": [...]
}
```

## Specify a Custom Run Group Name<a name="pipelines-experiments-custom-trial"></a>

A custom run group name is used and appended with the execution ID\. The experiment name is set to the pipeline name, as with the default behavior\.

**Create a pipeline**

```
pipeline_name = f"MyPipeline"
pipeline = Pipeline(
    name=pipeline_name,
    parameters=[...],
    pipeline_experiment_config=PipelineExperimentConfig(
      ExecutionVariables.PIPELINE_NAME,
      Join(on="-", values=["CustomTrialName", ExecutionVariables.PIPELINE_EXECUTION_ID])
    ),
    steps=[step_train]
)
```

**Pipeline definition file**

```
{
  ...,
  "PipelineExperimentConfig": {
    "ExperimentName": {"Get": "Execution.PipelineName"},
    "TrialName": {
      "On": "-",
      "Values": [
         "CustomTrialName",
         {"Get": "Execution.PipelineExecutionId"}
       ]
    }
  },
  "Steps": [...]
}
```