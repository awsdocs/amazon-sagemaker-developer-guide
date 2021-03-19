# Pipeline Steps<a name="build-and-manage-steps"></a>

SageMaker Pipelines are composed of steps\. These steps define the actions that the pipeline takes, and the relationships between steps using properties\.

 Amazon SageMaker Model Building Pipelines support the following step types: 
+ Processing
+ Training
+ Condition
+ BatchTransform
+ RegisterModel
+ CreateModel

The following describes the requirements of each step and provides an example implementation of the step\. These are not functional implementations because they don't provide the resource and inputs needed\. For a tutorial that implements these steps, see [Create and Manage SageMaker Pipelines](pipelines-build.md)\.

**Processing**

You use a processing step to create a processing job for data processing\. For more information on processing jobs, see [Process Data and Evaluate Models](https://docs.aws.amazon.com/sagemaker/latest/dg/processing-job.html)\.

A processing step requires a processor, a Python script that defines the processing code, outputs for processing, and job arguments\. The following example shows how to create a `ProcessingStep` definition\.  For more information on processing step requirements, see the [sagemaker\.workflow\.steps\.ProcessingStep](https://sagemaker.readthedocs.io/en/stable/workflows/pipelines/sagemaker.workflow.pipelines.html#sagemaker.workflow.steps.ProcessingStep) documentation\.

```
from sagemaker.processing import ProcessingInput, ProcessingOutput
from sagemaker.workflow.steps import ProcessingStep

step_process = ProcessingStep(
    name="AbaloneProcess",
    processor=sklearn_processor,
    inputs=[
      ProcessingInput(source=input_data, destination="/opt/ml/processing/input"),  
    ],
    outputs=[
        ProcessingOutput(output_name="train", source="/opt/ml/processing/train"),
        ProcessingOutput(output_name="validation", source="/opt/ml/processing/validation"),
        ProcessingOutput(output_name="test", source="/opt/ml/processing/test")
    ],
    code="abalone/preprocessing.py"
)
```

**Training**

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

**Condition**

You use a condition step to evaluate the condition of step properties to assess which action should be taken next in the pipeline\.

A condition step requires a list of conditions, and a list of steps to execute if the condition evaluates to `true` and a list of steps to execute if the condition evaluates to `false`\. The following example shows how to create a `Condition` step definition\.  For more information on `Condition` step requirements, see the [sagemaker\.workflow\.condition\_step\.ConditionStep](https://sagemaker.readthedocs.io/en/stable/workflows/pipelines/sagemaker.workflow.pipelines.html#conditionstep) documentation\.  

**Note**  
SageMaker Pipelines doesn't support the use of nested condition steps\. You can't pass a condition step as the input for another condition step\. 

```
from sagemaker.workflow.conditions import ConditionLessThanOrEqualTo
from sagemaker.workflow.condition_step import (
    ConditionStep,
    JsonGet
)

step_cond = ConditionStep(
    name="AbaloneMSECond",
    conditions=[cond_lte],
    if_steps=[step_register, step_create_model, step_transform],
    else_steps=[]
)
```

**Transform**

You use a transform step for batch transformation to run inference on an entire dataset\. For more information on batch transformation, see [Run Batch Transforms with Inference Pipelines ](https://docs.aws.amazon.com/sagemaker/latest/dg/inference-pipeline-batch.html)\.

A transform step requires a transformer, and the data to run batch transformation on\. The following example shows how to create a `Transform` step definition\.  For more information on `Transform` step requirements, see the [sagemaker\.workflow\.steps\.TransformStep](https://sagemaker.readthedocs.io/en/stable/workflows/pipelines/sagemaker.workflow.pipelines.html#sagemaker.workflow.steps.TransformStep)\.  

```
from sagemaker.inputs import TransformInput
from sagemaker.workflow.steps import TransformStep

step_transform = TransformStep(
    name="AbaloneTransform",
    transformer=transformer,
    inputs=TransformInput(data=batch_data)
)
```

**RegisterModel**

You use a RegisterModel step to register a model to a model group\. For more information on registering models, see [Register and Deploy Models with Model Registry](model-registry.md)\.

A RegisterModel step requires an estimator, model data output from training, and a model package group name to associate the model package with\. The following example shows how to create a `RegisterModel` definition\.  For more information on RegisterModel step requirements, see the [sagemaker\.workflow\.step\_collections\.RegisterModel](https://sagemaker.readthedocs.io/en/stable/workflows/pipelines/sagemaker.workflow.pipelines.html#sagemaker.workflow.step_collections.RegisterModel)\.  

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

**CreateModel**

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

 For more information on the available steps and their input requirements, see the [Pipelines](https://sagemaker.readthedocs.io/en/stable/workflows/pipelines/sagemaker.workflow.pipelines.html) documentation\. 

## Step Properties<a name="build-and-manage-properties"></a>

The `properties` attribute is used to add data dependencies between steps in the pipeline\. These data dependencies are then used by SageMaker Pipelines to construct the DAG from the pipeline definition\. These properties can be referenced as placeholder values and are resolved at runtime\.  

The `properties` attribute of a SageMaker Pipelines step matches the object returned by a `Describe` call for the corresponding SageMaker job type\. For each job type, the `Describe` call returns the following response object:
+ `ProcessingStep` – [DescribeProcessingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeProcessingJob.html)
+ `TrainingStep` – [DescribeTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeTrainingJob.html)
+ `TransformStep` – [DescribeTransformJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeTransformJob.html)

## Data Dependency Between Steps<a name="build-and-manage-data-dependency"></a>

You define the structure of your DAG by specifying the data relationships between steps\. To create data dependencies between steps, pass the properties of one step as the input to another step in the pipeline\. A data dependency uses JsonPath notation in the following format\. This format traverses the JSON property file, which means you can append as many *<property>* instances as needed to reach the desired nested property in the file\. For more information on JsonPath notation, see the [JsonPath repo](https://github.com/json-path/JsonPath)\.

```
<step_name>.properties.<property>.<property>
```

The following is an example of a data dependency using the `ProcessingOutputConfig` property of a processing step:

```
step_processing.properties.ProcessingOutputConfig.Outputs["train_data"].S3Output.S3Uri
```

The data dependency can be passed as the input to a step as follows:

```
step_train = TrainingStep(
    name="CensusTrain",
    estimator=sklearn_train,
    inputs=TrainingInput(
        s3_data=step_process.properties.ProcessingOutputConfig.Outputs[
            "train_data"
        ].S3Output.S3Uri
    ),
)
```

## Using Custom Images in Pipelines<a name="build-and-manage-images"></a>

 You can use any of the available SageMaker [Deep Learning Container images](https://github.com/aws/deep-learning-containers/blob/master/available_images.md) when you create a step in your pipeline\. 

You can also create a step using SageMaker S3 applications\. A SageMaker S3 application is a tar\.gz bundle with one or more Python scripts that can run within that bundle\. For more information on application package bundling, see [Deploying directly from model artifacts](https://sagemaker.readthedocs.io/en/stable/frameworks/tensorflow/using_tf.html?highlight=packaging#id22)\. 

 You can also use your own container with pipeline steps\. Because you can’t create an image from within SageMaker Studio, you must create your image using another method before using it with Amazon SageMaker Model Building Pipelines\.

 To use your own container when creating the steps for your pipeline, include the image URI in the estimator definition\. For more information on using your own container with SageMaker, see [Using Docker Containers with SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/docker-containers.html)\. 