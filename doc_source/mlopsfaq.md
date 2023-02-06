# Amazon SageMaker Workflows FAQ<a name="mlopsfaq"></a>

Use the following FAQ items to find answers to commonly asked questions about MLOps workflows\.

## Q\. Do I need to use the SageMaker Python SDK to create a SageMaker pipeline?<a name="collapsible-section-1"></a>

No, the SageMaker Python SDK is not required to create a SageMaker pipeline\. You can also use [boto3](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_pipeline) or [AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-sagemaker-pipeline.html)\. Creating a pipeline requires a pipeline definition, which is a JSON object that defines each step of the pipeline\. The SageMaker SDK offers a simple way to construct the pipeline definition, which you can use with any of the APIs previously mentioned to create the pipeline itself\. Without using the SDK, users have to write the raw JSON definition to create the pipeline without any of the error checks provided by the SageMaker Python SDK\. To see the schema for the pipeline JSON definition, see [ SageMaker Pipeline Definition JSON Schema](https://aws-sagemaker-mlops.github.io/sagemaker-model-building-pipeline-definition-JSON-schema/)\. The following code sample shows an example of a SageMaker pipeline definition JSON object:

```
{'Version': '2020-12-01',
 'Metadata': {},
 'Parameters': [{'Name': 'ProcessingInstanceType',
   'Type': 'String',
   'DefaultValue': 'ml.m5.xlarge'},
  {'Name': 'ProcessingInstanceCount', 'Type': 'Integer', 'DefaultValue': 1},
  {'Name': 'TrainingInstanceType',
   'Type': 'String',
   'DefaultValue': 'ml.m5.xlarge'},
  {'Name': 'ModelApprovalStatus',
   'Type': 'String',
   'DefaultValue': 'PendingManualApproval'},
  {'Name': 'ProcessedData',
   'Type': 'String',
   'DefaultValue': 'S3_URL',
{'Name': 'InputDataUrl',
   'Type': 'String',
   'DefaultValue': 'S3_URL',
 'PipelineExperimentConfig': {'ExperimentName': {'Get': 'Execution.PipelineName'},
  'TrialName': {'Get': 'Execution.PipelineExecutionId'}},
 'Steps': [{'Name': 'ReadTrainDataFromFS',
   'Type': 'Processing',
   'Arguments': {'ProcessingResources': {'ClusterConfig': {'InstanceType': 'ml.m5.4xlarge',
      'InstanceCount': 2,
      'VolumeSizeInGB': 30}},
    'AppSpecification': {'ImageUri': 'IMAGE_URI',
     'ContainerArguments': [....]},
    'RoleArn': 'ROLE',
      'ProcessingInputs': [...],
    'ProcessingOutputConfig': {'Outputs': [.....]},
    'StoppingCondition': {'MaxRuntimeInSeconds': 86400}},
   'CacheConfig': {'Enabled': True, 'ExpireAfter': '30d'}},
   ...
   ...
   ...
  }
```

## Q\. Why do I see a repack step in my SageMaker pipeline?<a name="collapsible-section-3"></a>

Model repacking happens when the pipeline needs to include a custom script in the compressed model file \(model\.tar\.gz\) to be uploaded to Amazon S3 and used to deploy a model to a SageMaker endpoint\. When SageMaker pipeline trains a model and registers it to the model registry, it introduces a repack step *if* the trained model output from the training job needs to include a custom inference script\. The repack step uncompresses the model, adds a new script, and recompresses the model\. Running the pipeline adds the repack step as a training job\.

## Q\. Can I use SageMaker Experiments with SageMaker Pipelines?<a name="collapsible-section-4"></a>

Yes\. SageMaker Pipelines is natively integrated with SageMaker Experiments\. You can use `PipelineExperimentConfig` when creating a pipeline and set your own SageMaker Experiment name\. Each run of the pipeline creates a trial, and each step in the pipeline corresponds to a `TrialComponent` within the trial\. If no trial name is specified in the experiment config, the pipeline execution ID is used as the trial name\. 

```
pipeline = Pipeline(
   name=pipeline_name,
    parameters=[...],
    steps=[...],
    sagemaker_session=sagemaker_session,
    pipeline_experiment_config=PipelineExperimentConfig(
        ExecutionVariables.PIPELINE_NAME,
        ExecutionVariables.PIPELINE_EXECUTION_ID
    )
)
```

## Q\. SageMaker Project templates have a model deploy repository that uses CloudFormation \(CFN\) to create an endpoint\. Are there ways to deploy the model without using CloudFormation?<a name="collapsible-section-5"></a>

You can customize the deploy repository in the project template to deploy the model from the model registry any way you like\. The template uses CloudFormation to create a real\-time endpoint, as an example\. You can update the deployment to use the SageMaker SDK, boto3, or any other API that can create endpoints instead of CFN\. If you need to update the CodeBuild steps as part of the deployment pipeline, you can create a custom template\.

## Q\. How do we pass the model file Amazon S3 URL from the train step to the model register step in a SageMaker pipeline at run time?<a name="collapsible-section-6"></a>

You can reference the model location as a property of the training step, as shown in the end\-to\-end example [CustomerChurn pipeline](https://github.com/aws-samples/amazon-sagemaker-immersion-day/blob/master/ML%20Pipelines%20scripts/pipeline.py) in Github\.

## Q\. If I am extending a prebuilt container to train an estimator or for a `ProcessingStep` on SageMaker Pipelines, is it necessary to copy the script to the container in the Dockerfile?<a name="collapsible-section-7"></a>

No, you can either copy the script to the container or pass it via the `entry_point` argument \(of your estimator entity\) or `code` argument \(of your processor entity\), as demonstrated in the following code sample\.

```
step_process = ProcessingStep(
    name="PreprocessAbaloneData",
    processor=sklearn_processor,
    inputs = [
        ProcessingInput(
            input_name='dataset',
            source=...,
            destination="/opt/ml/processing/code",
        )
    ],
    outputs=[
        ProcessingOutput(output_name="train", source="/opt/ml/processing/train", destination = processed_data_path),
        ProcessingOutput(output_name="validation", source="/opt/ml/processing/validation", destination = processed_data_path),
        ProcessingOutput(output_name="test", source="/opt/ml/processing/test", destination = processed_data_path),
    ],
    code=os.path.join(BASE_DIR, "process.py"), ## Code is passed through an argument
    cache_config = cache_config,
    job_arguments = ['--input', 'arg1']
)

sklearn_estimator = SKLearn(
    entry_point=os.path.join(BASE_DIR, "train.py"), ## Code is passed through the entry_point
    framework_version="0.23-1",
    instance_type=training_instance_type,
    role=role,
    output_path=model_path, # New
    sagemaker_session=sagemaker_session, # New
    instance_count=1, # New
    base_job_name=f"{base_job_prefix}/pilot-train",
    metric_definitions=[
        {'Name': 'train:accuracy', 'Regex': 'accuracy_train=(.*?);'},
        {'Name': 'validation:accuracy', 'Regex': 'accuracy_validation=(.*?);'}
    ],
)
```

## Q\. What’s the recommended way to manage dependencies for different SageMaker Pipelines steps?<a name="collapsible-section-8"></a>

You can use a SageMaker Projects template to implement image\-building CI/CD\. With this template, you can automate the CI/CD of images that are built and pushed to Amazon ECR\. Changes in the container files in your project’s source control repositories intiate the ML pipeline and deploy the latest version for your container\. For more information, see the blog [Create Amazon SageMaker projects with image building CI/CD pipelines](http://aws.amazon.com/blogs/machine-learning/create-amazon-sagemaker-projects-with-image-building-ci-cd-pipelines/)\.

## Q\. How do I provide SageMaker Project access to specific user profiles in Amazon SageMaker Studio?<a name="collapsible-section-10"></a>

Since SageMaker Projects is backed by Service Catalog, you must add each role that requires access to SageMaker Projects to the **Amazon SageMaker Solutions and ML Ops products** Portfolio in the service catalog\. You can do this on the **Groups, roles, and users** tab, as shown in the following image\. If each user profile in Studio has a different role, you should add each of those roles to the service catalog\. You can also do this while creating a user profile in Studio\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/projects/project-access3.png)

## Q\. Where do I see the properties associated with each SageMaker pipeline step so that I can use them in subsequent steps?<a name="collapsible-section-11"></a>

Each step in the pipeline uses the underlying SageMaker APIs for the corresponding jobs\. For example, `TrainingStep` invokes the `CreateTrainingJob` API and the step properties correspond to the response from `DescribeTrainingJob`\. The response output can be found in the API reference link for [DescribeTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeTrainingJob.html)\. You can follow the same procedure to get the properties for [TransformStep](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeTransformJob.html), [ ProcessingStep](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeProcessingJob.html), [TuningStep](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeHyperParameterTuningJob.html), and [CreateModelStep](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html)\. For more information about pipeline steps, see [Pipeline Steps](https://docs.aws.amazon.com/sagemaker/latest/dg/build-and-manage-steps.html)\. 

## Q\. What’s the best way to reproduce my model in SageMaker?<a name="collapsible-section-12"></a>

SageMaker’s Lineage Tracking service works in the backend to track all the metadata associated with your model training and deployment workflows\. This includes your training jobs, datasets used, pipelines, endpoints, and the actual models\. You can query the lineage service at any point to find the exact artifacts used to train a model\. Using those artifacts, you can recreate the same ML workflow to reproduce the model as long as you have access to the exact dataset that was used\. A trial component tracks the training job\. This trial component has all the parameters used as part of the training job\. If you don’t need to rerun the entire workflow, you can reproduce the training job to derive the same model\.