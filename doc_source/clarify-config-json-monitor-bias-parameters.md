# Configure Parameters to Monitor Bias Drift<a name="clarify-config-json-monitor-bias-parameters"></a>

Amazon SageMaker Clarify bias monitoring reuses a subset of the parameters used in the analysis configuration of [Configure the Analysis](clarify-processing-job-configure-analysis.md)\. After describing the configuration parameters, this topic provides examples of JSON files\. These files are used to configure CSV and JSON Lines datasets to monitor them for bias drift when machine learning models are in production\.

The following parameters must be provided in a JSON file\. The path to this JSON file must be provided in the `ConfigUri` parameter of the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ModelBiasAppSpecification](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ModelBiasAppSpecification) API\.
+ `"version"` – \(Optional\) Schema version of the configuration file\. If not provided, the latest supported version is used\.
+ `"headers"` – \(Optional\) A list of column names in the dataset\. If the `dataset_type` is `"application/jsonlines"` and `"label"` is specified, then the last header becomes the header of the label column\. 
+ `"label"` – \(Optional\) Target attribute for the model to be used for *bias metrics*\. Specified either as a column name, or an index \(if dataset format is CSV\), or as a JSONPath \(if dataset format is JSON Lines\)\.
+ `"label_values_or_threshold"` – \(Optional\) List of label values or threshold\. Indicates positive outcome used for bias metrics\.
+ `"facet"` – \(Optional\) A list of features that are sensitive attributes, referred to as facets\. Facets are used for *bias metrics* in the form of pairs, and include the following:
  + `"name_or_index"` – Facet column name or index\.
  + `"value_or_threshold"` – \(Optional\) List of values or threshold that the facet column can take\. Indicates the sensitive group, such as the group that is used to measure bias against\. If not provided, bias metrics are computed as one group for every unique value \(rather than all values\)\. If the facet column is numeric, this threshold value is applied as the lower bound to select the sensitive group\.
+ `"group_variable"` – \(Optional\) A column name or index to indicate the group variable to be used for the *bias metric* *Conditional Demographic Disparity\.*

The other parameters should be provided in `EndpointInput` of the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ModelBiasJobInput](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ModelBiasJobInput) API\.
+ `FeaturesAttribute` – This parameter is required if endpoint input data format is `"application/jsonlines"`\. It is the JSONPath used to locate the feature columns if the dataset format is JSON Lines\.
+ `InferenceAttribute` – Index or JSONPath location in the model output for the target attribute to be used for monitored for bias using bias metrics\. If it is not provided in the CSV `accept_type` case, then it is assumed that the model output is a single numeric value corresponding to a score or probability\.
+ `ProbabilityAttribute` – Index or JSONPath location in the model output for probabilities\. If the model output is JSON Lines with a list of labels and probabilities, for example, then the label that corresponds to the maximum probability is selected for bias computations\.
+ `ProbabilityThresholdAttribute` – \(Optional\) A float value to indicate the threshold to select the binary label, in the case of binary classification\. The default value is 0\.5\.

## Example JSON Configuration Files for CSV and JSON Lines Datasets<a name="clarify-config-json-monitor-bias-parameters-examples"></a>

Here are examples of the JSON files used to configure CSV and JSON Lines datasets to monitor them for bias drift\.

**Topics**
+ [CSV Datasets](#clarify-config-json-monitor-bias-parameters-example-csv)
+ [JSON Lines Datasets](#clarify-config-json-monitor-bias-parameters-example-jsonlines)

### CSV Datasets<a name="clarify-config-json-monitor-bias-parameters-example-csv"></a>

Consider a dataset that has four feature columns and one label column, where the first feature and the label are binary, as in the following example\.

```
0, 0.5814568701544718, 0.6651538910132964, 0.3138080342665499, 0
1, 0.6711642728531724, 0.7466687034026017, 0.1215477472819713, 1
0, 0.0453256543003371, 0.6377430803264152, 0.3558625219713576, 1
1, 0.4785191813363956, 0.0265841045263860, 0.0376935084990697, 1
```

Assume that the model output has two columns, where the first one is the predicted label and the second one is the probability, as in the following example\.

```
1, 0.5385257417814224
```

Then the following JSON configuration file shows an example of how this CSV dataset can be configured\.

```
{
    "headers": [
        "feature_0",
        "feature_1",
        "feature_2",
        "feature_3",
        "target"
    ],
    "label": "target",
    "label_values_or_threshold": [1],
    "facet": [{
        "name_or_index": "feature_1",
        "value_or_threshold": [1]
    }]
}
```

The predicted label is selected by the `"InferenceAttribute"` parameter\. Zero\-based numbering is used, so 0 indicates the first column of the model output,

```
"EndpointInput": {
    ...
    "InferenceAttribute": 0
    ...
}
```

Alternatively, you can use different parameters to convert probability values to binary predicted labels\. Zero\-based numbering is used: 1 indicates the second column; the `ProbabilityThresholdAttribute` value of 0\.6 indicates that a probability greater than 0\.6 predicts the binary label as 1\.

```
"EndpointInput": {
    ...
    "ProbabilityAttribute": 1,
    "ProbabilityThresholdAttribute": 0.6
    ...
}
```

### JSON Lines Datasets<a name="clarify-config-json-monitor-bias-parameters-example-jsonlines"></a>

Consider a dataset that has four feature columns and one label column, where the first feature and the label are binary, as in the following example\.

```
{"features":[0, 0.5814568701544718, 0.6651538910132964, 0.3138080342665499], "label":0}
{"features":[1, 0.6711642728531724, 0.7466687034026017, 0.1215477472819713], "label":1}
{"features":[0, 0.0453256543003371, 0.6377430803264152, 0.3558625219713576], "label":1}
{"features":[1, 0.4785191813363956, 0.0265841045263860, 0.0376935084990697], "label":1}
```

Assume that the model output has two columns, where the first is a predicted label and the second is a probability\.

```
{"predicted_label":1, "probability":0.5385257417814224}
```

The following JSON configuration file shows an example of how this JSON Lines dataset can be configured\.

```
{
    "headers": [
        "feature_0",
        "feature_1",
        "feature_2",
        "feature_3",
        "target"
    ],
    "label": "label",
    "label_values_or_threshold": [1],
    "facet": [{
        "name_or_index": "feature_1",
        "value_or_threshold": [1]
    }]
}
```

Then, the `"features"` parameter value in `EndpointInput` is used to locate the features in the dataset, and the `"predicted_label"` parameter value selects the predicted label from the model output\. 

```
"EndpointInput": {
    ...
    "FeaturesAttribute": "features",
    "InferenceAttribute": "predicted_label"
    ...
}
```

Alternatively, you can convert probability values to predicted binary labels using the `ProbabilityThresholdAttribute` parameter value\. A value of 0\.6, for example, indicates that a probability greater than 0\.6 predicts the binary label as 1\.

```
"EndpointInput": {
    ...
    "FeaturesAttribute": "features",
    "ProbabilityAttribute": "probability",
    "ProbabilityThresholdAttribute": 0.6
    ...
}
```