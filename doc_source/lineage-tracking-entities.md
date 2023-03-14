# Lineage Tracking Entities<a name="lineage-tracking-entities"></a>

Tracking entities maintain a representation of all the elements of your end\-to\-end machine learning workflow\. You can use this representation to establish model governance, reproduce your workflow, and maintain a record of your work history\.

Amazon SageMaker automatically creates tracking entities for trial components and their associated trials and experiments when you create SageMaker jobs such as processing jobs, training jobs, and batch transform jobs\. In additional to auto tracking, you can also [Manually Create Tracking Entities](lineage-tracking-manual-creation.md) to model custom steps in your workflow\. For more information, see [Manage Machine Learning with Amazon SageMaker ExperimentsSupported AWS Regions](experiments.md)\.

SageMaker also automatically creates tracking entities for the other steps in a workflow so you can track the workflow from end to end\. For more information, see [Amazon SageMaker–Created Tracking Entities](lineage-tracking-auto-creation.md)\.

You can create additional entities to supplement those created by SageMaker\. For more information, see [Manually Create Tracking Entities](lineage-tracking-manual-creation.md)\.

SageMaker reuses any existing entities rather than creating new ones\. For example, there can be only one artifact with a unique `SourceUri`\.

**Key concepts for querying lineage**
+ **Lineage** – Metadata that tracks the relationships between various entities in your ML workflows\.
+ **QueryLineage** – The action to inspect your lineage and discover relationships between entities\.
+ **Lineage entities** – The metadata elements of which your lineage is composed\.
+ **Cross\-account lineage** – Your ML workflow may span more than one account\. With cross\-account lineage, you can configure multiple accounts to automatically create lineage associations between shared entity resources\. QueryLineage then can return entities even from these shared accounts\.

The following tracking entities are defined:

**Experiment entities**
+ [Trial component](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrialComponent.html) – A stage of a machine learning trial\. Includes processing jobs, training jobs, and batch transform jobs\.
+ [Trial](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrial.html) – A combination of trial components that generally produces a model\.
+ [Experiment](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateExperiment.html) – A grouping of trials generally focused on solving a specific use case\.

**Lineage entities**
+ [Trial Component](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrialComponent.html) – Represents processing, training, and transform jobs in the lineage\. Also part of experiment management\.
+ [Context](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateContext.html) – Provides a logical grouping of other tracking or experiment entities\. Conceptually, experiments and trials are contexts\. Some examples are an endpoint and a model package\.
+ [Action](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAction.html) – Represents an action or activity\. Generally, an action involves at least one input artifact or output artifact\. Some examples are a workflow step and a model deployment\.
+ [Artifact](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateArtifact.html) – Represents a URI addressable object or data\. An artifact is generally either an input or an output to a trial component or action\. Some examples include a dataset \(S3 bucket URI\), or an image \(Amazon ECR registry path\)\.
+ [Association](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AddAssociation.html) – Links other tracking or experiment entities, such as an association between the location of training data and a training job\.

  An association has an optional `AssociationType` property\. The following values are available along with the suggested use for each type\. SageMaker places no restrictions on their use:
  + `ContributedTo` – The source contributed to the destination or had a part in enabling the destination\. For example, the training data contributed to the training job\.
  + `AssociatedWith` – The source is connected to the destination\. For example, an approval workflow is associated with a model deployment\.
  + `DerivedFrom` \- The destination is a modification of the source\. For example, a digest output of a channel input for a processing job is derived from the original inputs\.
  + `Produced` – The source generated the destination\. For example, a training job produced a model artifact\.
  + `SameAs` – When the same lineage entity used in different accounts\.

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
  + `ProjectId` – For example, the ID of the SageMaker MLOps project to which a model belongs\.
  + `GeneratedBy` – For example, the SageMaker pipeline execution that registered a model package version\.
  + `Repository` – For example, the repository that contains an algorithm\.
  + `CommitId` – For example, the commit ID of an algorithm version\.