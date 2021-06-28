# Pipeline Steps<a name="build-and-manage-steps"></a>

SageMaker Pipelines are composed of steps\. These steps define the actions that the pipeline takes, and the relationships between steps using properties\.

**Topics**
+ [Step Types](#build-and-manage-steps-types)
+ [Step Properties](#build-and-manage-properties)
+ [Data Dependency Between Steps](#build-and-manage-data-dependency)
+ [Custom Dependency Between Steps](#build-and-manage-custom-dependency)
+ [Use a Custom Image in a Step](#build-and-manage-images)

## Step Types<a name="build-and-manage-steps-types"></a>

The following describes the requirements of each step type and provides an example implementation of the step\. These are not functional implementations because they don't provide the resource and inputs needed\. For a tutorial that implements these steps, see [Create and Manage SageMaker Pipelines](pipelines-build.md)\.

Amazon SageMaker Model Building Pipelines support the following step types:
+ [Processing](#step-type-processing)
+ [Training](#step-type-training)
+ [Transform](#step-type-transform)
+ [CreateModel](#step-type-create-model)
+ [RegisterModel](#step-type-register-model)
+ [Condition](#step-type-condition)
+ [Callback](#step-type-callback)

### Processing Step<a name="step-type-processing"></a>

You use a processing step to create a processing job for data processing\. For more information on processing jobs, see [Process Data and Evaluate Models](https://docs.aws.amazon.com/sagemaker/latest/dg/processing-job.html)\.

A processing step requires a processor, a Python script that defines the processing code, outputs for processing, and job arguments\. The following example shows how to create a `ProcessingStep` definition\.  For more information on processing step requirements, see the [sagemaker\.workflow\.steps\.ProcessingStep](https://sagemaker.readthedocs.io/en/stable/workflows/pipelines/sagemaker.workflow.pipelines.html#sagemaker.workflow.steps.ProcessingStep) documentation\.

```
from sagemaker.sklearn.processing import SKLearnProcessor

sklearn_processor = SKLearnProcessor(framework_version='0.20.0',
                                     role=<role>,
                                     instance_type='ml.m5.xlarge',
                                     instance_count=1)
```

```
from sagemaker.processing import ProcessingInput, ProcessingOutput
from sagemaker.workflow.steps import ProcessingStep

step_process = ProcessingStep(
    name="AbaloneProcess",
    processor=sklearn_processor,
    inputs=[
      ProcessingInput(source=<input_data>, destination="/opt/ml/processing/input"),
    ],
    outputs=[
        ProcessingOutput(output_name="train", source="/opt/ml/processing/train"),
        ProcessingOutput(output_name="validation", source="/opt/ml/processing/validation"),
        ProcessingOutput(output_name="test", source="/opt/ml/processing/test")
    ],
    code="abalone/preprocessing.py"
)
```

**Pass runtime parameters**

You can pass runtime parameters to a processing step using the [get\_run\_args](https://sagemaker.readthedocs.io/en/stable/api/training/processing.html#sagemaker.processing.ScriptProcessor.get_run_args) method of the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\. This allows you to use processors besides `SKLearnProcessor` such as `PySparkProcessor` and `SparkJarProcessor`\.

The following example shows how to pass runtime parameters from a PySpark processor to a `ProcessingStep`\.

```
from sagemaker.spark.processing import PySparkProcessor

pyspark_processor = PySparkProcessor(framework_version='2.4',
                                     role=<role>,
                                     instance_type='ml.m5.xlarge',
                                     instance_count=1)
```

```
from sagemaker.processing import ProcessingInput, ProcessingOutput

run_args = pyspark_processor.get_run_args(
    "preprocess.py",
    inputs=[
      ProcessingInput(source=<input_data>, destination="/opt/ml/processing/input"),
    ],
    outputs=[
        ProcessingOutput(output_name="train", source="/opt/ml/processing/train"),
        ProcessingOutput(output_name="validation", source="/opt/ml/processing/validation"),
        ProcessingOutput(output_name="test", source="/opt/ml/processing/test")
    ],
    arguments=None
)
```

```
from sagemaker.workflow.steps import ProcessingStep

step_process = ProcessingStep(
    name="AbaloneProcess",
    processor=pyspark_processor,
    inputs=run_args.inputs,
    outputs=run_args.outputs,
    job_arguments=run_args.arguments,
    code=run_args.code
)
```

### Training Step<a name="step-type-training"></a>

You use a training step to create a training job to train a model\. For more information on training jobs, see [Train a Model with Amazon SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/how-it-works-training.html)\.

A training step requires an estimator, and training and validation data inputs\. The following example shows how to create a `TrainingStep` definition\.  For more information on Training step requirements, see the [sagemaker\.workflow\.steps\.TrainingStep](https://sagemaker.readthedocs.io/en/stable/workflows/pipelines/sagemaker.workflow.pipelines.html#sagemaker.workflow.steps.TrainingStep) documentation\.

```
from sagemaker.inputs import TrainingInput
from sagemaker.workflow.steps import TrainingStep

step_train = TrainingStep(
    name="TrainAbaloneModel",
    estimator=xgb_train,
    inputs={
        "train": TrainingInput(
            s3_data=step_process.properties.ProcessingOutputConfig.Outputs[
                "train"
            ].S3Output.S3Uri,
            content_type="text/csv"
        ),
        "validation": TrainingInput(
            s3_data=step_process.properties.ProcessingOutputConfig.Outputs[
                "validation"
            ].S3Output.S3Uri,
            content_type="text/csv"
        )
    }
)
```

### Transform Step<a name="step-type-transform"></a>

You use a transform step for batch transformation to run inference on an entire dataset\. For more information on batch transformation, see [Run Batch Transforms with Inference Pipelines ](https://docs.aws.amazon.com/sagemaker/latest/dg/inference-pipeline-batch.html)\.

A transform step requires a transformer, and the data to run batch transformation on\. The following example shows how to create a `Transform` step definition\.  For more information on `Transform` step requirements, see the [sagemaker\.workflow\.steps\.TransformStep](https://sagemaker.readthedocs.io/en/stable/workflows/pipelines/sagemaker.workflow.pipelines.html#sagemaker.workflow.steps.TransformStep)\. documentation\.

```
from sagemaker.inputs import TransformInput
from sagemaker.workflow.steps import TransformStep

step_transform = TransformStep(
    name="AbaloneTransform",
    transformer=transformer,
    inputs=TransformInput(data=batch_data)
)
```

### CreateModel Step<a name="step-type-create-model"></a>

You use a CreateModel step to create a SageMaker Model\. For more information on SageMaker Models, see [Train a Model with Amazon SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/how-it-works-training.html)\.

A CreateModel step requires model artifacts, and information on the SageMaker instance type that you need to use to create the model\. The following example shows how to create a `CreateModel` step definition\.  For more information on `CreateModel` step requirements, see the [sagemaker\.workflow\.steps\.CreateModelStep](https://sagemaker.readthedocs.io/en/stable/workflows/pipelines/sagemaker.workflow.pipelines.html#sagemaker.workflow.steps.CreateModelStep) documentation\.

```
from sagemaker.workflow.steps import CreateModelStep

step_create_model = CreateModelStep(
    name="AbaloneCreateModel",
    model=model,
    inputs=inputs
)
```

### RegisterModel Step<a name="step-type-register-model"></a>

You use a RegisterModel step to register a model to a model group\. For more information on registering models, see [Register and Deploy Models with Model Registry](model-registry.md)\.

A RegisterModel step requires an estimator, model data output from training, and a model package group name to associate the model package with\. The following example shows how to create a `RegisterModel` definition\.  For more information on `RegisterModel` step requirements, see the [sagemaker\.workflow\.step\_collections\.RegisterModel](https://sagemaker.readthedocs.io/en/stable/workflows/pipelines/sagemaker.workflow.pipelines.html#sagemaker.workflow.step_collections.RegisterModel) documentation\.

```
from sagemaker.workflow.step_collections import RegisterModel

step_register = RegisterModel(
    name="AbaloneRegisterModel",
    estimator=xgb_train,
    model_data=step_train.properties.ModelArtifacts.S3ModelArtifacts,
    content_types=["text/csv"],
    response_types=["text/csv"],
    inference_instances=["ml.t2.medium", "ml.m5.xlarge"],
    transform_instances=["ml.m5.xlarge"],
    model_package_group_name=model_package_group_name,
    approval_status=model_approval_status,
    model_metrics=model_metrics
)
```

### Condition Step<a name="step-type-condition"></a>

You use a condition step to evaluate the condition of step properties to assess which action should be taken next in the pipeline\.

A condition step requires a list of conditions, and a list of steps to execute if the condition evaluates to `true` and a list of steps to execute if the condition evaluates to `false`\. The following example shows how to create a `Condition` step definition\.  For more information on `Condition` step requirements, see the [sagemaker\.workflow\.condition\_step\.ConditionStep](https://sagemaker.readthedocs.io/en/stable/workflows/pipelines/sagemaker.workflow.pipelines.html#conditionstep) documentation\.

**Limitations**
+ SageMaker Pipelines doesn't support the use of nested condition steps\. You can't pass a condition step as the input for another condition step\.
+ A condition step can't use identical steps in both branches\. If you need the same step functionality in both branches, duplicate the step and give it a different name\.

```
from sagemaker.workflow.conditions import ConditionLessThanOrEqualTo
from sagemaker.workflow.condition_step import (
    ConditionStep,
    JsonGet
)

cond_lte = ConditionLessThanOrEqualTo(
    left=JsonGet(
        step=step_eval,
        property_file=evaluation_report,
        json_path="regression_metrics.mse.value"
    ),
    right=6.0
)

step_cond = ConditionStep(
    name="AbaloneMSECond",
    conditions=[cond_lte],
    if_steps=[step_register, step_create_model, step_transform],
    else_steps=[]
)
```

### Callback Step<a name="step-type-callback"></a>

You can use a callback step to incorporate additional processes and AWS services into your workflow that aren't directly provided by Amazon SageMaker Model Building Pipelines\. When a callback step runs, the following procedure occurs:
+ SageMaker Pipelines sends a message to a customer\-specified Amazon Simple Queue Service \(Amazon SQS\) queue\. The message contains a SageMaker Pipelines–generated token and a customer\-supplied list of input parameters\. After sending the message, SageMaker Pipelines waits for a response from the customer\.
+ The customer retrieves the message from the Amazon SQS queue and starts their custom process\.
+ When the process finishes, the customer calls one of the following APIs and submits the SageMaker Pipelines–generated token:
  +  [SendPipelineExecutionStepSuccess](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_SendPipelineExecutionStepSuccess.html) – along with a list of output parameters
  +  [SendPipelineExecutionStepFailure](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_SendPipelineExecutionStepFailure.html) – along with a failure reason
+ The API call causes SageMaker Pipelines to either continue the pipeline execution or fail the execution\.

For more information on `Callback` step requirements, see the  [sagemaker\.workflow\.callback\_step](https://github.com/aws/sagemaker-python-sdk/blob/master/src/sagemaker/workflow/callback_step.py) documentation\.

**Stopping Behavior**

A pipeline execution won't stop while a callback step is running\.

When you call [StopPipelineExecution](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_StopPipelineExecution.html) on a pipeline execution with a running callback step, SageMaker Pipelines sends an additional Amazon SQS message to the specified SQS queue\. The body of the SQS message contains a "Status" field which is set to "Stopping"\.

You should add logic to your Amazon SQS message consumer to take any needed action \(for example, resource cleanup\) upon receipt of the message followed by a call to `SendPipelineExecutionStepSuccess` or `SendPipelineExecutionStepFailure`\.

Only when SageMaker Pipelines receives one of these calls will it stop the pipeline execution\.

**Important**  
Callback steps were introduced in Amazon SageMaker Python SDK v2\.45\.0 and Amazon SageMaker Studio v3\.6\.2\. You must update Studio before you use a callback step or the pipeline DAG doesn't display\. To update Studio, see [Update SageMaker Studio](studio-tasks-update-studio.md)\.

The following sample demonstrates an implementation of the preceding procedure\.

```
from sagemaker.workflow.callback_step import CallbackStep

step_callback = CallbackStep(
    name="MyCallbackStep",
    sqs_queue_url="https://sqs.us-east-2.amazonaws.com/012345678901/MyCallbackQueue",
    inputs={...},
    outputs=[...]
)

callback_handler_code = '
    import boto3
    import json

    def handler(event, context):
        sagemaker_client=boto3.client("sagemaker")

        for record in event["Records"]:
            payload=json.loads(record["body"])
            token=payload["token"]

            # Custom processing

            # Call SageMaker to complete the step
            sagemaker_client.send_pipeline_execution_step_success(
                CallbackToken=token,
                OutputParameters={...}
            )
'
```

**Stopping Behavior**

A pipeline execution won't stop while a callback step is running\.

When you call [StopPipelineExecution](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_StopPipelineExecution.html) on a pipeline execution with a running callback step, SageMaker Pipelines sends an additional Amazon SQS message to the specified SQS queue\. The body of the SQS message contains a "Status" field which is set to "Stopping"\.

You should add logic to your Amazon SQS message consumer to take any needed action \(for example, resource cleanup\) upon receipt of the message followed by a call to `SendPipelineExecutionStepSuccess` or `SendPipelineExecutionStepFailure`\.

Only when SageMaker Pipelines receives one of these calls will it stop the pipeline execution\.

## Step Properties<a name="build-and-manage-properties"></a>

The `properties` attribute is used to add data dependencies between steps in the pipeline\. These data dependencies are then used by SageMaker Pipelines to construct the DAG from the pipeline definition\. These properties can be referenced as placeholder values and are resolved at runtime\.  

The `properties` attribute of a SageMaker Pipelines step matches the object returned by a `Describe` call for the corresponding SageMaker job type\. For each job type, the `Describe` call returns the following response object:
+ `ProcessingStep` – [DescribeProcessingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeProcessingJob.html)
+ `TrainingStep` – [DescribeTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeTrainingJob.html)
+ `TransformStep` – [DescribeTransformJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeTransformJob.html)

## Data Dependency Between Steps<a name="build-and-manage-data-dependency"></a>

You define the structure of your DAG by specifying the data relationships between steps\. To create data dependencies between steps, pass the properties of one step as the input to another step in the pipeline\. The step receiving the input isn't started until after the step providing the input finishes execution\.

A data dependency uses JsonPath notation in the following format\. This format traverses the JSON property file, which means you can append as many *<property>* instances as needed to reach the desired nested property in the file\. For more information on JsonPath notation, see the [JsonPath repo](https://github.com/json-path/JsonPath)\.

```
<step_name>.properties.<property>.<property>
```

The following shows how to specify an Amazon S3 bucket using the `ProcessingOutputConfig` property of a processing step\.

```
step_process.properties.ProcessingOutputConfig.Outputs["train_data"].S3Output.S3Uri
```

To create the data dependency, pass the bucket to a training step as follows\.

```
step_train = TrainingStep(
    name="CensusTrain",
    estimator=sklearn_train,
    inputs=TrainingInput(
        s3_data=step_process.properties.ProcessingOutputConfig.Outputs[
            "train_data"].S3Output.S3Uri
    )
)
```

## Custom Dependency Between Steps<a name="build-and-manage-custom-dependency"></a>

When you specify a data dependency, SageMaker Pipelines provides the data connection between the steps\. Alternatively, one step can access the data from a previous step without directly using SageMaker Pipelines\. In this case, you can create a custom dependency that tells SageMaker Pipelines not to start a step until after another step has finished executing\. You create a custom dependency by specifying a step's `DependsOn` attribute\.

As an example, the following defines a step `C` that starts only after both step `A` and step `B` finish executing\.

```
{
  'Steps': [
    {'Name':'A', ...},
    {'Name':'B', ...},
    {'Name':'C', 'DependsOn': ['A', 'B']}
  ]
}
```

SageMaker Pipelines throws a validation exception if the dependency would create a cyclic dependency\.

The following example creates a training step that starts after a processing step finishes executing\.

```
processing_step = ProcessingStep(...)
training_step = TrainingStep(...)

training_step.add_depends_on([processing_step])
```

The following example creates a training step that doesn't start until two different processing steps finish executing\.

```
processing_step_1 = ProcessingStep(...)
processing_step_2 = ProcessingStep(...)

training_step = TrainingStep(...)

training_step.add_depends_on([processing_step_1, processing_step_2])
```

The following provides an alternate way to create the custom dependency\.

```
training_step.add_depends_on([processing_step_1])
training_step.add_depends_on([processing_step_2])
```

The following example creates a training step that receives input from one processing step and waits for a different processing step to finish executing\.

```
processing_step_1 = ProcessingStep(...)
processing_step_2 = ProcessingStep(...)

training_step = TrainingStep(
    ...,
    inputs=TrainingInput(
        s3_data=processing_step_1.properties.ProcessingOutputConfig.Outputs[
            "train_data"
        ].S3Output.S3Uri
    )

training_step.add_depends_on([processing_step_2])
```

The following example shows how to retrieve a string list of the custom dependencies of a step\.

```
custom_dependencies = training_step.depends_on()
```

## Use a Custom Image in a Step<a name="build-and-manage-images"></a>

 You can use any of the available SageMaker [Deep Learning Container images](https://github.com/aws/deep-learning-containers/blob/master/available_images.md) when you create a step in your pipeline\. 

You can also create a step using SageMaker S3 applications\. A SageMaker S3 application is a tar\.gz bundle with one or more Python scripts that can run within that bundle\. For more information on application package bundling, see [Deploying directly from model artifacts](https://sagemaker.readthedocs.io/en/stable/frameworks/tensorflow/using_tf.html?highlight=packaging#id22)\. 

 You can also use your own container with pipeline steps\. Because you can’t create an image from within SageMaker Studio, you must create your image using another method before using it with Amazon SageMaker Model Building Pipelines\.

 To use your own container when creating the steps for your pipeline, include the image URI in the estimator definition\. For more information on using your own container with SageMaker, see [Using Docker Containers with SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/docker-containers.html)\.