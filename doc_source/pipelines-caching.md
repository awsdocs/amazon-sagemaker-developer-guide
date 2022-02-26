# Caching Pipeline Steps<a name="pipelines-caching"></a>

When you use step signature caching, before SageMaker Pipelines executes a step, it attempts to find a previous execution of a step that was called with the same arguments\. SageMaker Pipelines checks that the call signatures are identical\. Pipelines doesn't check whether the data or code to which the arguments point has changed\. If Pipelines finds a previous execution, it creates a cache hit\. Pipelines then propagates the values from the cache hit during execution, rather than recomputing the step\.

Step caching only considers successful executions, so it never reuses failed executions\. When multiple successful executions exist within the timeout period, Pipelines uses the result for the most recent successful execution\. If no successful executions match in the timeout period, Pipelines doesn't reuse any steps\. If the executor finds a cache hit for a previous step execution that is still in progress, both steps continue executing and update the cache, if they're successful\.

You must opt in to step caching, otherwise it is off by default\. When you enable step caching, you must also define a timeout\. This timeout defines how old a previous execution can be to be considered for reuse\.

Step caching is only scoped for individual pipelines, so you can’t reuse a step from another pipeline\. Even if there is a step signature match in the other pipeline, the step is not reused\.

Step caching is available for the following step types: 
+ Training 
+ Processing 
+ Transform 

## Enabling Step Caching<a name="pipelines-caching-enabling"></a>

To enable step caching, you must add a `CacheConfig` property to the step definition\.

`CacheConfig` properties use the following format in the pipeline definition file\.

```
{
    "CacheConfig": {
        "Enabled": false,
        "ExpireAfter": "<time>"
    }
}
```

The `Enabled` field may be true or false\. `ExpireAfter` is a string that defines the timeout period\. Any ISO 8601 duration string is a valid `ExpireAfter` value\. The `ExpireAfter` duration can contain a year, month, week, day, hour, and minute value\. Each value is consists of a number followed by a letter indicating the duration unit it is for\. For example:
+ "30d" = Thirty days
+ "5y" = Five years
+ "T16m" = 16 minutes
+ "30dT5h" = 30 days and five hours\.

The following example shows how to enable caching for a training step using the Amazon SageMaker Python SDK\.

```
from sagemaker.workflow.steps import CacheConfig
      
cache_config = CacheConfig(enable_caching=True, expire_after="PT1H")

step_train = TrainingStep(
    name="TrainAbaloneModel",
    estimator=xgb_train,
    inputs=inputs,
    cache_config=cache_config
)
```