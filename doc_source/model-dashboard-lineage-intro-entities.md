# Introduction to entities<a name="model-dashboard-lineage-intro-entities"></a>

Amazon SageMaker automatically creates tracking entities for SageMaker jobs, models, model packages, and endpoints if the data is available\. For a basic workflow, suppose you train a model using a dataset\. SageMaker automatically generates a lineage graph with three entities: 
+ **Dataset** : A type of artifact, which is an entity representing a URI addressable object or data\. An artifact is generally either an input or an output to a trial component or action\.
+ **TrainingJob**: A type of trial component, which is an entity representing processing, training, and transform jobs\.
+ **Model**: Another type of artifact\. Like the **Dataset** artifact, a **Model** is a URI addressable object\. In this case, it is an output of the **TrainingJob** trial component\. 

Your model lineage graph expands quickly if you add additional steps to your workflow, such as data preprocessing or postprocessing, if you deploy your model to an endpoint, or if you include your model in a model package, among many other possibilities\. For the complete list of SageMaker entities, see [Amazon SageMaker ML Lineage Tracking](lineage-tracking.md)\.

## Entity properties<a name="model-dashboard-lineage-entity-properties"></a>

Each node in the graph displays the entity type, but you can choose the vertical ellipsis to the right of the entity type to see specific details related to your workflow\. In our previous barebones lineage graph, you can choose the vertical ellipsis next to **DataSet** to see specific values for the following properties \(common to all artifact entities\):
+ **Name**: The name of your dataset\.
+ **Source URI**: The Amazon S3 location of your dataset\.

For the `TrainingJob` entity, you can see the specific values for the following properties \(common to all `TrialComponent` entities\):
+ **Name**: The name of the training job\.
+ **Job ARN**: The Amazon Resource Name \(ARN\) of your training job\.

For the **Model** entity, you see the same properties as listed for **DataSet** since they are both artifact entities\. For a list of the entities and their associated properties, see [Lineage Tracking Entities](lineage-tracking-entities.md)\.

## Entity queries<a name="model-dashboard-lineage-entity-queries"></a>

Amazon SageMaker automatically generates graphs of lineage entities as you use them\. However if you are running many iterations of an experiment and don't want to view every lineage graph, the AWS SDK can help you perform queries across all your workflows\. For example, you can query your lineage entities for all the processing jobs that use an endpoint\. Or, you can see all the downstream trails that use an artifact\. For a list of all the queries you can perform, see [Querying Lineage Entities](querying-lineage-entities.md)\.