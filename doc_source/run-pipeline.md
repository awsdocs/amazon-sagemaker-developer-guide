# Run a pipeline<a name="run-pipeline"></a>

After you’ve created a pipeline definition using the SageMaker Python SDK, you can submit it to SageMaker to start your execution\. The following tutorial shows how to submit a pipeline, start an execution, examine the results of that execution, and delete your pipeline\. 

**Topics**
+ [Prerequisites](#run-pipeline-prereq)
+ [Step 1: Start the Pipeline](#run-pipeline-submit)
+ [Step 2: Examine a Pipeline Execution](#run-pipeline-examine)
+ [Step 3: Override Default Parameters for a Pipeline Execution](#run-pipeline-parametrized)
+ [Step 4: Stop and Delete a Pipeline Execution](#run-pipeline-delete)

## Prerequisites<a name="run-pipeline-prereq"></a>

This tutorial requires the following: 
+  A SageMaker notebook instance\.  
+  A SageMaker Pipelines pipeline definition\. This tutorial assumes you're using the pipeline definition created by completing the [Define a Pipeline](define-pipeline.md) tutorial\. 

## Step 1: Start the Pipeline<a name="run-pipeline-submit"></a>

First, you need to start the pipeline\. 

**To start the pipeline**

1. Examine the JSON pipeline definition to ensure that it's well\-formed\.

   ```
   import json
   
   json.loads(pipeline.definition())
   ```

1. Submit the pipeline definition to the SageMaker Pipelines service to create a pipeline if it doesn't exist, or update the pipeline if it does\. The role passed in is used by SageMaker Pipelines to create all of the jobs defined in the steps\. 

   ```
   pipeline.upsert(role_arn=role)
   ```

1. Start a pipeline execution\.

   ```
   execution = pipeline.start()
   ```

## Step 2: Examine a Pipeline Execution<a name="run-pipeline-examine"></a>

Next, you need to examine the pipeline execution\. 

**To examine a pipeline execution**

1.  Describe the pipeline execution status to ensure that it has been created and started successfully\.

   ```
   execution.describe()
   ```

1. Wait for the execution to finish\. 

   ```
   execution.wait()
   ```

1. List the execution steps and their status\.

   ```
   execution.list_steps()
   ```

   Your output should look like the following:

   ```
   [{'StepName': 'AbaloneTransform',
     'StartTime': datetime.datetime(2020, 11, 21, 2, 41, 27, 870000, tzinfo=tzlocal()),
     'EndTime': datetime.datetime(2020, 11, 21, 2, 45, 50, 492000, tzinfo=tzlocal()),
     'StepStatus': 'Succeeded',
     'CacheHitResult': {'SourcePipelineExecutionArn': ''},
     'Metadata': {'TransformJob': {'Arn': 'arn:aws:sagemaker:us-east-2:111122223333:transform-job/pipelines-cfvy1tjuxdq8-abalonetransform-ptyjoef3jy'}}},
    {'StepName': 'AbaloneRegisterModel',
     'StartTime': datetime.datetime(2020, 11, 21, 2, 41, 26, 929000, tzinfo=tzlocal()),
     'EndTime': datetime.datetime(2020, 11, 21, 2, 41, 28, 15000, tzinfo=tzlocal()),
     'StepStatus': 'Succeeded',
     'CacheHitResult': {'SourcePipelineExecutionArn': ''},
     'Metadata': {'RegisterModel': {'Arn': 'arn:aws:sagemaker:us-east-2:111122223333:model-package/abalonemodelpackagegroupname/1'}}},
    {'StepName': 'AbaloneCreateModel',
     'StartTime': datetime.datetime(2020, 11, 21, 2, 41, 26, 895000, tzinfo=tzlocal()),
     'EndTime': datetime.datetime(2020, 11, 21, 2, 41, 27, 708000, tzinfo=tzlocal()),
     'StepStatus': 'Succeeded',
     'CacheHitResult': {'SourcePipelineExecutionArn': ''},
     'Metadata': {'Model': {'Arn': 'arn:aws:sagemaker:us-east-2:111122223333:model/pipelines-cfvy1tjuxdq8-abalonecreatemodel-jl94rai0ra'}}},
    {'StepName': 'AbaloneMSECond',
     'StartTime': datetime.datetime(2020, 11, 21, 2, 41, 25, 558000, tzinfo=tzlocal()),
     'EndTime': datetime.datetime(2020, 11, 21, 2, 41, 26, 329000, tzinfo=tzlocal()),
     'StepStatus': 'Succeeded',
     'CacheHitResult': {'SourcePipelineExecutionArn': ''},
     'Metadata': {'Condition': {'Outcome': 'True'}}},
    {'StepName': 'AbaloneEval',
     'StartTime': datetime.datetime(2020, 11, 21, 2, 37, 34, 767000, tzinfo=tzlocal()),
     'EndTime': datetime.datetime(2020, 11, 21, 2, 41, 18, 80000, tzinfo=tzlocal()),
     'StepStatus': 'Succeeded',
     'CacheHitResult': {'SourcePipelineExecutionArn': ''},
     'Metadata': {'ProcessingJob': {'Arn': 'arn:aws:sagemaker:us-east-2:111122223333:processing-job/pipelines-cfvy1tjuxdq8-abaloneeval-zfraozhmny'}}},
    {'StepName': 'AbaloneTrain',
     'StartTime': datetime.datetime(2020, 11, 21, 2, 34, 55, 867000, tzinfo=tzlocal()),
     'EndTime': datetime.datetime(2020, 11, 21, 2, 37, 34, 34000, tzinfo=tzlocal()),
     'StepStatus': 'Succeeded',
     'CacheHitResult': {'SourcePipelineExecutionArn': ''},
     'Metadata': {'TrainingJob': {'Arn': 'arn:aws:sagemaker:us-east-2:111122223333:training-job/pipelines-cfvy1tjuxdq8-abalonetrain-tavd6f3wdf'}}},
    {'StepName': 'AbaloneProcess',
     'StartTime': datetime.datetime(2020, 11, 21, 2, 30, 27, 160000, tzinfo=tzlocal()),
     'EndTime': datetime.datetime(2020, 11, 21, 2, 34, 48, 390000, tzinfo=tzlocal()),
     'StepStatus': 'Succeeded',
     'CacheHitResult': {'SourcePipelineExecutionArn': ''},
     'Metadata': {'ProcessingJob': {'Arn': 'arn:aws:sagemaker:us-east-2:111122223333:processing-job/pipelines-cfvy1tjuxdq8-abaloneprocess-mgqyfdujcj'}}}]
   ```

1. After your pipeline execution is complete, download the resulting  `evaluation.json` file from Amazon S3 to examine the report\. 

   ```
   evaluation_json = sagemaker.s3.S3Downloader.read_file("{}/evaluation.json".format(
       step_eval.arguments["ProcessingOutputConfig"]["Outputs"][0]["S3Output"]["S3Uri"]
   ))
   json.loads(evaluation_json)
   ```

## Step 3: Override Default Parameters for a Pipeline Execution<a name="run-pipeline-parametrized"></a>

You can run additional executions of the pipeline by specifying different pipeline parameters to override the defaults\.

**To override default parameters**

1. Create the pipeline execution\. This starts another pipeline execution with the model approval status override set to "Approved"\. This means that the model package version generated by the `RegisterModel` step is automatically ready for deployment through CI/CD pipelines, such as with SageMaker Projects\. For more information, see [Automate MLOps with SageMaker Projects](sagemaker-projects.md)\.

   ```
   execution = pipeline.start(
       parameters=dict(
           ModelApprovalStatus="Approved",
       )
   )
   ```

1. Wait for the execution to finish\. 

   ```
   execution.wait()
   ```

1. List the execution steps and their status\.

   ```
   execution.list_steps()
   ```

1. After your pipeline execution is complete, download the resulting  `evaluation.json` file from Amazon S3 to examine the report\. 

   ```
   evaluation_json = sagemaker.s3.S3Downloader.read_file("{}/evaluation.json".format(
       step_eval.arguments["ProcessingOutputConfig"]["Outputs"][0]["S3Output"]["S3Uri"]
   ))
   json.loads(evaluation_json)
   ```

## Step 4: Stop and Delete a Pipeline Execution<a name="run-pipeline-delete"></a>

When you're finished with your pipeline, you can stop any ongoing executions and delete the pipeline\.

**To stop and delete a pipeline execution**

1. Stop the pipeline execution\.

   ```
   execution.stop()
   ```

1. Delete the pipeline\.

   ```
   pipeline.delete()
   ```