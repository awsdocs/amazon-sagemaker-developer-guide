# Querying Lineage Entities<a name="querying-lineage-entities"></a>

Amazon SageMaker automatically generates graphs of lineage entities as you use them\. You can query this data to answer a variety of questions\. You can query your lineage entities to: 
+ Retrieve all data sets that went into the creation of a model\.
+ Retrieve all jobs that went into the creation of an endpoint\.
+ Retrieve all models that use a data set\.
+ Retrieve all endpoints that use a model\.
+ Retrieve which endpoints are derived from a certain data set\.
+ Retrieve the pipeline execution that created a training job\.
+ Retrieve the relationships between entities for investigation, governance, and reproducibility\.
+ Retrieve all downstream trials that use the artifact\.
+ Retrieve all upstream trials that use the artifact\.
+ Retrieve a list of artifacts that use the provided S3 uri\.
+ Retrieve upstream artifacts that use the dataset artifact\.
+ Retrieve downstream artifacts that use the dataset artifact\.
+ Retrieve datasets that use the image artifact\.
+ Retrieve actions that use the context\.
+ Retrieve processing jobs that use the endpoint\.
+ Retrieve transform jobs that use the endpoint\.
+ Retrieve trial components that use the endpoint\.
+ Retrieve the ARN for the pipeline execution associated with the model package group\.
+ Retrieve all artifacts that use the action\.
+ Retrieve all upstream datasets that use the model package approval action\.
+ Retrieve model package from model package approval action\.
+ Retrieve downstream endpoint contexts that use the endpoint\.
+ Retrieve the ARN for the pipeline execution associated with the trial component\.
+ Retrieve datasets that use the trial component\.
+ Retrieve models that use the trial component\.
+ Explore your lineage for visualization\.

**Limitations**
+ Lineage querying is not available in the following Regions:
  + Africa \(Cape Town\) – af\-south
  + Asia Pacific \(Jakarta\) – ap\-southeast\-3
  + Asia Pacific \(Osaka\) – ap\-northeast\-3
  + Europe \(Milan\) – eu\-south\-1
+ The maximum depth of relationships to discover is currently limited to 10\.
+ Filtering is limited to the following properties: last modified date, created date, type, and lineage entity type\. 

