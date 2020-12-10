# Manually Create Tracking Entities<a name="lineage-tracking-manual-creation"></a>

You can manually create tracking entities for any property\. For information on the tracking entities that Amazon SageMaker automatically creates, see [Amazon SageMaker Created Tracking Entities](lineage-tracking-auto-creation.md)\. An error occurs if you try to create an entity that already exists\.

You can add tags to all entities except associations\. Tags are arbitrary key\-value pairs that provide custom information\. You can filter or sort a list or search query by tags\. For more information, see [Tagging AWS resources](https://docs.aws.amazon.com/general/latest/gr/aws_tagging.html) in the *AWS General Reference*\.

## Manually Create Tracking Entities<a name="lineage-tracking-manual-create"></a>

The following procedure shows you how to create and associate artifacts between a SageMaker training job and endpoint\. You perform the following steps:
+ Create input artifacts for the source code, training data, and testing data locations\.
+ Create an output artifact for the generated model location\.
+ Create a trial component as a training job\.
+ Associate the input artifact and output artifacts with the training job\.
+ Train the model and create an endpoint\.
+ Create a context for the endpoint\.
+ Associate the training job and the endpoint context\.

**To create tracking entities and associations**

1. Import the tracking entities\.

   ```
   import sys
     !{sys.executable} -m pip install -q sagemaker
   
     from sagemaker import get_execution_role
     from sagemaker.session import Session
     from sagemaker.lineage import context, artifact, association, action
   
     import boto3
     boto_session = boto3.Session(region_name=region)
     sagemaker_client = boto_session.client("sagemaker")
   ```

1. Create the input and output artifacts\.

   ```
   code_location_arn = artifact.Artifact.create(
         artifact_name='source-code-location',
         source_uri='s3://...',
         artifact_type='code-location'
     ).artifact_arn
   
     # Similar constructs for train_data_location_arn and test_data_location_arn
   
     model_location_arn = artifact.Artifact.create(
         artifact_name='model-location',
         source_uri='s3://...',
         artifact_type='model-location'
     ).artifact_arn
   ```

1. Train the model and get the `trial_component_arn` that represents the training job\.

1. Associate the input artifacts and output artifacts with the training job \(trial component\)\.

   ```
   input_artifacts = [code_location_arn, train_data_location_arn, test_data_location_arn]
     for artifact_arn in input_artifacts:
         try:
             association.Association.create(
                 source_arn=artifact_arn,
                 destination_arn=trial_component_arn,
                 association_type='ContributedTo'
             )
         except:
             logging.info('association between {} and {} already exists', artifact_arn, trial_component_arn)
   
     output_artifacts = [model_location_arn]
     for artifact_arn in output_artifacts:
         try:
              association.Association.create(
                 source_arn=trial_component_arn,
                 destination_arn=artifact_arn,
                 association_type='Produced'
             )
         except:
             logging.info('association between {} and {} already exists', artifact_arn, trial_component_arn)
   ```

1. Create the inference endpoint\.

   ```
   predictor = mnist_estimator.deploy(initial_instance_count=1,
                                        instance_type='ml.m4.xlarge')
   ```

1. Create the endpoint context\.

   ```
   from sagemaker.lineage import context
   
     endpoint = sagemaker_client.describe_endpoint(EndpointName=predictor.endpoint_name)
     endpoint_arn = endpoint['EndpointArn']
   
     endpoint_context_arn = context.Context.create(
         context_name=predictor.endpoint_name,
         context_type='Endpoint',
         source_uri=endpoint_arn
     ).context_arn
   ```

1. Associate the training job \(trial component\) and endpoint context\.

   ```
   association.Association.create(
         source_arn=trial_component_arn,
         destination_arn=endpoint_context_arn
     )
   ```

## Manually Track a Workflow<a name="lineage-tracking-manual-track"></a>

You can manually track the workflow created in the previous section\.

Given the endpoint Amazon Resource Name \(ARN\) from the previous example, the following procedure shows you how to track the workflow back to the datasets used to train the model that was deployed to the endpoint\. You perform the following steps:
+ Given the endpoint ARN, get the endpoint context\.
+ Get the trial component from the association between the trial component and the endpoint context\.
+ Get the training data location artifact from the association between the trial component and the endpoint context\.
+ Get the training data location from the training data location artifact\.

**To track a workflow from endpoint to training data source**

1. Import the tracking entities\.

   ```
   import sys
     !{sys.executable} -m pip install -q sagemaker
   
     from sagemaker import get_execution_role
     from sagemaker.session import Session
     from sagemaker.lineage import context, artifact, association, action
   
     import boto3
     boto_session = boto3.Session(region_name=region)
     sagemaker_client = boto_session.client("sagemaker")
   ```

1. Get the endpoint context from the endpoint ARN\.

   ```
   endpoint_context_arn = sagemaker_client.list_contexts(
       SourceUri=endpoint_arn)['ContextSummaries'][0]['ContextArn']
   ```

1. Get the trial component from the association between the trial component and the endpoint context\.

   ```
   trial_component_arn = sagemaker_client.list_associations(
       DestinationArn=endpoint_context_arn)['AssociationSummaries'][0]['SourceArn']
   ```

1. Get the training data location artifact from the association between the trial component and the endpoint context\.

   ```
   train_data_location_artifact_arn = sagemaker_client.list_associations(
       DestinationArn=trial_source_arn)['AssociationSummaries'][0]['SourceArn']
   ```

1. Get the training data location from the training data location artifact\.

   ```
   train_data_location = sagemaker_client.describe_artifact(
       ArtifactArn=train_data_location_artifact_arn)['Source']['SourceUri']
       print(train_data_location)
   ```

   Response:

   ```
   s3://sagemaker-sample-data-us-east-2/mxnet/mnist/train
   ```