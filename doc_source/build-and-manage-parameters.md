# Pipeline Parameters<a name="build-and-manage-parameters"></a>

You can introduce variables into your pipeline definition using parameters\. You can reference parameters that you define throughout your pipeline definition\. Parameters have a default value, which you can override by specifying parameter values when starting a pipeline execution\. The default value must be an instance matching the parameter type\. All parameters used in step definitions must be defined in your pipeline definition\. Amazon SageMaker Model Building Pipelines supports the following parameter types: 
+  `ParameterString` – Representing a string parameter\. 
+  `ParameterInteger` – Representing an integer parameter\. 
+  `ParameterFloat` – Representing a float parameter\.
+  `ParameterBoolean` – Representing a Boolean Python type\.

Parameters take the following format:

```
<parameter> = <parameter_type>(
    name="<parameter_name>",
    default_value=<default_value>
)
```

The following example shows a sample parameter implementation\.

```
from sagemaker.workflow.parameters import (
    ParameterInteger,
    ParameterString,
    ParameterFloat,
    ParameterBoolean
)

processing_instance_count = ParameterInteger(
    name="ProcessingInstanceCount",
    default_value=1
)
```

You pass the parameter when creating your pipeline as shown in the following example\.

```
pipeline = Pipeline(
    name=pipeline_name,
    parameters=[
        processing_instance_count
    ],
    steps=[step_process]
)
```

You can also pass a parameter value that differs from the default value to a pipeline execution, as shown in the following example\.

```
execution = pipeline.start(
    parameters=dict(
        ProcessingInstanceType="ml.c5.xlarge",
        ModelApprovalStatus="Approved"
    )
)
```

You can manipulate parameters with SageMaker Python SDK functions like `[ sagemaker\.workflow\.functions\.Join](https://sagemaker.readthedocs.io/en/stable/workflows/pipelines/sagemaker.workflow.pipelines.html#sagemaker.workflow.functions.Join)`\. For more information on parameters, see [ SageMaker Pipelines Parameters](https://sagemaker.readthedocs.io/en/stable/workflows/pipelines/sagemaker.workflow.pipelines.html#parameters)\.