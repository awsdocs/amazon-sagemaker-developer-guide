# Define a Pipeline<a name="define-pipeline"></a>

 To orchestrate your workflows with Amazon SageMaker Model Building Pipelines, you need to generate a directed acyclic graph \(DAG\) in the form of a JSON pipeline definition\. The following image is a representation of the pipeline DAG that you create in this tutorial:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pipeline-full.png)

 You can generate your JSON pipeline definition using the SageMaker Python SDK\. The following tutorial shows how to generate a pipeline definition for a pipeline that solves a regression problem to determine the age of an abalone based on its physical measurements\. For a Jupyter notebook that includes the content in this tutorial that you can run, see [Orchestrating Jobs with Amazon SageMaker Model Building Pipelines](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-pipelines/tabular/abalone_build_train_deploy/sagemaker-pipelines-preprocess-train-evaluate-batch-transform.html)\.

**Topics**
+ [Prerequisites](#define-pipeline-prereq)
+ [Create a Pipeline](#define-pipeline-create)

## Prerequisites<a name="define-pipeline-prereq"></a>

 To run the following tutorial you must do the following: 
+ Set up your notebook instance as outlined in [Create a notebook instance](https://docs.aws.amazon.com/sagemaker/latest/dg/howitworks-create-ws.html)\. This gives your role permissions to read and write to Amazon S3, and create training, batch transform, and processing jobs in SageMaker\. 
+ Grant your notebook permissions to get and pass its own role as shown in [Modifying a role permissions policy](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-managingrole-editing-console.html#roles-modify_permissions-policy)\. Add the following JSON snippet to attach this policy to your role\. Replace `<your-role-arn>` with the ARN used to create your notebook instance\. 

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Effect": "Allow",
              "Action": [
                  "iam:GetRole",
                  "iam:PassRole"
              ],
              "Resource": "<your-role-arn>"
          }
      ]
  }
  ```
+  Trust the SageMaker service principal by following the steps in [Modifying a role trust policy](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-managingrole-editing-cli.html#roles-managingrole_edit-trust-policy-cli)\. Add the following statement fragment to the trust relationship of your role: 

  ```
  {
        "Sid": "",
        "Effect": "Allow",
        "Principal": {
          "Service": "sagemaker.amazonaws.com"
        },
        "Action": "sts:AssumeRole"
      }
  ```

### Set Up Your Environment<a name="define-pipeline-prereq-setup"></a>

Create a new SageMaker session using the following code block\. This returns the role ARN for the session\. This role ARN should be the execution role ARN that you set up as a prerequisite\. 

```
import boto3
import sagemaker
import sagemaker.session


region = boto3.Session().region_name
sagemaker_session = sagemaker.session.Session()
role = sagemaker.get_execution_role()
default_bucket = sagemaker_session.default_bucket()
model_package_group_name = f"AbaloneModelPackageGroupName"
```

## Create a Pipeline<a name="define-pipeline-create"></a>

Run the following steps from your SageMaker notebook instance to create a pipeline including steps for preprocessing, training, evaluation, conditional evaluation, and model registration\. 

### Step 1: Download the Dataset<a name="define-pipeline-data-download"></a>

This notebook uses the UCI Machine Learning Abalone Dataset\. The dataset contains the following features: 
+ `length` – The longest shell measurement of the abalone\.
+ `diameter` – The diameter of the abalone perpendicular to its length\.
+ `height` – The height of the abalone with meat in the shell\.
+ `whole_weight` – The weight of the whole abalone\.
+ `shucked_weight` – The weight of the meat removed from the abalone\.
+ `viscera_weight` – The weight of the abalone viscera after bleeding\.
+ `shell_weight` – The weight of the abalone shell after meat removal and drying\.
+ `sex` – The sex of the abalone\. One of 'M', 'F', or 'I', where 'I' is an infant abalone\.
+ `rings` – The number of rings in the abalone shell\.

The number of rings in the abalone shell is a good approximation for its age using the formula `age=rings + 1.5`\. However, obtaining this number is a time\-consuming task\. You must cut the shell through the cone, stain the section, and count the number of rings through a microscope\. However, the other physical measurements are easier to determine\. This notebook uses the dataset to build a predictive model of the variable rings using the other physical measurements\.

**To download the dataset**

1. Download the dataset into your account's default Amazon S3 bucket\.

   ```
   !mkdir -p data
   local_path = "data/abalone-dataset.csv"
   
   s3 = boto3.resource("s3")
   s3.Bucket(f"sagemaker-servicecatalog-seedcode-{region}").download_file(
       "dataset/abalone-dataset.csv",
       local_path
   )
   
   base_uri = f"s3://{default_bucket}/abalone"
   input_data_uri = sagemaker.s3.S3Uploader.upload(
       local_path=local_path, 
       desired_s3_uri=base_uri,
   )
   print(input_data_uri)
   ```

1. Download a second dataset for batch transformation after your model is created\.

   ```
   local_path = "data/abalone-dataset-batch"
   
   s3 = boto3.resource("s3")
   s3.Bucket(f"sagemaker-servicecatalog-seedcode-{region}").download_file(
       "dataset/abalone-dataset-batch",
       local_path
   )
   
   base_uri = f"s3://{default_bucket}/abalone"
   batch_data_uri = sagemaker.s3.S3Uploader.upload(
       local_path=local_path, 
       desired_s3_uri=base_uri,
   )
   print(batch_data_uri)
   ```

### Step 2: Define Pipeline Parameters<a name="define-pipeline-parameters"></a>

 This code block defines the following parameters for your pipeline: 
+  `processing_instance_count` – The instance count of the processing job\. 
+  `input_data` – The Amazon S3 location of the input data\. 
+  `batch_data` – The Amazon S3 location of the input data for batch transformation\. 
+  `model_approval_status` – The approval status to register the trained model with for CI/CD\. For more information, see [Automate MLOps with SageMaker Projects](sagemaker-projects.md)\.

```
from sagemaker.workflow.parameters import (
    ParameterInteger,
    ParameterString,
)

processing_instance_count = ParameterInteger(
    name="ProcessingInstanceCount",
    default_value=1
)
model_approval_status = ParameterString(
    name="ModelApprovalStatus",
    default_value="PendingManualApproval"
)
input_data = ParameterString(
    name="InputData",
    default_value=input_data_uri,
)
batch_data = ParameterString(
    name="BatchData",
    default_value=batch_data_uri,
)
```

### Step 3: Define a Processing Step for Feature Engineering<a name="define-pipeline-processing"></a>

This section shows how to create a processing step to prepare the data from the dataset for training\.

**To create a processing step**

1.  Create a directory for the processing script\.

   ```
   !mkdir -p abalone
   ```

1. Create a file in the `/abalone` directory named `preprocessing.py` with the following content\. This preprocessing script is passed in to the processing step for execution on the input data\. The training step then uses the preprocessed training features and labels to train a model, and the evaluation step uses the trained model and preprocessed test features and labels to evaluate the model\. The script uses `scikit-learn` to do the following:
   +  Fill in missing `sex` categorical data and encode it so it's suitable for training\. 
   +  Scale and normalize all numerical fields except for `rings` and `sex`\. 
   +  Split the data into training, test, and validation datasets\. 

   ```
   %%writefile abalone/preprocessing.py
   import argparse
   import os
   import requests
   import tempfile
   import numpy as np
   import pandas as pd
   
   
   from sklearn.compose import ColumnTransformer
   from sklearn.impute import SimpleImputer
   from sklearn.pipeline import Pipeline
   from sklearn.preprocessing import StandardScaler, OneHotEncoder
   
   
   # Because this is a headerless CSV file, specify the column names here.
   feature_columns_names = [
       "sex",
       "length",
       "diameter",
       "height",
       "whole_weight",
       "shucked_weight",
       "viscera_weight",
       "shell_weight",
   ]
   label_column = "rings"
   
   feature_columns_dtype = {
       "sex": str,
       "length": np.float64,
       "diameter": np.float64,
       "height": np.float64,
       "whole_weight": np.float64,
       "shucked_weight": np.float64,
       "viscera_weight": np.float64,
       "shell_weight": np.float64
   }
   label_column_dtype = {"rings": np.float64}
   
   
   def merge_two_dicts(x, y):
       z = x.copy()
       z.update(y)
       return z
   
   
   if __name__ == "__main__":
       base_dir = "/opt/ml/processing"
   
       df = pd.read_csv(
           f"{base_dir}/input/abalone-dataset.csv",
           header=None, 
           names=feature_columns_names + [label_column],
           dtype=merge_two_dicts(feature_columns_dtype, label_column_dtype)
       )
       numeric_features = list(feature_columns_names)
       numeric_features.remove("sex")
       numeric_transformer = Pipeline(
           steps=[
               ("imputer", SimpleImputer(strategy="median")),
               ("scaler", StandardScaler())
           ]
       )
   
       categorical_features = ["sex"]
       categorical_transformer = Pipeline(
           steps=[
               ("imputer", SimpleImputer(strategy="constant", fill_value="missing")),
               ("onehot", OneHotEncoder(handle_unknown="ignore"))
           ]
       )
   
       preprocess = ColumnTransformer(
           transformers=[
               ("num", numeric_transformer, numeric_features),
               ("cat", categorical_transformer, categorical_features)
           ]
       )
       
       y = df.pop("rings")
       X_pre = preprocess.fit_transform(df)
       y_pre = y.to_numpy().reshape(len(y), 1)
       
       X = np.concatenate((y_pre, X_pre), axis=1)
       
       np.random.shuffle(X)
       train, validation, test = np.split(X, [int(.7*len(X)), int(.85*len(X))])
   
       
       pd.DataFrame(train).to_csv(f"{base_dir}/train/train.csv", header=False, index=False)
       pd.DataFrame(validation).to_csv(f"{base_dir}/validation/validation.csv", header=False, index=False)
       pd.DataFrame(test).to_csv(f"{base_dir}/test/test.csv", header=False, index=False)
   ```

1.  Create an instance of an `SKLearnProcessor` to pass in to the processing step\. 

   ```
   from sagemaker.sklearn.processing import SKLearnProcessor
   
   
   framework_version = "0.23-1"
   
   sklearn_processor = SKLearnProcessor(
       framework_version=framework_version,
       instance_type="ml.m5.xlarge",
       instance_count=processing_instance_count,
       base_job_name="sklearn-abalone-process",
       role=role,
   )
   ```

1. Create a processing step\. This step takes in the `SKLearnProcessor`, the input and output channels, and the `preprocessing.py` script that you created\. This is very similar to a processor instance's `run` method in the SageMaker Python SDK\. The `input_data` parameter passed into `ProcessingStep` is the input data of the step itself\. This input data is used by the processor instance when it runs\. 

    Note the  `"train`, `"validation`, and `"test"` named channels specified in the output configuration for the processing job\. Step `Properties` such as these can be used in subsequent steps and resolve to their runtime values at execution\. 

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
       code="abalone/preprocessing.py",
   )
   ```

### Step 4: Define a Training step<a name="define-pipeline-training"></a>

This section shows how to use the SageMaker [XGBoost Algorithm](https://docs.aws.amazon.com/sagemaker/latest/dg/xgboost.html) to train a model on the training data output from the processing steps\. 

**To define a training step**

1.  Specify the model path where you want to save the models from training\. 

   ```
   model_path = f"s3://{default_bucket}/AbaloneTrain"
   ```

1. Configure an estimator for the XGBoost algorithm and the input dataset\. The training instance type is passed into the estimator\. A typical training script loads data from the input channels, configures training with hyperparameters, trains a model, and saves a model to `model_dir` so that it can be hosted later\. SageMaker uploads the model to Amazon S3 in the form of a `model.tar.gz` at the end of the training job\. 

   ```
   from sagemaker.estimator import Estimator
   
   
   image_uri = sagemaker.image_uris.retrieve(
       framework="xgboost",
       region=region,
       version="1.0-1",
       py_version="py3",
       instance_type="ml.m5.xlarge"
   )
   xgb_train = Estimator(
       image_uri=image_uri,
       instance_type="ml.m5.xlarge",
       instance_count=1,
       output_path=model_path,
       role=role,
   )
   xgb_train.set_hyperparameters(
       objective="reg:linear",
       num_round=50,
       max_depth=5,
       eta=0.2,
       gamma=4,
       min_child_weight=6,
       subsample=0.7,
       silent=0
   )
   ```

1. Create a `TrainingStep` using the estimator instance and properties of the `ProcessingStep`\. In particular, pass in the `S3Uri` of the `"train"` and `"validation"` output channel to the `TrainingStep`\.  

   ```
   from sagemaker.inputs import TrainingInput
   from sagemaker.workflow.steps import TrainingStep
   
   
   step_train = TrainingStep(
       name="AbaloneTrain",
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
       },
   )
   ```

### Step 5: Define a Processing Step for Model Evaluation<a name="define-pipeline-processing"></a>

This section shows how to create a processing step to evaluate the accuracy of the model\. The result of this model evaluation is used in the condition step to determine which execute path to take\.

**To define a processing step for model evaluation**

1. Create a file in the `/abalone` directory named `evaluation.py`\. This script is used in a processing step to perform model evaluation\. It takes a trained model and the test dataset as input, then produces a JSON file containing classification evaluation metrics\.

   ```
   %%writefile abalone/evaluation.py
   import json
   import pathlib
   import pickle
   import tarfile
   import joblib
   import numpy as np
   import pandas as pd
   import xgboost
   
   
   from sklearn.metrics import mean_squared_error
   
   
   if __name__ == "__main__":
       model_path = f"/opt/ml/processing/model/model.tar.gz"
       with tarfile.open(model_path) as tar:
           tar.extractall(path=".")
       
       model = pickle.load(open("xgboost-model", "rb"))
   
       test_path = "/opt/ml/processing/test/test.csv"
       df = pd.read_csv(test_path, header=None)
       
       y_test = df.iloc[:, 0].to_numpy()
       df.drop(df.columns[0], axis=1, inplace=True)
       
       X_test = xgboost.DMatrix(df.values)
       
       predictions = model.predict(X_test)
   
       mse = mean_squared_error(y_test, predictions)
       std = np.std(y_test - predictions)
       report_dict = {
           "regression_metrics": {
               "mse": {
                   "value": mse,
                   "standard_deviation": std
               },
           },
       }
   
       output_dir = "/opt/ml/processing/evaluation"
       pathlib.Path(output_dir).mkdir(parents=True, exist_ok=True)
       
       evaluation_path = f"{output_dir}/evaluation.json"
       with open(evaluation_path, "w") as f:
           f.write(json.dumps(report_dict))
   ```

1.  Create an instance of a `ScriptProcessor` that is used to create a `ProcessingStep`\. 

   ```
   from sagemaker.processing import ScriptProcessor
   
   
   script_eval = ScriptProcessor(
       image_uri=image_uri,
       command=["python3"],
       instance_type="ml.m5.xlarge",
       instance_count=1,
       base_job_name="script-abalone-eval",
       role=role,
   )
   ```

1.  Create a `ProcessingStep` using the processor instance, the input and output channels, and the  `evaluation.py` script\. In particular, pass in the `S3ModelArtifacts` property from the `step_train` training step, as well as the `S3Uri` of the `"test"` output channel of the `step_process` processing step\. This is very similar to a processor instance's `run` method in the SageMaker Python SDK\.  

   ```
   from sagemaker.workflow.properties import PropertyFile
   
   
   evaluation_report = PropertyFile(
       name="EvaluationReport",
       output_name="evaluation",
       path="evaluation.json"
   )
   step_eval = ProcessingStep(
       name="AbaloneEval",
       processor=script_eval,
       inputs=[
           ProcessingInput(
               source=step_train.properties.ModelArtifacts.S3ModelArtifacts,
               destination="/opt/ml/processing/model"
           ),
           ProcessingInput(
               source=step_process.properties.ProcessingOutputConfig.Outputs[
                   "test"
               ].S3Output.S3Uri,
               destination="/opt/ml/processing/test"
           )
       ],
       outputs=[
           ProcessingOutput(output_name="evaluation", source="/opt/ml/processing/evaluation"),
       ],
       code="abalone/evaluation.py",
       property_files=[evaluation_report],
   )
   ```

### Step 6: Define a CreateModelStep for Batch Transformation<a name="define-pipeline-create-model"></a>

**Important**  
We recommend using [Model Step](build-and-manage-steps.md#step-type-model) to create models as of v2\.90\.0 of the SageMaker Python SDK\. `CreateModelStep` will continue to work in previous versions of the SageMaker Python SDK, but is no longer actively supported\.

This section shows how to create a SageMaker model from the output of the training step\. This model is used for batch transformation on a new dataset\. This step is passed into the condition step and only executes if the condition step evaluates to `true`\.

**To define a CreateModelStep for batch transformation**

1.  Create a SageMaker model\. Pass in the `S3ModelArtifacts` property from the `step_train` training step\.

   ```
   from sagemaker.model import Model
   
   
   model = Model(
       image_uri=image_uri,
       model_data=step_train.properties.ModelArtifacts.S3ModelArtifacts,
       sagemaker_session=sagemaker_session,
       role=role,
   )
   ```

1. Define the model input for your SageMaker model\.

   ```
   from sagemaker.inputs import CreateModelInput
   
   
   inputs = CreateModelInput(
       instance_type="ml.m5.large",
       accelerator_type="ml.eia1.medium",
   )
   ```

1. Create your `CreateModelStep` using the `CreateModelInput` and SageMaker model instance you defined\.

   ```
   from sagemaker.workflow.steps import CreateModelStep
   
   
   step_create_model = CreateModelStep(
       name="AbaloneCreateModel",
       model=model,
       inputs=inputs,
   )
   ```

### Step 7: Define a TransformStep to Perform Batch Transformation<a name="define-pipeline-transform"></a>

This section shows how to create a `TransformStep` to perform batch transformation on a dataset after the model is trained\. This step is passed into the condition step and only executes if the condition step evaluates to `true`\.

**To define a TransformStep to perform batch transformation**

1. Create a transformer instance with the appropriate compute instance type, instance count, and desired output Amazon S3 bucket URI\. Pass in the `ModelName` property from the `step_create_model` `CreateModel` step\. 

   ```
   from sagemaker.transformer import Transformer
   
   
   transformer = Transformer(
       model_name=step_create_model.properties.ModelName,
       instance_type="ml.m5.xlarge",
       instance_count=1,
       output_path=f"s3://{default_bucket}/AbaloneTransform"
   )
   ```

1. Create a `TransformStep` using the transformer instance you defined and the `batch_data` pipeline parameter\.

   ```
   from sagemaker.inputs import TransformInput
   from sagemaker.workflow.steps import TransformStep
   
   
   step_transform = TransformStep(
       name="AbaloneTransform",
       transformer=transformer,
       inputs=TransformInput(data=batch_data)
   )
   ```

### Step 8: Define a RegisterModel Step to Create a Model Package<a name="define-pipeline-register"></a>

**Important**  
We recommend using [Model Step](build-and-manage-steps.md#step-type-model) to register models as of v2\.90\.0 of the SageMaker Python SDK\. `RegisterModel` will continue to work in previous versions of the SageMaker Python SDK, but is no longer actively supported\.

This section shows how to construct an instance of `RegisterModel`\. The result of executing `RegisterModel` in a pipeline is a model package\. A model package is a reusable model artifacts abstraction that packages all ingredients necessary for inference\. It consists of an inference specification that defines the inference image to use along with an optional model weights location\. A model package group is a collection of model packages\. You can use a `ModelPackageGroup` for SageMaker Pipelines to add a new version and model package to the group for every pipeline execution\. For more information about model registry, see [Register and Deploy Models with Model Registry](model-registry.md)\.

This step is passed into the condition step and only executes if the condition step evaluates to `true`\.

**To define a RegisterModel step to create a model package**
+  Construct a `RegisterModel` step using the estimator instance you used for the training step \. Pass in the `S3ModelArtifacts` property from the `step_train` training step and specify a `ModelPackageGroup`\. SageMaker Pipelines creates this `ModelPackageGroup` for you\.

  ```
  from sagemaker.model_metrics import MetricsSource, ModelMetrics 
  from sagemaker.workflow.step_collections import RegisterModel
  
  
  model_metrics = ModelMetrics(
      model_statistics=MetricsSource(
          s3_uri="{}/evaluation.json".format(
              step_eval.arguments["ProcessingOutputConfig"]["Outputs"][0]["S3Output"]["S3Uri"]
          ),
          content_type="application/json"
      )
  )
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

### Step 9: Define a Condition Step to Verify Model Accuracy<a name="define-pipeline-condition"></a>

A `ConditionStep` allows SageMaker Pipelines to support conditional execution in your pipeline DAG based on the condition of step properties\. In this case, you only want to register a model package if the accuracy of that model, as determined by the model evaluation step, exceeds the required value\. If the accuracy exceeds the required value, the pipeline also creates a SageMaker Model and runs batch transformation on a dataset\. This section shows how to define the Condition step\.

**To define a condition step to verify model accuracy**

1.  Define a `ConditionLessThanOrEqualTo` condition using the accuracy value found in the output of the model evaluation processing step, `step_eval`\. Get this output using the property file you indexed in the processing step and the respective JSONPath of the mean squared error value, `"mse"`\.

   ```
   from sagemaker.workflow.conditions import ConditionLessThanOrEqualTo
   from sagemaker.workflow.condition_step import ConditionStep
   from sagemaker.workflow.functions import JsonGet
   
   
   cond_lte = ConditionLessThanOrEqualTo(
       left=JsonGet(
           step_name=step_eval.name,
           property_file=evaluation_report,
           json_path="regression_metrics.mse.value"
       ),
       right=6.0
   )
   ```

1.  Construct a `ConditionStep`\. Pass the `ConditionEquals` condition in, then set the model package registration and batch transformation steps as the next steps if the condition passes\. 

   ```
   step_cond = ConditionStep(
       name="AbaloneMSECond",
       conditions=[cond_lte],
       if_steps=[step_register, step_create_model, step_transform],
       else_steps=[], 
   )
   ```

### Step 10: Create a pipeline<a name="define-pipeline-pipeline"></a>

Now that you’ve created all of the steps, combine them into a pipeline\.

**To create a pipeline**

1.  Define the following for your pipeline: `name`, `parameters`, and `steps`\. Names must be unique within an `(account, region)` pair\.
**Note**  
A step can only appear once in either the pipeline's step list or the if/else step lists of the condition step\. It cannot appear in both\. 

   ```
   from sagemaker.workflow.pipeline import Pipeline
   
   
   pipeline_name = f"AbalonePipeline"
   pipeline = Pipeline(
       name=pipeline_name,
       parameters=[
           processing_instance_count,
           model_approval_status,
           input_data,
           batch_data,
       ],
       steps=[step_process, step_train, step_eval, step_cond],
   )
   ```

1.  \(Optional\) Examine the JSON pipeline definition to ensure that it's well\-formed\.

   ```
   import json
   
   json.loads(pipeline.definition())
   ```

 This pipeline definition is ready to submit to SageMaker\. In the next tutorial, you submit this pipeline to SageMaker and start an execution\. 

 **Next step:** [Run a pipeline](run-pipeline.md) 