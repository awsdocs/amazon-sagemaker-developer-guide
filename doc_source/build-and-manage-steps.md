# Pipeline Steps<a name="build-and-manage-steps"></a>

SageMaker Pipelines are composed of steps\. These steps define the actions that the pipeline takes and the relationships between steps using properties\.

**Topics**
+ [Step Types](#build-and-manage-steps-types)
+ [Step Properties](#build-and-manage-properties)
+ [Step Parallelism](#build-and-manage-parallelism)
+ [Data Dependency Between Steps](#build-and-manage-data-dependency)
+ [Custom Dependency Between Steps](#build-and-manage-custom-dependency)
+ [Use a Custom Image in a Step](#build-and-manage-images)

## Step Types<a name="build-and-manage-steps-types"></a>

The following describes the requirements of each step type and provides an example implementation of the step\. These are not functional implementations because they don't provide the resource and inputs needed\. For a tutorial that implements these steps, see [Create and Manage SageMaker Pipelines](pipelines-build.md)\.

Amazon SageMaker Model Building Pipelines support the following step types:
+ [Processing](#step-type-processing)
+ [Training](#step-type-training)
+ [Tuning](#step-type-tuning)
+ [AutoML](#step-type-automl)
+ [`Model`](#step-type-model)
+ [`CreateModel`](#step-type-create-model)
+ [`RegisterModel`](#step-type-register-model)
+ [Transform](#step-type-transform)
+ [Condition](#step-type-condition)
+ [`Callback`](#step-type-callback)
+ [Lambda](#step-type-lambda)
+ [`ClarifyCheck`](#step-type-clarify-check)
+ [`QualityCheck`](#step-type-quality-check)
+ [EMR](#step-type-emr)
+ [Fail](#step-type-fail)

### Processing Step<a name="step-type-processing"></a>

Use a processing step to create a processing job for data processing\. For more information on processing jobs, see [Process Data and Evaluate Models](https://docs.aws.amazon.com/sagemaker/latest/dg/processing-job.html)\.

A processing step requires a processor, a Python script that defines the processing code, outputs for processing, and job arguments\. The following example shows how to create a `ProcessingStep` definition\. 

```
from sagemaker.sklearn.processing import SKLearnProcessor

sklearn_processor = SKLearnProcessor(framework_version='1.0-1',
                                     role=<role>,
                                     instance_type='ml.m5.xlarge',
                                     instance_count=1)
```

```
from sagemaker.processing import ProcessingInput, ProcessingOutput
from sagemaker.workflow.steps import ProcessingStep

inputs = [
    ProcessingInput(source=<input_data>, destination="/opt/ml/processing/input"),
]

outputs = [
    ProcessingOutput(output_name="train", source="/opt/ml/processing/train"),
    ProcessingOutput(output_name="validation", source="/opt/ml/processing/validation"),
    ProcessingOutput(output_name="test", source="/opt/ml/processing/test")
]

step_process = ProcessingStep(
    name="AbaloneProcess",
    step_args = sklearn_processor.run(inputs=inputs, outputs=outputs,
        code="abalone/preprocessing.py")
)
```

**Pass runtime parameters**

The following example shows how to pass runtime parameters from a PySpark processor to a `ProcessingStep`\.

```
from sagemaker.workflow.pipeline_context import PipelineSession
from sagemaker.spark.processing import PySparkProcessor
from sagemaker.processing import ProcessingInput, ProcessingOutput
from sagemaker.workflow.steps import ProcessingStep

pipeline_session = PipelineSession()

pyspark_processor = PySparkProcessor(
    framework_version='2.4',
    role=<role>,
    instance_type='ml.m5.xlarge',
    instance_count=1,
    sagemaker_session=pipeline_session,
)

step_args = pyspark_processor.run(
    inputs=[ProcessingInput(source=<input_data>, destination="/opt/ml/processing/input"),],
    outputs=[
        ProcessingOutput(output_name="train", source="/opt/ml/processing/train"),
        ProcessingOutput(output_name="validation", source="/opt/ml/processing/validation"),
        ProcessingOutput(output_name="test", source="/opt/ml/processing/test")
    ],
    code="preprocess.py",
    arguments=None,
)


step_process = ProcessingStep(
    name="AbaloneProcess",
    step_args=step_args,
)
```

For more information on processing step requirements, see the [sagemaker\.workflow\.steps\.ProcessingStep](https://sagemaker.readthedocs.io/en/stable/workflows/pipelines/sagemaker.workflow.pipelines.html#sagemaker.workflow.steps.ProcessingStep) documentation\. For an in\-depth example, see *Define a Processing Step for Feature Engineering* in the [Orchestrate Jobs to Train and Evaluate Models with Amazon SageMaker Pipelines](https://github.com/aws/amazon-sagemaker-examples/blob/62de6a1fca74c7e70089d77e36f1356033adbe5f/sagemaker-pipelines/tabular/abalone_build_train_deploy/sagemaker-pipelines-preprocess-train-evaluate-batch-transform.ipynb) example notebook\.

### Training Step<a name="step-type-training"></a>

You use a training step to create a training job to train a model\. For more information on training jobs, see [Train a Model with Amazon SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/how-it-works-training.html)\.

A training step requires an estimator, as well as training and validation data inputs\. The following example shows how to create a `TrainingStep` definition\. For more information on training step requirements, see the [sagemaker\.workflow\.steps\.TrainingStep](https://sagemaker.readthedocs.io/en/stable/workflows/pipelines/sagemaker.workflow.pipelines.html#sagemaker.workflow.steps.TrainingStep) documentation\.

```
from sagemaker.workflow.pipeline_context import PipelineSession

from sagemaker.inputs import TrainingInput
from sagemaker.workflow.steps import TrainingStep

from sagemaker.xgboost.estimator import XGBoost

pipeline_session = PipelineSession()

xgb_estimator = XGBoost(..., sagemaker_session=pipeline_session)

step_args = xgb_estimator.fit(
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

step_train = TrainingStep(
    name="TrainAbaloneModel",
    step_args=step_args,
)
```

### Tuning Step<a name="step-type-tuning"></a>

You use a tuning step to create a hyperparameter tuning job, also known as hyperparameter optimization \(HPO\)\. A hyperparameter tuning job runs multiple training jobs, each one producing a model version\. For more information on hyperparameter tuning, see [Perform Automatic Model Tuning with SageMaker](automatic-model-tuning.md)\.

The tuning job is associated with the SageMaker experiment for the pipeline, with the training jobs created as trials\. For more information, see [Experiments Integration](pipelines-experiments.md)\.

A tuning step requires a [HyperparameterTuner](https://sagemaker.readthedocs.io/en/stable/api/training/tuner.html) and training inputs\. You can retrain previous tuning jobs by specifying the `warm_start_config` parameter of the `HyperparameterTuner`\. For more information on hyperparameter tuning and warm start, see [Run a Warm Start Hyperparameter Tuning Job](automatic-model-tuning-warm-start.md)\.

You use the [get\_top\_model\_s3\_uri](https://sagemaker.readthedocs.io/en/stable/workflows/pipelines/sagemaker.workflow.pipelines.html#sagemaker.workflow.steps.TuningStep.get_top_model_s3_uri) method of the [sagemaker\.workflow\.steps\.TuningStep](https://sagemaker.readthedocs.io/en/stable/workflows/pipelines/sagemaker.workflow.pipelines.html#sagemaker.workflow.steps.TuningStep) class to get the model artifact from one of the top\-performing model versions\. For a notebook that shows how to use a tuning step in a SageMaker pipeline, see [sagemaker\-pipelines\-tuning\-step\.ipynb](https://github.com/aws/amazon-sagemaker-examples/blob/master/sagemaker-pipelines/tabular/tuning-step/sagemaker-pipelines-tuning-step.ipynb)\.

**Important**  
Tuning steps were introduced in Amazon SageMaker Python SDK v2\.48\.0 and Amazon SageMaker Studio v3\.8\.0\. You must update Studio before you use a tuning step or the pipeline DAG doesn't display\. To update Studio, see [Shut down and Update SageMaker Studio](studio-tasks-update-studio.md)\.

The following example shows how to create a `TuningStep` definition\.

```
from sagemaker.workflow.pipeline_context import PipelineSession

from sagemaker.tuner import HyperparameterTuner
from sagemaker.inputs import TrainingInput
from sagemaker.workflow.steps import TuningStep

tuner = HyperparameterTuner(..., sagemaker_session=PipelineSession())
    
step_tuning = TuningStep(
    name = "HPTuning",
    step_args = tuner.fit(inputs=TrainingInput(s3_data="s3://my-bucket/my-data"))
)
```

**Get the best model version**

The following example shows how to get the best model version from the tuning job using the `get_top_model_s3_uri` method\. At most, the top 50 performing versions are available ranked according to [HyperParameterTuningJobObjective](https://docs.aws.amazon.com/sagemaker/latest/APIReference/HyperParameterTuningJobObjective.html)\. The `top_k` argument is an index into the versions, where `top_k=0` is the best\-performing version and `top_k=49` is the worst\-performing version\.

```
best_model = Model(
    image_uri=image_uri,
    model_data=step_tuning.get_top_model_s3_uri(
        top_k=0,
        s3_bucket=sagemaker_session.default_bucket()
    ),
    ...
)
```

For more information on tuning step requirements, see the [sagemaker\.workflow\.steps\.TuningStep](https://sagemaker.readthedocs.io/en/stable/workflows/pipelines/sagemaker.workflow.pipelines.html#sagemaker.workflow.steps.TuningStep)  documentation\.

### AutoML Step<a name="step-type-automl"></a>

Use the [AutoML](https://sagemaker.readthedocs.io/en/stable/api/training/automl.html) API to create an AutoML job to automatically train a model\. For more information on AutoML jobs, see [Automate model development with Amazon SageMaker Autopilot](https://docs.aws.amazon.com/sagemaker/latest/dg/autopilot-automate-model-development.html)\. 

**Note**  
Currently, the AutoML step supports only [ensembling training mode](https://docs.aws.amazon.com/sagemaker/latest/dg/autopilot-model-support-validation.html)\.

The following example shows how to create a definition using `AutoMLStep`\.

```
from sagemaker.workflow.pipeline_context import PipelineSession
from sagemaker.workflow.automl_step import AutoMLStep

pipeline_session = PipelineSession()

auto_ml = AutoML(...,
    role="<role>",
    target_attribute_name="my_target_attribute_name",
    mode="ENSEMBLING",
    sagemaker_session=pipeline_session) 

input_training = AutoMLInput(
    inputs="s3://my-bucket/my-training-data",
    target_attribute_name="my_target_attribute_name",
    channel_type="training",
)
input_validation = AutoMLInput(
    inputs="s3://my-bucket/my-validation-data",
    target_attribute_name="my_target_attribute_name",
    channel_type="validation",
)

step_args = auto_ml.fit(
    inputs=[input_training, input_validation]
)

step_automl = AutoMLStep(
    name="AutoMLStep",
    step_args=step_args,
)
```

**Get the best model version**

The AutoML step automatically trains several model candidates\. You can get the model with the best objective metric from the AutoML job using the `get_best_auto_ml_model` method and an IAM `role` to access model artifacts as follows\.

```
best_model = step_automl.get_best_auto_ml_model(role=<role>)
```

For more information, see the [AutoML](https://sagemaker.readthedocs.io/en/stable/workflows/pipelines/sagemaker.workflow.pipelines.html#sagemaker.workflow.automl_step.AutoMLStep) step in the SageMaker Python SDK\.

### Model Step<a name="step-type-model"></a>

Use a `ModelStep` to create or register a SageMaker model\. For more information on `ModelStep` requirements, see the [sagemaker\.workflow\.model\_step\.ModelStep](https://sagemaker.readthedocs.io/en/stable/workflows/pipelines/sagemaker.workflow.pipelines.html#sagemaker.workflow.model_step.ModelStep) documentation\.

#### Create a model<a name="step-type-model-create"></a>

You can use a `ModelStep` to create a SageMaker model\. A `ModelStep` requires model artifacts and information about the SageMaker instance type that you need to use to create the model\. For more information on SageMaker models, see [Train a Model with Amazon SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/how-it-works-training.html)\.

The following example shows how to create a `ModelStep` definition\.

```
from sagemaker.workflow.pipeline_context import PipelineSession
from sagemaker.model import Model
from sagemaker.workflow.model_step import ModelStep

step_train = TrainingStep(...)
model = Model(
    image_uri=pytorch_estimator.training_image_uri(),
    model_data=step_train.properties.ModelArtifacts.S3ModelArtifacts,
    sagemaker_session=PipelineSession(),
    role=role,
)

step_model_create = ModelStep(
   name="MyModelCreationStep",
   step_args=model.create(instance_type="ml.m5.xlarge"),
)
```

#### Register a model<a name="step-type-model-register"></a>

You can use a `ModelStep` to register a `sagemaker.model.Model` or a `sagemaker.pipeline.PipelineModel` with the Amazon SageMaker model registry\. A `PipelineModel` represents an inference pipeline, which is a model composed of a linear sequence of containers that process inference requests\. For more information about how to register a model, see [Register and Deploy Models with Model Registry](model-registry.md)\.

The following example shows how to create a `ModelStep` that registers a `PipelineModel`\.

```
import time

from sagemaker.workflow.pipeline_context import PipelineSession
from sagemaker.sklearn import SKLearnModel
from sagemaker.xgboost import XGBoostModel

pipeline_session = PipelineSession()

code_location = 's3://{0}/{1}/code'.format(bucket_name, prefix)

sklearn_model = SKLearnModel(
   model_data=processing_step.properties.ProcessingOutputConfig.Outputs['model'].S3Output.S3Uri,
   entry_point='inference.py',
   source_dir='sklearn_source_dir/',
   code_location=code_location,
   framework_version='1.0-1',
   role=role,
   sagemaker_session=pipeline_session,
   py_version='py3'
)

xgboost_model = XGBoostModel(
   model_data=training_step.properties.ModelArtifacts.S3ModelArtifacts,
   entry_point='inference.py',
   source_dir='xgboost_source_dir/',
   code_location=code_location,
   framework_version='0.90-2',
   py_version='py3',
   sagemaker_session=pipeline_session,
   role=role
)

from sagemaker.workflow.model_step import ModelStep
from sagemaker import PipelineModel

pipeline_model = PipelineModel(
   models=[sklearn_model, xgboost_model],
   role=role,sagemaker_session=pipeline_session,
)

register_model_step_args = pipeline_model.register(
    content_types=["application/json"],
   response_types=["application/json"],
   inference_instances=["ml.t2.medium", "ml.m5.xlarge"],
   transform_instances=["ml.m5.xlarge"],
   model_package_group_name='sipgroup',
)

step_model_registration = ModelStep(
   name="AbaloneRegisterModel",
   step_args=register_model_step_args,
)
```

### CreateModel Step<a name="step-type-create-model"></a>

**Important**  
We recommend using [Model Step](#step-type-model) to create models as of v2\.90\.0 of the SageMaker Python SDK\. `CreateModelStep` will continue to work in previous versions of the SageMaker Python SDK, but is no longer actively supported\.

You use a `CreateModel` step to create a SageMaker model\. For more information on SageMaker models, see [Train a Model with Amazon SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/how-it-works-training.html)\.

A create model step requires model artifacts and information about the SageMaker instance type that you need to use to create the model\. The following example shows how to create a `CreateModel` step definition\. For more information on `CreateModel` step requirements, see the [sagemaker\.workflow\.steps\.CreateModelStep](https://sagemaker.readthedocs.io/en/stable/workflows/pipelines/sagemaker.workflow.pipelines.html#sagemaker.workflow.steps.CreateModelStep) documentation\.

```
from sagemaker.workflow.steps import CreateModelStep

step_create_model = CreateModelStep(
    name="AbaloneCreateModel",
    model=best_model,
    inputs=inputs
)
```

### RegisterModel Step<a name="step-type-register-model"></a>

**Important**  
We recommend using [Model Step](#step-type-model) to register models as of v2\.90\.0 of the SageMaker Python SDK\. `RegisterModel` will continue to work in previous versions of the SageMaker Python SDK, but is no longer actively supported\.

You use a `RegisterModel` step to register a [sagemaker\.model\.Model](https://sagemaker.readthedocs.io/en/stable/api/inference/model.html) or a [sagemaker\.pipeline\.PipelineModel](https://sagemaker.readthedocs.io/en/stable/api/inference/pipeline.html#pipelinemodel) with the Amazon SageMaker model registry\. A `PipelineModel` represents an inference pipeline, which is a model composed of a linear sequence of containers that process inference requests\.

For more information about how to register a model, see [Register and Deploy Models with Model Registry](model-registry.md)\. For more information on `RegisterModel` step requirements, see the [sagemaker\.workflow\.step\_collections\.RegisterModel](https://sagemaker.readthedocs.io/en/stable/workflows/pipelines/sagemaker.workflow.pipelines.html#sagemaker.workflow.step_collections.RegisterModel) documentation\.

The following example shows how to create a `RegisterModel` step that registers a `PipelineModel`\.

```
import time
from sagemaker.sklearn import SKLearnModel
from sagemaker.xgboost import XGBoostModel

code_location = 's3://{0}/{1}/code'.format(bucket_name, prefix)

sklearn_model = SKLearnModel(model_data=processing_step.properties.ProcessingOutputConfig.Outputs['model'].S3Output.S3Uri,
 entry_point='inference.py',
 source_dir='sklearn_source_dir/',
 code_location=code_location,
 framework_version='1.0-1',
 role=role,
 sagemaker_session=sagemaker_session,
 py_version='py3')

xgboost_model = XGBoostModel(model_data=training_step.properties.ModelArtifacts.S3ModelArtifacts,
 entry_point='inference.py',
 source_dir='xgboost_source_dir/',
 code_location=code_location,
 framework_version='0.90-2',
 py_version='py3',
 sagemaker_session=sagemaker_session,
 role=role)

from sagemaker.workflow.step_collections import RegisterModel
from sagemaker import PipelineModel
pipeline_model = PipelineModel(models=[sklearn_model,xgboost_model],role=role,sagemaker_session=sagemaker_session)

step_register = RegisterModel(
 name="AbaloneRegisterModel",
 model=pipeline_model,
 content_types=["application/json"],
 response_types=["application/json"],
 inference_instances=["ml.t2.medium", "ml.m5.xlarge"],
 transform_instances=["ml.m5.xlarge"],
 model_package_group_name='sipgroup',
)
```

If `model` isn't provided, the register model step requires an estimator as shown in the following example\.

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

### Transform Step<a name="step-type-transform"></a>

You use a transform step for batch transformation to run inference on an entire dataset\. For more information about batch transformation, see [Run Batch Transforms with Inference Pipelines](inference-pipeline-batch.md)\.

A transform step requires a transformer and the data on which to run batch transformation\. The following example shows how to create a `Transform` step definition\. For more information on `Transform` step requirements, see the [sagemaker\.workflow\.steps\.TransformStep](https://sagemaker.readthedocs.io/en/stable/workflows/pipelines/sagemaker.workflow.pipelines.html#sagemaker.workflow.steps.TransformStep) documentation\.

```
from sagemaker.workflow.pipeline_context import PipelineSession

from sagemaker.transformer import Transformer
from sagemaker.inputs import TransformInput
from sagemaker.workflow.steps import TransformStep

transformer = Transformer(..., sagemaker_session=PipelineSession())

step_transform = TransformStep(
    name="AbaloneTransform",
    step_args=transformer.transform(data="s3://my-bucket/my-data"),
)
```

### Condition Step<a name="step-type-condition"></a>

You use a condition step to evaluate the condition of step properties to assess which action should be taken next in the pipeline\.

A condition step requires a list of conditions, a list of steps to run if the condition evaluates to `true`, and a list of steps to run if the condition evaluates to `false`\. The following example shows how to create a `ConditionStep` definition\. 

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
        step_name=step_eval.name,
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

For more information on `ConditionStep` requirements, see the [sagemaker\.workflow\.condition\_step\.ConditionStep](https://sagemaker.readthedocs.io/en/stable/workflows/pipelines/sagemaker.workflow.pipelines.html#conditionstep) API reference\. For more information on supported conditions, see *[Amazon SageMaker Model Building Pipelines \- Conditions](https://sagemaker.readthedocs.io/en/stable/amazon_sagemaker_model_building_pipeline.html#conditions)* in the SageMaker Python SDK documentation\. 

### Callback Step<a name="step-type-callback"></a>

You use a `Callback` step to incorporate additional processes and AWS services into your workflow that aren't directly provided by Amazon SageMaker Model Building Pipelines\. When a `Callback` step runs, the following procedure occurs:
+ SageMaker Pipelines sends a message to a customer\-specified Amazon Simple Queue Service \(Amazon SQS\) queue\. The message contains a SageMaker Pipelines–generated token and a customer\-supplied list of input parameters\. After sending the message, SageMaker Pipelines waits for a response from the customer\.
+ The customer retrieves the message from the Amazon SQS queue and starts their custom process\.
+ When the process finishes, the customer calls one of the following APIs and submits the SageMaker Pipelines–generated token:
  +  [SendPipelineExecutionStepSuccess](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_SendPipelineExecutionStepSuccess.html), along with a list of output parameters
  +  [SendPipelineExecutionStepFailure](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_SendPipelineExecutionStepFailure.html), along with a failure reason
+ The API call causes SageMaker Pipelines to either continue the pipeline process or fail the process\.

For more information on `Callback` step requirements, see the [sagemaker\.workflow\.callback\_step\.CallbackStep](https://sagemaker.readthedocs.io/en/stable/workflows/pipelines/sagemaker.workflow.pipelines.html#sagemaker.workflow.callback_step.CallbackStep) documentation\. For a complete solution, see [Extend SageMaker Pipelines to include custom steps using callback steps](http://aws.amazon.com/blogs/machine-learning/extend-amazon-sagemaker-pipelines-to-include-custom-steps-using-callback-steps/)\.

**Important**  
`Callback` steps were introduced in Amazon SageMaker Python SDK v2\.45\.0 and Amazon SageMaker Studio v3\.6\.2\. You must update Studio before you use a `Callback` step or the pipeline DAG doesn't display\. To update Studio, see [Shut down and Update SageMaker Studio](studio-tasks-update-studio.md)\.

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

**Note**  
Output parameters for `CallbackStep` should not be nested\. For example, if you use a nested dictionary as your output parameter, then the dictionary is treated as a single string \(ex\. `{"output1": "{\"nested_output1\":\"my-output\"}"}`\)\. If you provide a nested value, then when you try to refer to a particular output parameter, a non\-retryable client error is thrown\.

**Stopping behavior**

A pipeline process doesn't stop while a `Callback` step is running\.

When you call [StopPipelineExecution](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_StopPipelineExecution.html) on a pipeline process with a running `Callback` step, SageMaker Pipelines sends an additional Amazon SQS message to the specified SQS queue\. The body of the SQS message contains a **Status** field, which is set to `Stopping`\. The following shows an example SQS message body\.

```
{
  "token": "26vcYbeWsZ",
  "pipelineExecutionArn": "arn:aws:sagemaker:us-east-2:012345678901:pipeline/callback-pipeline/execution/7pinimwddh3a",
  "arguments": {
    "number": 5,
    "stringArg": "some-arg",
    "inputData": "s3://sagemaker-us-west-2-012345678901/abalone/abalone-dataset.csv"
  },
  "status": "Stopping"
}
```

You should add logic to your Amazon SQS message consumer to take any needed action \(for example, resource cleanup\) upon receipt of the message, followed by a call to `SendPipelineExecutionStepSuccess` or `SendPipelineExecutionStepFailure`\.

Only when SageMaker Pipelines receives one of these calls does it stop the pipeline process\.

### Lambda Step<a name="step-type-lambda"></a>

You use a Lambda step to run an AWS Lambda function\. You can run an existing Lambda function, or SageMaker can create and run a new Lambda function\. For a notebook that shows how to use a Lambda step in a SageMaker pipeline, see [sagemaker\-pipelines\-lambda\-step\.ipynb](https://github.com/aws/amazon-sagemaker-examples/blob/master/sagemaker-pipelines/tabular/lambda-step/sagemaker-pipelines-lambda-step.ipynb)\.

**Important**  
Lambda steps were introduced in Amazon SageMaker Python SDK v2\.51\.0 and Amazon SageMaker Studio v3\.9\.1\. You must update Studio before you use a Lambda step or the pipeline DAG doesn't display\. To update Studio, see [Shut down and Update SageMaker Studio](studio-tasks-update-studio.md)\.

SageMaker provides the [sagemaker\.lambda\_helper\.Lambda](https://sagemaker.readthedocs.io/en/stable/api/utility/lambda_helper.html) class to create, update, invoke, and delete Lambda functions\. `Lambda` has the following signature\.

```
Lambda(
    function_arn,       # Only required argument to invoke an existing Lambda function

    # The following arguments are required to create a Lambda function:
    function_name,
    execution_role_arn,
    zipped_code_dir,    # Specify either zipped_code_dir and s3_bucket, OR script
    s3_bucket,          # S3 bucket where zipped_code_dir is uploaded
    script,             # Path of Lambda function script
    handler,            # Lambda handler specified as "lambda_script.lambda_handler"
    timeout,            # Maximum time the Lambda function can run before the lambda step fails
    ...
)
```

The [sagemaker\.workflow\.lambda\_step\.LambdaStep](https://sagemaker.readthedocs.io/en/stable/workflows/pipelines/sagemaker.workflow.pipelines.html#sagemaker.workflow.lambda_step.LambdaStep) class has a `lambda_func` argument of type `Lambda`\. To invoke an existing Lambda function, the only requirement is to supply the Amazon Resource Name \(ARN\) of the function to `function_arn`\. If you don't supply a value for `function_arn`, you must specify `handler` and one of the following:
+ `zipped_code_dir` – The path of the zipped Lambda function

  `s3_bucket` – Amazon S3 bucket where `zipped_code_dir` is to be uploaded
+ `script` – The path of the Lambda function script file

The following example shows how to create a `Lambda` step definition that invokes an existing Lambda function\.

```
from sagemaker.workflow.lambda_step import LambdaStep
from sagemaker.lambda_helper import Lambda

step_lambda = LambdaStep(
    name="ProcessingLambda",
    lambda_func=Lambda(
        function_arn="arn:aws:lambda:us-west-2:012345678910:function:split-dataset-lambda"
    ),
    inputs={
        s3_bucket = s3_bucket,
        data_file = data_file
    },
    outputs=[
        "train_file", "test_file"
    ]
)
```

The following example shows how to create a `Lambda` step definition that creates and invokes a Lambda function using a Lambda function script\.

```
from sagemaker.workflow.lambda_step import LambdaStep
from sagemaker.lambda_helper import Lambda

step_lambda = LambdaStep(
    name="ProcessingLambda",
    lambda_func=Lambda(
      function_name="split-dataset-lambda",
      execution_role_arn=execution_role_arn,
      script="lambda_script.py",
      handler="lambda_script.lambda_handler",
      ...
    ),
    inputs={
        s3_bucket = s3_bucket,
        data_file = data_file
    },
    outputs=[
        "train_file", "test_file"
    ]
)
```

**Inputs and outputs**

If your `Lambda` function has inputs or outputs, these must also be defined in your `Lambda` step\.

**Note**  
Input and output parameters should not be nested\. For example, if you use a nested dictionary as your output parameter, then the dictionary is treated as a single string \(ex\. `{"output1": "{\"nested_output1\":\"my-output\"}"}`\)\. If you provide a nested value and try to refer to it later, a non\-retryable client error is thrown\.

When defining the `Lambda` step, `inputs` must be a dictionary of key\-value pairs\. Each value of the `inputs` dictionary must be a primitive type \(string, integer, or float\)\. Nested objects are not supported\. If left undefined, the `inputs` value defaults to `None`\.

The `outputs` value must be a list of keys\. These keys refer to a dictionary defined in the output of the `Lambda` function\. Like `inputs`, these keys must be primitive types, and nested objects are not supported\.

**Timeout and stopping behavior**

The `Lambda` class has a `timeout` argument that specifies the maximum time that the Lambda function can run\. The default value is 120 seconds with a maximum value of 10 minutes\. If the Lambda function is running when the timeout is met, the Lambda step fails; however, the Lambda function continues to run\.

A pipeline process can't be stopped while a Lambda step is running because the Lambda function invoked by the Lambda step can't be stopped\. If you attempt to stop the process while the Lambda function is running, the pipeline waits for the Lambda function to finish or until the timeout is hit, whichever occurs first, and then stops\. If the Lambda function finishes, the pipeline process status is `Stopped`\. If the timeout is hit the pipeline process status is `Failed`\.

### ClarifyCheck Step<a name="step-type-clarify-check"></a>

You can use the `ClarifyCheck` step to conduct baseline drift checks against previous baselines for bias analysis and model explainability\. You can then generate and [register your baselines](https://docs.aws.amazon.com/sagemaker/latest/dg/pipelines-quality-clarify-baseline-lifecycle.html#pipelines-quality-clarify-baseline-calculations) with the `model.register()` method and pass the output of that method to [Model Step](#step-type-model) using `[step\_args](https://sagemaker.readthedocs.io/en/stable/amazon_sagemaker_model_building_pipeline.html#model-step)`\. These baselines for drift check can be used by Amazon SageMaker Model Monitor for your model endpoints so that you don’t need to do a [baseline](https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor-create-baseline.html) suggestion separately\. The `ClarifyCheck` step can also pull baselines for drift check from the model registry\. The `ClarifyCheck` step leverages the Amazon SageMaker Clarify prebuilt container that provides a range of model monitoring capabilities, including constraint suggestion and constraint validation against a given baseline\. For more information, see [Getting Started with a SageMaker Clarify Container](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-configure-processing-jobs.html#clarify-processing-job-configure-container)\.

#### Configuring the ClarifyCheck step<a name="configuring-step-type-clarify"></a>

You can configure the `ClarifyCheck` step to conduct only one of the following check types each time it’s used in a pipeline\.
+ Data bias check
+ Model bias check
+ Model explainability check

You do this by setting the `clarify_check_config` parameter with one of the following check type values:
+ `DataBiasCheckConfig`
+ `ModelBiasCheckConfig`
+ `ModelExplainabilityCheckConfig`

The `ClarifyCheck` step launches a processing job that runs the SageMaker Clarify prebuilt container and requires dedicated [configurations for the check and the processing job](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-configure-processing-jobs.html)\. `ClarifyCheckConfig` and `CheckJobConfig` are helper functions for these configurations that are aligned with how the SageMaker Clarify processing job computes for checking model bias, data bias, or model explainability\. For more information, see [Run SageMaker Clarify Processing Jobs for Bias Analysis and Explainability](clarify-processing-job-run.md)\. 

#### Controlling step behaviors for drift check<a name="controlling-step-type-clarify"></a>

The `ClarifyCheck` step requires the following two boolean flags to control its behavior:
+ `skip_check`: This parameter indicates if the drift check against the previous baseline is skipped or not\. If it is set to `False`, the previous baseline of the configured check type must be available\.
+ `register_new_baseline`: This parameter indicates if a newly calculated baseline can be accessed though step property `BaselineUsedForDriftCheckConstraints`\. If it is set to `False`, the previous baseline of the configured check type also must be available\. This can be accessed through the `BaselineUsedForDriftCheckConstraints` property\. 

For more information, see [Baseline calculation, drift detection and lifecycle with ClarifyCheck and QualityCheck steps in Amazon SageMaker Model Building Pipelines](pipelines-quality-clarify-baseline-lifecycle.md)\.

#### Working with baselines<a name="step-type-clarify-working-with-baselines"></a>

You can optionally specify the `model_package_group_name` to locate the existing baseline and the `ClarifyCheck` step pulls the `DriftCheckBaselines` on the latest approved model package in the model package group\. Or, you can provide a previous baseline through the `supplied_baseline_constraints` parameter\. If you specify both the `model_package_group_name` and the `supplied_baseline_constraints`, the `ClarifyCheck` step uses the baseline specified by the `supplied_baseline_constraints` parameter\.

For more information on using the `ClarifyCheck` step requirements, see the [ sagemaker\.workflow\.steps\.ClarifyCheckStep](https://sagemaker.readthedocs.io/en/stable/workflows/pipelines/sagemaker.workflow.pipelines.html#sagemaker.workflow.steps.ClarifyCheckStep) in the *Amazon SageMaker SageMaker SDK for Python*\. For an Amazon SageMaker Studio notebook that shows how to use `ClarifyCheck` step in SageMaker Pipelines, see [sagemaker\-pipeline\-model\-monitor\-clarify\-steps\.ipynb](https://github.com/aws/amazon-sagemaker-examples/tree/master/sagemaker-pipelines/tabular/model-monitor-clarify-pipelines/sagemaker-pipeline-model-monitor-clarify-steps.ipynb)\.

**Example Create a `ClarifyCheck` step for data bias check**  

```
from sagemaker.workflow.check_job_config import CheckJobConfig
from sagemaker.workflow.clarify_check_step import DataBiasCheckConfig, ClarifyCheckStep
from sagemaker.workflow.execution_variables import ExecutionVariables

check_job_config = CheckJobConfig(
    role=role,
    instance_count=1,
    instance_type="ml.c5.xlarge",
    volume_size_in_gb=120,
    sagemaker_session=sagemaker_session,
)

data_bias_data_config = DataConfig(
    s3_data_input_path=step_process.properties.ProcessingOutputConfig.Outputs["train"].S3Output.S3Uri,
    s3_output_path=Join(on='/', values=['s3:/', your_bucket, base_job_prefix, ExecutionVariables.PIPELINE_EXECUTION_ID, 'databiascheckstep']),
    label=0,
    dataset_type="text/csv",
    s3_analysis_config_output_path=data_bias_analysis_cfg_output_path,
)

data_bias_config = BiasConfig(
    label_values_or_threshold=[15.0], facet_name=[8], facet_values_or_threshold=[[0.5]]  
)

data_bias_check_config = DataBiasCheckConfig(
    data_config=data_bias_data_config,
    data_bias_config=data_bias_config,
)h

data_bias_check_step = ClarifyCheckStep(
    name="DataBiasCheckStep",
    clarify_check_config=data_bias_check_config,
    check_job_config=check_job_config,
    skip_check=False,
    register_new_baseline=False
   supplied_baseline_constraints="s3://sagemaker-us-west-2-111122223333/baseline/analysis.json",
    model_package_group_name="MyModelPackageGroup"
)
```

### QualityCheck Step<a name="step-type-quality-check"></a>

You can use the `QualityCheck` step to conduct [baseline suggestions](https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor-create-baseline.html) and drift checks against a previous baseline for data quality or model quality in a pipeline\. You can then generate and [register your baselines](https://docs.aws.amazon.com/sagemaker/latest/dg/pipelines-quality-clarify-baseline-lifecycle.html#pipelines-quality-clarify-baseline-calculations) with the `model.register()` method and pass the output of that method to [Model Step](#step-type-model) using `[step\_args](https://sagemaker.readthedocs.io/en/stable/amazon_sagemaker_model_building_pipeline.html#model-step)`\. Model Monitor can use these baselines for drift check for your model endpoints so that you don’t need to do a baseline suggestion separately\. The `QualityCheck` step can also pull baselines for drift check from the model registry\. The `QualityCheck` step leverages the Amazon SageMaker Model Monitor prebuilt container, which has a range of model monitoring capabilities including constraint suggestion, statistics generation, and constraint validation against a baseline\. For more information, see [Amazon SageMaker Model Monitor prebuilt container](model-monitor-pre-built-container.md)\.

#### Configuring the QualityCheck step<a name="configuring-step-type-quality"></a>

You can configure the `QualityCheck` step to conduct only one of the following check types each time it’s used in a pipeline\.
+ Data quality check
+ Model quality check

You do this by setting the `quality_check_config` parameter with one of the following check type values:
+ `DataQualityCheckConfig`
+ `ModelQualityCheckConfig`

The `QualityCheck` step launches a processing job that runs the Model Monitor prebuilt container and requires dedicated configurations for the check and the processing job\. The `QualityCheckConfig` and `CheckJobConfig` are helper functions for these configurations that are aligned with how Model Monitor creates a baseline for the model quality or data quality monitoring\. For more information on the Model Monitor baseline suggestions, see [Create a Baseline](model-monitor-create-baseline.md) and [Create a Model Quality Baseline](model-monitor-model-quality-baseline.md)\.

#### Controlling step behaviors for drift check<a name="controlling-step-type-quality"></a>

The `QualityCheck` step requires the following two Boolean flags to control its behavior:
+ `skip_check`: This parameter indicates if the drift check against the previous baseline is skipped or not\. If it is set to `False`, the previous baseline of the configured check type must be available\.
+ `register_new_baseline`: This parameter indicates if a newly calculated baseline can be accessed through step properties `BaselineUsedForDriftCheckConstraints` and `BaselineUsedForDriftCheckStatistics`\. If it is set to `False`, the previous baseline of the configured check type must also be available\. These can be accessed through the `BaselineUsedForDriftCheckConstraints` and `BaselineUsedForDriftCheckStatistics` properties\.

For more information, see [Baseline calculation, drift detection and lifecycle with ClarifyCheck and QualityCheck steps in Amazon SageMaker Model Building Pipelines](pipelines-quality-clarify-baseline-lifecycle.md)\.

#### Working with baselines<a name="step-type-quality-working-with-baselines"></a>

You can specify a previous baseline directly through the `supplied_baseline_statistics` and `supplied_baseline_constraints` parameters, or you can simply specify the `model_package_group_name` and the `QualityCheck` step pulls the `DriftCheckBaselines` on the latest approved model package in the model package group\. When you specify the `model_package_group_name`, the `supplied_baseline_constraints`, and `supplied_baseline_statistics`, the `QualityCheck` step uses the baseline specified by `supplied_baseline_constraints` and `supplied_baseline_statistics` on the check type of the `QualityCheck` step you are running\.

For more information on using the `QualityCheck` step requirements, see the [ sagemaker\.workflow\.steps\.QualityCheckStep](https://sagemaker.readthedocs.io/en/stable/workflows/pipelines/sagemaker.workflow.pipelines.html#sagemaker.workflow.steps.QualityCheckStep) in the *Amazon SageMaker SageMaker SDK for Python*\. For an Amazon SageMaker Studio notebook that shows how to use `QualityCheck` step in SageMaker Pipelines, see [sagemaker\-pipeline\-model\-monitor\-clarify\-steps\.ipynb](https://github.com/aws/amazon-sagemaker-examples/tree/master/sagemaker-pipelines/tabular/model-monitor-clarify-pipelines/sagemaker-pipeline-model-monitor-clarify-steps.ipynb)\. 

**Example Create a `QualityCheck` step for data quality check**  

```
from sagemaker.workflow.check_job_config import CheckJobConfig
from sagemaker.workflow.quality_check_step import DataQualityCheckConfig, QualityCheckStep
from sagemaker.workflow.execution_variables import ExecutionVariables

check_job_config = CheckJobConfig(
    role=role,
    instance_count=1,
    instance_type="ml.c5.xlarge",
    volume_size_in_gb=120,
    sagemaker_session=sagemaker_session,
)

data_quality_check_config = DataQualityCheckConfig(
    baseline_dataset=step_process.properties.ProcessingOutputConfig.Outputs["train"].S3Output.S3Uri,
    dataset_format=DatasetFormat.csv(header=False, output_columns_position="START"),
    output_s3_uri=Join(on='/', values=['s3:/', your_bucket, base_job_prefix, ExecutionVariables.PIPELINE_EXECUTION_ID, 'dataqualitycheckstep'])
)

data_quality_check_step = QualityCheckStep(
    name="DataQualityCheckStep",
    skip_check=False,
    register_new_baseline=False,
    quality_check_config=data_quality_check_config,
    check_job_config=check_job_config,
    supplied_baseline_statistics="s3://sagemaker-us-west-2-555555555555/baseline/statistics.json",
    supplied_baseline_constraints="s3://sagemaker-us-west-2-555555555555/baseline/constraints.json",
    model_package_group_name="MyModelPackageGroup"
)
```

### Amazon EMR Step<a name="step-type-emr"></a>

You can use the Amazon SageMaker Model Building Pipelines [Amazon EMR](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-overview.html) step to process [ EMR steps](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-work-with-steps.html) on a running Amazon EMR cluster or have the pipeline create and manage an Amazon EMR cluster for you\. For more information about Amazon EMR, see [Getting started with Amazon EMR](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-gs.html)\.

The Amazon EMR step requires that `EMRStepConfig` include the Amazon S3 location of the JAR file to be used by the Amazon EMR cluster and any arguments to be passed\. You also provide the Amazon EMR cluster ID if you want to run the step on a running EMR cluster, or the cluster configuration if you want the EMR step to run on a cluster that it creates, manages, and terminates for you\. The following sections include examples demonstrating both methods\.

**Note**  
Amazon EMR steps require that the role passed to your pipeline has additional permissions\. You should attach the [AWS managed policy: `AmazonSageMakerPipelinesIntegrations`](https://docs.aws.amazon.com/sagemaker/latest/dg/security-iam-awsmanpol-pipelines.html#security-iam-awsmanpol-AmazonSageMakerPipelinesIntegrations) to your pipeline role, or ensure that the role includes the permissions in that policy\. If you are launching a new job on a new Amazon EMR cluster, you need to add additional permissions\. See the following discussion for details\.
Amazon EMR on EKS is not supported\.
If you process an EMR step on a running cluster, you can only use a cluster that is in one of the following states: `STARTING`, `BOOTSTRAPPING`, `RUNNING`, or `WAITING`\.
If you process EMR steps on a running cluster, you can have at most 256 EMR steps in a `PENDING` state on an EMR cluster\. EMR steps submitted beyond this limit result in pipeline execution failure\. You may consider using [Retry Policy for Pipeline Steps](pipelines-retry-policy.md)\.
You can specify either cluster ID or cluster configuration, but not both\.
The Amazon EMR step relies on Amazon EventBridge to monitor changes in the EMR step or cluster state\. If you process your Amazon EMR job on a running cluster, the EMR step uses the `SageMakerPipelineExecutionEMRStepStatusUpdateRule` rule to monitor EMR step state\. If you process your job on a cluster that the EMR step creates for you, the step uses the `SageMakerPipelineExecutionEMRClusterStatusRule` rule to monitor changes in cluster state\. If you see either of these EventBridge rules in your AWS account, do not delete them or else your EMR step may not complete\.

**Launch a new job on a running Amazon EMR cluster**

If you want to launch a new job on a running Amazon EMR cluster, you pass the cluster ID as a string to the `cluster_id` argument of `EMRStep`\. The following example demonstrates this procedure\.

```
from sagemaker.workflow.emr_step import EMRStep, EMRStepConfig

emr_config = EMRStepConfig(
    jar="s3://path/to/jar/MyJar.jar", # required, S3 path to jar
    args=["--verbose", "--force"], # optional list of arguments to pass to the jar
    main_class="com.my.Main1", # optional main class, this can be omitted if jar above has a manifest 
    properties=[ # optional list of Java properties that are set when the step runs
    {
        "key": "mapred.tasktracker.map.tasks.maximum",
        "value": "2"
    },
    {
        "key": "mapreduce.map.sort.spill.percent",
        "value": "0.90"
   },
   {
       "key": "mapreduce.tasktracker.reduce.tasks.maximum",
       "value": "5"
    }
  ]
)

step_emr = EMRStep (
    name="EMRSampleStep", # required
    cluster_id="j-1ABCDEFG2HIJK", # include cluster_id to use a running cluster
    step_config=emr_config, # required
    display_name="My EMR Step",
    description="Pipeline step to execute EMR job"
)
```

**Launch a new job on a new Amazon EMR cluster**

If you want to launch a new job on a new cluster that `EMRStep` creates for you, you need to add additional permissions\. Grant these permissions, as shown in the following policy, in your SageMaker pipeline execution IAM role:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "iam:PassRole"
            ],
            "Resource": "arn:aws:iam::*:role/*",
            "Condition": {
                "StringEquals": {
                    "iam:PassedToService": [
                        "elasticmapreduce.amazonaws.com",
                        "ec2.amazonaws.com"
                    ]
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "events:DescribeRule",
                "events:PutRule",
                "events:PutTargets"
            ],
            "Resource": [
                "arn:aws:events:*:*:rule/SageMakerPipelineExecutionEMRClusterStatusUpdateRule"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "elasticmapreduce:RunJobFlow",
                "elasticmapreduce:DescribeCluster",
                "elasticmapreduce:TerminateJobFlows",
                "elasticmapreduce:ListSteps"
            ],
            "Resource": [
                "arn:aws:elasticmapreduce:*:*:cluster/*"
            ]
        }
    ]
}
```

To create your `EMRStep` pipeline step, provide your cluster configuration as a dictionary with the same structure as a [RunJobFlow](https://docs.aws.amazon.com/emr/latest/APIReference/API_RunJobFlow.html) request\. However, do not include the following fields in your cluster configuration:
+ \[`Name`\]
+ \[`Steps`\]
+ \[`AutoTerminationPolicy`\]
+ \[`Instances`\]\[`KeepJobFlowAliveWhenNoSteps`\]
+ \[`Instances`\]\[`TerminationProtected`\]

All other `RunJobFlow` arguments are available for use in your cluster configuration\. For details about the request syntax, see [RunJobFlow](https://docs.aws.amazon.com/emr/latest/APIReference/API_RunJobFlow.html)\.

The following example passes a cluster configuration to an Amazon EMR step definition, which prompts the step to launch a new job on a new EMR cluster\.

```
from sagemaker.workflow.emr_step import EMRStep, EMRStepConfig

emr_step_config = EMRStepConfig(
    jar="s3://path/to/jar/MyJar.jar", # required, S3 path to jar
    args=["--verbose", "--force"], # optional list of arguments to pass to the jar
    main_class="com.my.Main1", # optional main class, this can be omitted if jar above has a manifest 
    properties=[ # optional list of Java properties that are set when the step runs
    {
        "key": "mapred.tasktracker.map.tasks.maximum",
        "value": "2"
    },
    {
        "key": "mapreduce.map.sort.spill.percent",
        "value": "0.90"
   },
   {
       "key": "mapreduce.tasktracker.reduce.tasks.maximum",
       "value": "5"
    }
  ]
)

# include your cluster configuration as a dictionary
emr_cluster_config = {
    "Instances":{
        "InstanceGroups":[
            {
                "InstanceRole": "MASTER",
                "InstanceCount": 1,
                "InstanceType": "m4.large"
            }
        ]
    },
    "BootstrapActions": [],
    "ReleaseLabel": "emr-5.14.0",
    "JobFlowRole": "job-flow-role",
    "ServiceRole": "service-role"
}

emr_step = EMRStep(
    name="emr-step",
    cluster_id=None,
    display_name="emr_step",
    description="MyEMRStepDescription",
    step_config=emr_step_config,
    cluster_config=emr_cluster_config
)
```

### Fail Step<a name="step-type-fail"></a>

You use a `FailStep` to stop an Amazon SageMaker Model Building Pipelines execution when a desired condition or state is not achieved and to mark that pipeline's execution as failed\. The `FailStep` also allows you to enter a custom error message, indicating the cause of the pipeline's execution failure\. 

**Note**  
When a `FailStep` and other pipeline steps execute concurrently, the pipeline does not terminate until all concurrent steps are completed\.

#### Limitations for using `FailStep`<a name="step-type-fail-limitations"></a>
+ You cannot add a `FailStep` to the `DependsOn` list of other steps\. For more information, see [Custom Dependency Between Steps](#build-and-manage-custom-dependency)\.
+ Other steps cannot reference the `FailStep`\. It is *always* the last step in a pipeline's execution\.
+ You cannot retry a pipeline execution ending with a `FailStep`\.

You can create the `FailStep` `ErrorMessage` in the form of a static text string\. Alternatively, you can also use [Pipeline Parameters](https://docs.aws.amazon.com/sagemaker/latest/dg/build-and-manage-parameters.html) a [ Join](https://sagemaker.readthedocs.io/en/stable/workflows/pipelines/sagemaker.workflow.pipelines.html?highlight=Join#sagemaker.workflow.functions.Join) operation, or other [step properties](https://docs.aws.amazon.com/sagemaker/latest/dg/build-and-manage-steps.html#build-and-manage-properties) to create a more informative error message\.

**Example**  
The following example code snippet uses a `FailStep` with an `ErrorMessage` configured with Pipeline Parameters and a `Join` operation\.  

```
from sagemaker.workflow.fail_step import FailStep
from sagemaker.workflow.functions import Join
from sagemaker.workflow.parameters import ParameterInteger

mse_threshold_param = ParameterInteger(name="MseThreshold", default_value=5)
step_fail = FailStep(
    name="AbaloneMSEFail",
    error_message=Join(
        on=" ", values=["Execution failed due to MSE >", mse_threshold_param]
    ),
)
```

## Step Properties<a name="build-and-manage-properties"></a>

The `properties` attribute is used to add data dependencies between steps in the pipeline\. These data dependencies are then used by SageMaker Pipelines to construct the DAG from the pipeline definition\. These properties can be referenced as placeholder values and are resolved at runtime\. 

The `properties` attribute of a SageMaker Pipelines step matches the object returned by a `Describe` call for the corresponding SageMaker job type\. For each job type, the `Describe` call returns the following response object:
+ `ProcessingStep` – [DescribeProcessingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeProcessingJob.html)
+ `TrainingStep` – [DescribeTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeTrainingJob.html)
+ `TransformStep` – [DescribeTransformJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeTransformJob.html)

To check which properties are referrable for each step type during data dependency creation, see *[Data Dependency \- Property Reference](https://sagemaker.readthedocs.io/en/stable/amazon_sagemaker_model_building_pipeline.html#data-dependency-property-reference)* in the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\.

## Step Parallelism<a name="build-and-manage-parallelism"></a>

When a step does not depend on any other step, it is run immediately upon pipeline execution\. However, executing too many pipeline steps in parallel can quickly exhaust available resources\. Control the number of concurrent steps for a pipeline execution with `ParallelismConfiguration`\.

The following example uses `ParallelismConfiguration` to set the concurrent step limit to five\.

```
pipeline.create(
    parallelism_config=ParallelismConfiguration(5),
)
```

## Data Dependency Between Steps<a name="build-and-manage-data-dependency"></a>

You define the structure of your DAG by specifying the data relationships between steps\. To create data dependencies between steps, pass the properties of one step as the input to another step in the pipeline\. The step receiving the input isn't started until after the step providing the input finishes running\.

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
from sagemaker.workflow.pipeline_context import PipelineSession

sklearn_train = SKLearn(..., sagemaker_session=PipelineSession())

step_train = TrainingStep(
    name="CensusTrain",
    step_args=sklearn_train.fit(inputs=TrainingInput(
        s3_data=step_process.properties.ProcessingOutputConfig.Outputs[
            "train_data"].S3Output.S3Uri
    ))
)
```

To check which properties are referrable for each step type during data dependency creation, see *[Data Dependency \- Property Reference](https://sagemaker.readthedocs.io/en/stable/amazon_sagemaker_model_building_pipeline.html#data-dependency-property-reference)* in the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\.

## Custom Dependency Between Steps<a name="build-and-manage-custom-dependency"></a>

When you specify a data dependency, SageMaker Pipelines provides the data connection between the steps\. Alternatively, one step can access the data from a previous step without directly using SageMaker Pipelines\. In this case, you can create a custom dependency that tells SageMaker Pipelines not to start a step until after another step has finished running\. You create a custom dependency by specifying a step's `DependsOn` attribute\.

As an example, the following defines a step `C` that starts only after both step `A` and step `B` finish running\.

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

The following example creates a training step that starts after a processing step finishes running\.

```
processing_step = ProcessingStep(...)
training_step = TrainingStep(...)

training_step.add_depends_on([processing_step])
```

The following example creates a training step that doesn't start until two different processing steps finish running\.

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

The following example creates a training step that receives input from one processing step and waits for a different processing step to finish running\.

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
custom_dependencies = training_step.depends_on
```

## Use a Custom Image in a Step<a name="build-and-manage-images"></a>

 You can use any of the available SageMaker [Deep Learning Container images](https://github.com/aws/deep-learning-containers/blob/master/available_images.md) when you create a step in your pipeline\. 

You can also use your own container with pipeline steps\. Because you can’t create an image from within Amazon SageMaker Studio, you must create your image using another method before using it with Amazon SageMaker Model Building Pipelines\.

To use your own container when creating the steps for your pipeline, include the image URI in the estimator definition\. For more information on using your own container with SageMaker, see [Using Docker Containers with SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/docker-containers.html)\.