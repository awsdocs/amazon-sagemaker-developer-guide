# Tracking Entities<a name="lineage-tracking-entities"></a>

Tracking entities maintain a representation of all the elements of your end\-to\-end machine learning workflow\. You can use this representation to establish model governance, reproduce your workflow, and maintain a record of your work history\.

Amazon SageMaker automatically creates tracking entities for trial components and their associated trials and experiments when you create SageMaker jobs such as processing jobs, training jobs, and batch transform jobs\. For more information, see [Manage Machine Learning with Amazon SageMaker Experiments](experiments.md)\.

SageMaker also automatically creates tracking entities for the other steps in a workflow that enable you to track the workflow from end to end\. For more information, see [Amazon SageMaker Created Tracking Entities](lineage-tracking-auto-creation.md)\.

You can create additional entities to supplement those created by SageMaker\. For more information, see [Manually Create Tracking Entities](lineage-tracking-manual-creation.md)\.

SageMaker reuses any existing entities rather than create new ones\. For example, there can be only one artifact with a unique `SourceUri`\.

The following tracking entities are defined:

**Experiment entities**
+ [Trial component](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrialComponent.html) – A stage of a machine learning trial\. Includes processing jobs, training jobs, and batch transform jobs\.
+ [Trial](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrial.html) – A combination of trial components that generally produces a model\.
+ [Experiment](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateExperiment.html) – A grouping of trials generally focused on solving a specific use case\.

**Lineage entities**
+ [Context](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateContext.html) – Provides a logical grouping of other tracking or experiment entities\. Conceptually, experiments and trials are contexts\. Some examples are an endpoint and a model package\.
+ [Action](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAction.html) – Represents an action or activity\. Generally, an action involves at least one input artifact or output artifact\. Some examples are a workflow step and a model deployment\.
+ [Artifact](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateArtifact.html) – Represents a URI addressable object or data\. An artifact is generally either an input or an output to a trial component or Action\. Some examples include a dataset \(Amazon S3 bucket URI\), an image \(Amazon ECR registry path\), or an action \(ARN\)\.
+ [Association](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AddAssociation.html) – Links other tracking or experiment entities\. For example, an association between the location of training data and a training job\.

  An association has an optional `AssociationType` property\. The following values are available along with the suggested use for each type\. SageMaker places no restrictions on their use:
  + `ContributedTo` – The source contributed to the destination or had a part in enabling the destination\. For example, the training data contributed to the training job\.
  + `AssociatedWith` – The source is connected to the destination\. For example, an approval workflow is associated with a model deployment\.
  + `DerivedFrom` \- The destination is a modification of the source\. For example, a digest output of a channel input for a processing job is derived from the original inputs\.
  + `Produced` – The source generated the destination\. For example, a training job produced a model artifact\.

**Common properties**
+ **Type property**

  The action, artifact, and context entities have a *type* property, `ActionType`, `ArtifactType`, and `ContextType`, respectively\. This property is a custom string which can associate meaningful information with the entity and be used as a filter in the List APIs\.
+ **Source property**

  The action, artifact, and context entities have a `Source` property\. This property provides the underlying URI that the entity represents\. Some examples are:
  + An `UpdateEndpoint` action where the source is the `EndpointArn`\.
  + An image artifact for a processing job where the source is the `ImageUri`\.
  + An `Endpoint` context where the source is the `EndpointArn`\.
+ **Metadata property**

  The action and artifact entities have an optional `Metadata` property which can provide the following information:
  + `ProjectId` – For example, the ID of the SageMaker MLOps project a model belongs to\.
  + `GeneratedBy` – For example, the SageMaker pipeline execution that registered a model package version\.
  + `Repository` – For example, the repository that contains an algorithm\.
  + `CommitId` – For example, the commit ID of an algorithm version\.