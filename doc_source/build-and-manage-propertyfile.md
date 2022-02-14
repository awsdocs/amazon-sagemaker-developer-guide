# Property Files and `JsonGet`<a name="build-and-manage-propertyfile"></a>

You use property files to store information from the output of a processing step\. This is particularly useful when analyzing the results of a processing step to decide how a conditional step should be executed\. The `JsonGet` function processes a property file and enables you to use JsonPath notation to query the property JSON file\. For more information on JsonPath notation, see the [JsonPath repo](https://github.com/json-path/JsonPath)\.

To store a property file for later use, you must first create a `PropertyFile` instance with the following format\. The `path` parameter is the name of the JSON file to which the property file is saved\. `output_name` must match the `output_name` of the `ProcessingOutput` that you define in your processing step\. This enables the property file to capture the `ProcessingOutput` in the step\.

```
from sagemaker.workflow.properties import PropertyFile

<property_file_instance> = PropertyFile(
    name="<property_file_name>",
    output_name="<processingoutput_output_name>",
    path="<path_to_json_file>"
)
```

When you create your `ProcessingStep` instance, add the `property_files` parameter to list all of the parameter files that the Amazon SageMaker Model Building Pipelines service must index\. This saves the property file for later use\.

```
property_files=[<property_file_instance>]
```

To use your property file in a condition step, add the `property_file` to the condition that you pass to your condition step as shown in the following example to query the JSON file for your desired property using the `json_path` parameter\.

```
cond_lte = ConditionLessThanOrEqualTo(
    left=JsonGet(
        step_name=step_eval.name,
        property_file=<property_file_instance>,
        json_path="mse"
    ),
    right=6.0
)
```