**Topics**
+ [Getting Started with Querying Lineage Entities](#querying-lineage-entities-getting-started)

## Getting Started with Querying Lineage Entities<a name="querying-lineage-entities-getting-started"></a>

The easiest way to get started is either via the:
+ [ Amazon SageMaker SDK for Python](https://github.com/aws/sagemaker-python-sdk/blob/master/src/sagemaker/lineage/artifact.py#L397) which has defined many common use cases\.
+ For a notebook that demonstrates how to use SageMaker Lineage APIs to query relationships across the lineage graph, see [sagemaker\-lineage\-multihop\-queries\.ipynb](https://github.com/aws/amazon-sagemaker-examples/blob/master/sagemaker-lineage/sagemaker-lineage-multihop-queries.ipynb)\.

The following examples show how to use the `LineageQuery` and `LineageFilter` APIs to construct queries to answer questions about the Lineage Graph and extract entity relationships for a few use cases\.

**Example Using the `LineageQuery` API to find entity associations**  

```
from sagemaker.lineage.context import Context, EndpointContext
from sagemaker.lineage.action import Action
from sagemaker.lineage.association import Association
from sagemaker.lineage.artifact import Artifact, ModelArtifact, DatasetArtifact

from sagemaker.lineage.query import (
    LineageQuery,
    LineageFilter,
    LineageSourceEnum,
    LineageEntityEnum,
    LineageQueryDirectionEnum,
)
# Find the endpoint context and model artifact that should be used for the lineage queries.

contexts = Context.list(source_uri=endpoint_arn)
context_name = list(contexts)[0].context_name
endpoint_context = EndpointContext.load(context_name=context_name)
```

**Example Find all the datasets associated with an endpoint**  

```
# Define the LineageFilter to look for entities of type `ARTIFACT` and the source of type `DATASET`.

query_filter = LineageFilter(
    entities=[LineageEntityEnum.ARTIFACT], sources=[LineageSourceEnum.DATASET]
)

# Providing this `LineageFilter` to the `LineageQuery` constructs a query that traverses through the given context `endpoint_context`
# and find all datasets.

query_result = LineageQuery(sagemaker_session).query(
    start_arns=[endpoint_context.context_arn],
    query_filter=query_filter,
    direction=LineageQueryDirectionEnum.ASCENDANTS,
    include_edges=False,
)

# Parse through the query results to get the lineage objects corresponding to the datasets
dataset_artifacts = []
for vertex in query_result.vertices:
    dataset_artifacts.append(vertex.to_lineage_object().source.source_uri)

pp.pprint(dataset_artifacts)
```

**Example Find the models associated with an endpoint**  

```
# Define the LineageFilter to look for entities of type `ARTIFACT` and the source of type `MODEL`.

query_filter = LineageFilter(
    entities=[LineageEntityEnum.ARTIFACT], sources=[LineageSourceEnum.MODEL]
)

# Providing this `LineageFilter` to the `LineageQuery` constructs a query that traverses through the given context `endpoint_context`
# and find all datasets.

query_result = LineageQuery(sagemaker_session).query(
    start_arns=[endpoint_context.context_arn],
    query_filter=query_filter,
    direction=LineageQueryDirectionEnum.ASCENDANTS,
    include_edges=False,
)

# Parse through the query results to get the lineage objects corresponding to the model
model_artifacts = []
for vertex in query_result.vertices:
    model_artifacts.append(vertex.to_lineage_object().source.source_uri)

# The results of the `LineageQuery` API call return the ARN of the model deployed to the endpoint along with
# the S3 URI to the model.tar.gz file associated with the model
pp.pprint(model_artifacts)
```

**Example Find the trial components associated with the endpoint**  

```
# Define the LineageFilter to look for entities of type `TRIAL_COMPONENT` and the source of type `TRAINING_JOB`.

query_filter = LineageFilter(
    entities=[LineageEntityEnum.TRIAL_COMPONENT],
    sources=[LineageSourceEnum.TRAINING_JOB],
)

# Providing this `LineageFilter` to the `LineageQuery` constructs a query that traverses through the given context `endpoint_context`
# and find all datasets.

query_result = LineageQuery(sagemaker_session).query(
    start_arns=[endpoint_context.context_arn],
    query_filter=query_filter,
    direction=LineageQueryDirectionEnum.ASCENDANTS,
    include_edges=False,
)

# Parse through the query results to get the ARNs of the training jobs associated with this Endpoint
trial_components = []
for vertex in query_result.vertices:
    trial_components.append(vertex.arn)

pp.pprint(trial_components)
```

**Example Changing the focal point of lineage**  
The `LineageQuery` can be modified to have different `start_arns` which changes the focal point of lineage\. In addition, the `LineageFilter` can take multiple sources and entities to expand the scope of the query\.  
In the following we use the model as the lineage focal point and find the endpoints and datasets associated with it\.  

```
# Get the ModelArtifact

model_artifact_summary = list(Artifact.list(source_uri=model_package_arn))[0]
model_artifact = ModelArtifact.load(artifact_arn=model_artifact_summary.artifact_arn)
query_filter = LineageFilter(
    entities=[LineageEntityEnum.ARTIFACT],
    sources=[LineageSourceEnum.ENDPOINT, LineageSourceEnum.DATASET],
)

query_result = LineageQuery(sagemaker_session).query(
    start_arns=[model_artifact.artifact_arn],  # Model is the starting artifact
    query_filter=query_filter,
    # Find all the entities that descend from the model, i.e. the endpoint
    direction=LineageQueryDirectionEnum.DESCENDANTS,
    include_edges=False,
)

associations = []
for vertex in query_result.vertices:
    associations.append(vertex.to_lineage_object().source.source_uri)

query_result = LineageQuery(sagemaker_session).query(
    start_arns=[model_artifact.artifact_arn],  # Model is the starting artifact
    query_filter=query_filter,
    # Find all the entities that ascend from the model, i.e. the datasets
    direction=LineageQueryDirectionEnum.ASCENDANTS,
    include_edges=False,
)

for vertex in query_result.vertices:
    associations.append(vertex.to_lineage_object().source.source_uri)

pp.pprint(associations)
```

**Example Using `LineageQueryDirectionEnum.BOTH` to find ascendent and descendent relationships**  
When the direction is set to `BOTH`, the query traverses the graph to find ascendant and descendant relationships\. This traversal takes place not only from the starting node, but from each node that is visited\. For example; if a training job is run twice and both models generated by the training job are deployed to endpoints, the result of the query with direction set to `BOTH` shows both endpoints\. This is because the same image is used for training and deploying the model\. Since the image is common to the model, the `start_arn` and both the endpoints, appear in the query result\.  

```
query_filter = LineageFilter(
    entities=[LineageEntityEnum.ARTIFACT],
    sources=[LineageSourceEnum.ENDPOINT, LineageSourceEnum.DATASET],
)

query_result = LineageQuery(sagemaker_session).query(
    start_arns=[model_artifact.artifact_arn],  # Model is the starting artifact
    query_filter=query_filter,
    # This specifies that the query should look for associations both ascending and descending for the start
    direction=LineageQueryDirectionEnum.BOTH,
    include_edges=False,
)

associations = []
for vertex in query_result.vertices:
    associations.append(vertex.to_lineage_object().source.source_uri)

pp.pprint(associations)
```

**Example Directions in `LineageQuery` \- `ASCENDANTS` vs\. `DESCENDANTS`**  
To understand the direction in the Lineage Graph, take the following entity relationship graph \- Dataset \-> Training Job \-> Model \-> Endpoint  
The endpoint is a descendant of the model, and the model is a descendant of the dataset\. Similarly, the model is an ascendant of the endpoint\. The `direction` parameter can be used to specify whether the query should return entities that are descendants or ascendants of the entity in `start_arns`\. If the `start_arns` contains a model and the direction is `DESCENDANTS`, the query returns the endpoint\. If the direction is `ASCENDANTS`, the query returns the dataset\.  

```
# In this example, we'll look at the impact of specifying the direction as ASCENDANT or DESCENDANT in a `LineageQuery`.

query_filter = LineageFilter(
    entities=[LineageEntityEnum.ARTIFACT],
    sources=[
        LineageSourceEnum.ENDPOINT,
        LineageSourceEnum.MODEL,
        LineageSourceEnum.DATASET,
        LineageSourceEnum.TRAINING_JOB,
    ],
)

query_result = LineageQuery(sagemaker_session).query(
    start_arns=[model_artifact.artifact_arn],
    query_filter=query_filter,
    direction=LineageQueryDirectionEnum.ASCENDANTS,
    include_edges=False,
)

ascendant_artifacts = []

# The lineage entity returned for the Training Job is a TrialComponent which can't be converted to a
# lineage object using the method `to_lineage_object()` so we extract the TrialComponent ARN.
for vertex in query_result.vertices:
    try:
        ascendant_artifacts.append(vertex.to_lineage_object().source.source_uri)
    except:
        ascendant_artifacts.append(vertex.arn)

print("Ascendant artifacts : ")
pp.pprint(ascendant_artifacts)

query_result = LineageQuery(sagemaker_session).query(
    start_arns=[model_artifact.artifact_arn],
    query_filter=query_filter,
    direction=LineageQueryDirectionEnum.DESCENDANTS,
    include_edges=False,
)

descendant_artifacts = []
for vertex in query_result.vertices:
    try:
        descendant_artifacts.append(vertex.to_lineage_object().source.source_uri)
    except:
        # Handling TrialComponents.
        descendant_artifacts.append(vertex.arn)

print("Descendant artifacts : ")
pp.pprint(descendant_artifacts)
```

**Example SDK helper functions to make lineage queries easier**  
The classes `EndpointContext`, `ModelArtifact`, and `DatasetArtifact` have helper functions that are wrappers over the `LineageQuery` API to make certain lineage queries easier to leverage\. The following example shows how to use these helper function\.  

```
# Find all the datasets associated with this endpoint

datasets = []
dataset_artifacts = endpoint_context.dataset_artifacts()
for dataset in dataset_artifacts:
    datasets.append(dataset.source.source_uri)
print("Datasets : ", datasets)

# Find the training jobs associated with the endpoint
training_job_artifacts = endpoint_context.training_job_arns()
training_jobs = []
for training_job in training_job_artifacts:
    training_jobs.append(training_job)
print("Training Jobs : ", training_jobs)

# Get the ARN for the pipeline execution associated with this endpoint (if any)
pipeline_executions = endpoint_context.pipeline_execution_arn()
if pipeline_executions:
    for pipeline in pipelines_executions:
        print(pipeline)

# Here we use the `ModelArtifact` class to find all the datasets and endpoints associated with the model

dataset_artifacts = model_artifact.dataset_artifacts()
endpoint_contexts = model_artifact.endpoint_contexts()

datasets = [dataset.source.source_uri for dataset in dataset_artifacts]
endpoints = [endpoint.source.source_uri for endpoint in endpoint_contexts]

print("Datasets associated with this model : ")
pp.pprint(datasets)

print("Endpoints associated with this model : ")
pp.pprint(endpoints)

# Here we use the `DatasetArtifact` class to find all the endpoints hosting models that were trained with a particular dataset
# Find the artifact associated with the dataset

dataset_artifact_arn = list(Artifact.list(source_uri=training_data))[0].artifact_arn
dataset_artifact = DatasetArtifact.load(artifact_arn=dataset_artifact_arn)

# Find the endpoints that used this training dataset
endpoint_contexts = dataset_artifact.endpoint_contexts()
endpoints = [endpoint.source.source_uri for endpoint in endpoint_contexts]

print("Endpoints associated with the training dataset {}".format(training_data))
pp.pprint(endpoints)
```

**Example Getting a Lineage graph visualization**  
A helper class `Visualizer` is provided in the sameple notebook [visualizer\.py ](https://github.com/aws/amazon-sagemaker-examples/blob/master/sagemaker-lineage/visualizer.py) to help plot the lineage graph\. When the query response is rendered, a graph with the lineage relationships from the `StartArns` is displayed\. From the `StartArns` the visualization shows the relationships with the other lineage entities returned in the `query_lineage` API action\.  

```
# Graph APIs
# Here we use the boto3 `query_lineage` API to generate the query response to plot.

from visualizer import Visualizer

query_response = sm_client.query_lineage(
    StartArns=[endpoint_context.context_arn], Direction="Ascendants", IncludeEdges=True
)

viz = Visualizer()
viz.render(query_response, "Endpoint")
        
        query_response = sm_client.query_lineage(
    StartArns=[model_artifact.artifact_arn], Direction="Ascendants", IncludeEdges=True
)
viz.render(query_response, "Model")
```