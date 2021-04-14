# Pipeline Structure<a name="build-and-manage-pipeline"></a>

 A pipeline instance is composed of a `name`, `parameters`, and `steps`\. Pipeline names must be unique within an `(account, region)` pair\. All parameters used in step definitions must be defined in the pipeline\. Steps passed into the pipeline don't need to be listed in the order of execution because the steps themselves define the relationships between them using data dependencies\. The SageMaker Pipelines service resolves the relationships between steps in the data dependency DAG to create a series of steps that the execution completes\. The following is an example of a pipeline structure\.

```
from sagemaker.workflow.pipeline import Pipeline

pipeline_name = f"AbalonePipeline"
pipeline = Pipeline(
    name=pipeline_name,
    parameters=[
        processing_instance_type, 
        processing_instance_count,
        training_instance_type,
        model_approval_status,
        input_data,
        batch_data,
    ],
    steps=[step_process, step_train, step_eval, step_cond],
)
```