# Caching Pipeline Steps<a name="pipelines-caching"></a>

When you use step signature caching, SageMaker Pipelines tries to find a previous run of your current pipeline step with the same values for certain attributes\. If found, SageMaker Pipelines propagates the outputs from the previous run rather than recomputing the step\. The attributes checked are specific to the step type, and are listed in [Default cache key attributes by pipeline step type](#pipelines-default-keys)\.

You must opt in to step caching — it is off by default\. When you turn on step caching, you must also define a timeout\. This timeout defines how old a previous run can be to remain a candidate for reuse\.

Step caching only considers successful runs — it never reuses failed runs\. A previous run of a pipeline step is considered successful when the run itself and the entire pipeline are both successful\. When multiple successful runs exist within the timeout period, SageMaker Pipelines uses the result for the most recent successful run\. If no successful runs match in the timeout period, SageMaker Pipelines reruns the step\. If the executor finds a previous run that meets the criteria but is still in progress, both steps continue running and update the cache if they're successful\.

Step caching is only scoped for individual pipelines, so you can’t reuse a step from another pipeline even if there is a step signature match\.

Step caching is available for the following step types: 
+ [Processing](build-and-manage-steps.md#step-type-processing)
+ [Training](build-and-manage-steps.md#step-type-training)
+ [Tuning](build-and-manage-steps.md#step-type-tuning)
+ [Transform](build-and-manage-steps.md#step-type-transform)
+ [`ClarifyCheck`](build-and-manage-steps.md#step-type-clarify-check)
+ [`QualityCheck`](build-and-manage-steps.md#step-type-quality-check)
+ [EMR](build-and-manage-steps.md#step-type-emr)

**Topics**
+ [Turn on step caching](#pipelines-caching-enabling)
+ [Turn off step caching](#pipelines-caching-disabling)
+ [Default cache key attributes by pipeline step type](#pipelines-default-keys)
+ [Cached data access control](#pipelines-access-control)

## Turn on step caching<a name="pipelines-caching-enabling"></a>

To turn on step caching, you must add a `CacheConfig` property to the step definition\.

`CacheConfig` properties use the following format in the pipeline definition file:

```
{
    "CacheConfig": {
        "Enabled": false,
        "ExpireAfter": "<time>"
    }
}
```

The `Enabled` field indicates whether caching is turned on for the particular step\. You can set the field to `true`, which tells SageMaker to try to find a previous run of the step with the same attributes\. Or, you can set the field to `false`, which tells SageMaker to run the step every time the pipeline runs\. `ExpireAfter` is a string in [ISO 8601 duration](https://en.wikipedia.org/wiki/ISO_8601#Durations) format that defines the timeout period\. The `ExpireAfter` duration can be a year, month, week, day, hour, or minute value\. Each value consists of a number followed by a letter indicating the unit of duration\. For example:
+ "30d" = 30 days
+ "5y" = 5 years
+ "T16m" = 16 minutes
+ "30dT5h" = 30 days and 5 hours\.

The following discussion describes the procedure to turn on caching for new or pre\-existing pipelines using the Amazon SageMaker Python SDK\.

**Turn on caching for new pipelines**

For new pipelines, initialize a `CacheConfig` instance with `enable_caching=True` and provide it as an input to your pipeline step\. The following example turns on caching with a 1\-hour timeout period for a training step: 

```
from sagemaker.workflow.pipeline_context import PipelineSession
from sagemaker.workflow.steps import CacheConfig
      
cache_config = CacheConfig(enable_caching=True, expire_after="PT1H")
estimator = Estimator(..., sagemaker_session=PipelineSession())

step_train = TrainingStep(
    name="TrainAbaloneModel",
    step_args=estimator.fit(inputs=inputs),
    cache_config=cache_config
)
```

**Turn on caching for pre\-existing pipelines**

To turn on caching for pre\-existing, already\-defined pipelines, turn on the `enable_caching` property for the step, and set `expire_after` to a timeout value\. Lastly, update the pipeline with `pipeline.upsert()` or `pipeline.update()`\. Once you run it again, the following code example turns on caching with a 1\-hour timeout period for a training step:

```
from sagemaker.workflow.pipeline_context import PipelineSession
from sagemaker.workflow.steps import CacheConfig
from sagemaker.workflow.pipeline import Pipeline

cache_config = CacheConfig(enable_caching=True, expire_after="PT1H")
estimator = Estimator(..., sagemaker_session=PipelineSession())

step_train = TrainingStep(
    name="TrainAbaloneModel",
    step_args=estimator.fit(inputs=inputs),
    cache_config=cache_config
)

# define pipeline
pipeline = Pipeline(
    steps=[step_train]
)

# additional step for existing pipelines
pipeline.update()
# or, call upsert() to update the pipeline
# pipeline.upsert()
```

Alternatively, update the cache config after you have already defined the \(pre\-existing\) pipeline, allowing one continuous code run\. The following code sample demonstrates this method:

```
# turn on caching with timeout period of one hour
pipeline.steps[0].cache_config.enable_caching = True 
pipeline.steps[0].cache_config.expire_after = "PT1H" 

# additional step for existing pipelines
pipeline.update()
# or, call upsert() to update the pipeline
# pipeline.upsert()
```

For more detailed code examples and a discussion about how Python SDK parameters affect caching, see [ Caching Configuration](https://sagemaker.readthedocs.io/en/stable/amazon_sagemaker_model_building_pipeline.html#caching-configuration) in the Amazon SageMaker Python SDK documentation\.

## Turn off step caching<a name="pipelines-caching-disabling"></a>

A pipeline step does not rerun if you change any attributes that are not listed in [Default cache key attributes by pipeline step type](#pipelines-default-keys) for its step type\. However, you may decide that you want the pipeline step to rerun anyway\. In this case, you need to turn off step caching\.

To turn off step caching, set the `Enabled` attribute in the step definition’s `CacheConfig` property in the step definition to `false`, as shown in the following code snippet:

```
{
    "CacheConfig": {
        "Enabled": false,
        "ExpireAfter": "<time>"
    }
}
```

Note that the `ExpireAfter` attribute is ignored when `Enabled` is `false`\.

To turn off caching for a pipeline step using the Amazon SageMaker Python SDK, define the pipeline of your pipeline step, turn off the `enable_caching` property, and update the pipeline\.

Once you run it again, the following code example turns off caching for a training step:

```
from sagemaker.workflow.pipeline_context import PipelineSession
from sagemaker.workflow.steps import CacheConfig
from sagemaker.workflow.pipeline import Pipeline

cache_config = CacheConfig(enable_caching=False, expire_after="PT1H")
estimator = Estimator(..., sagemaker_session=PipelineSession())

step_train = TrainingStep(
    name="TrainAbaloneModel",
    step_args=estimator.fit(inputs=inputs),
    cache_config=cache_config
)

# define pipeline
pipeline = Pipeline(
    steps=[step_train]
)

# update the pipeline
pipeline.update()
# or, call upsert() to update the pipeline
# pipeline.upsert()
```

Alternatively, turn off the `enable_caching` property after you have already defined the pipeline, allowing one continuous code run\. The following code sample demonstrates this solution:

```
# turn off caching for the training step
pipeline.steps[0].cache_config.enable_caching = False

# update the pipeline
pipeline.update()
# or, call upsert() to update the pipeline
# pipeline.upsert()
```

For more detailed code examples and a discussion about how Python SDK parameters affect caching, see [Caching Configuration](https://sagemaker.readthedocs.io/en/stable/amazon_sagemaker_model_building_pipeline.html#caching-configuration) in the Amazon SageMaker Python SDK documentation\.

## Default cache key attributes by pipeline step type<a name="pipelines-default-keys"></a>

When deciding whether to reuse a previous pipeline step or rerun the step, SageMaker Pipelines checks to see if certain attributes have changed\. If the set of attributes is different from all previous runs within the timeout period, the step runs again\. These attributes include input artifacts, app or algorithm specification, and environment variables\.

The following list shows each pipeline step type and the attributes that, if changed, initiate a rerun of the step\. For more information about which Python SDK parameters are used to create the following attributes, see [ Caching Configuration](https://sagemaker.readthedocs.io/en/stable/amazon_sagemaker_model_building_pipeline.html#caching-configuration) in the Amazon SageMaker Python SDK documentation\.

### [Processing step](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateProcessingJob.html)<a name="collapsible-caching-section-1"></a>
+ AppSpecification
+ Environment
+ ProcessingInputs\. This attribute contains information about the preprocessing script\. If the contents of the preprocessing script change, the processing step is rerun\.

  

### [Training step](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html)<a name="collapsible-caching-section-2"></a>
+ AlgorithmSpecification
+ CheckpointConfig
+ DebugHookConfig
+ DebugRuleConfigurations
+ Environment
+ HyperParameters
+ InputDataConfig\. This attribute contains information about the training script\. If the contents of the training script change, the training step is rerun\.

  

### [Tuning step](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateHyperParameterTuningJob.html)<a name="collapsible-caching-section-3"></a>
+ HyperParameterTuningJobConfig
+ TrainingJobDefinition\. This attribute is composed of multiple child attributes, not all of which cause the step to rerun\. The child attributes that could incur a rerun \(if changed\) are:
  + AlgorithmSpecification
  + HyperParameterRanges
  + InputDataConfig
  + StaticHyperParameters
  + TuningObjective
+ TrainingJobDefinitions

  

### [Transform step](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTransformJob.html)<a name="collapsible-caching-section-4"></a>
+ DataProcessing
+ Environment
+ ModelName
+ TransformInput

  

### [ClarifyCheck Step](build-and-manage-steps.md#step-type-clarify-check)<a name="collapsible-caching-section-5"></a>
+ ClarifyCheckConfig
+ CheckJobConfig
+ SkipCheck
+ RegisterNewBaseline
+ ModelPackageGroupName
+ SuppliedBaselineConstraints

  

### [QualityCheck Step](build-and-manage-steps.md#step-type-quality-check)<a name="collapsible-caching-section-6"></a>
+ QualityCheckConfig
+ CheckJobConfig
+ SkipCheck
+ RegisterNewBaseline
+ ModelPackageGroupName
+ SuppliedBaselineConstraints
+ SuppliedBaselineStatistics

  

### [EMR Step](https://docs.aws.amazon.com/sagemaker/latest/dg/build-and-manage-steps.html#step-type-emr)<a name="collapsible-caching-section-7"></a>
+ ClusterId
+ StepConfig

  

## Cached data access control<a name="pipelines-access-control"></a>

When a SageMaker pipeline runs, it caches the parameters and metadata associated with the SageMaker jobs launched by the pipeline and saves them for reuse in subsequent runs\. This metadata is accessible through a variety of sources in addition to cached pipeline steps, and includes the following types:
+ `Describe*Job` requests
+ CloudWatch Logs
+ CloudWatch Events
+ CloudWatch Metrics
+ SageMaker Search

Note that access to each data source in the list is controlled by its own set of IAM permissions\. Removing a particular role’s access to one data source does not affect the level of access to the others\. For example, an account admin might remove IAM permissions for `Describe*Job` requests from a caller’s role\. While the caller can no longer make `Describe*Job` requests, they can still retrieve the metadata from a pipeline run with cached steps as long as they have permission to run the pipeline\. If an account admin wants to remove access to the metadata from a particular SageMaker job completely, they need to remove permissions for each of the relevant services that provide access to the data\. 