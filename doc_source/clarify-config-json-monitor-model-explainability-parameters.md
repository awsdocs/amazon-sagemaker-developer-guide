# Configure Parameters to Monitor Attribution Drift<a name="clarify-config-json-monitor-model-explainability-parameters"></a>

Amazon SageMaker Clarify explainability monitor reuses a subset of the parameters used in the analysis configuration of [Configure the Analysis](clarify-processing-job-configure-analysis.md)\. The following parameters must be provided in a JSON file and the path must be provided in the `ConfigUri` parameter of [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ModelExplainabilityAppSpecification](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ModelExplainabilityAppSpecification)\.
+ `"version"` – \(Optional\) Schema version of the configuration file\. If not provided, the latest supported version is used\.
+ `"headers"` – \(Optional\) A list of feature names in the dataset\. Explainability analysis does not require labels\. 
+ `"methods"` – A list of methods and their parameters for the analyses and reports\. If any section is omitted, then it is not computed\.
  + `"shap"` – \(Optional\) Section on SHAP value computation\.
    + `"baseline"` – \(Optional\) A list of rows \(at least one\), or an Amazon Simple Storage Service Amazon S3 object URI\. To be used as the baseline dataset \(also known as a background dataset\) in the Kernel SHAP algorithm\. The format should be the same as the dataset format\. Each row should contain only the feature columns \(or values\)\. Before you send each row to the model, omit any column that must be excluded\.
    + `"num_samples"` – Number of samples to be used in the Kernel SHAP algorithm\. This number determines the size of the generated synthetic dataset to compute the SHAP values\. If not provided, then a SageMaker Clarify job chooses the value based on a count of features\.
    + `"agg_method"` – Aggregation method for global SHAP values\. Valid values are as follows:
      + `"mean_abs"` – Mean of absolute SHAP values for all instances\.
      + `"median"` – Median of SHAP values for all instances\.
      + `"mean_sq"` – Mean of squared SHAP values for all instances\.
    + `"use_logit"` – \(Optional\) Boolean value to indicate if the logit function is to be applied to the model predictions\. If `"use_logit"` is `true`, then the SHAP values have log\-odds units\. The default value is `false`\. 
    + `"save_local_shap_values"` – \(Optional\) Boolean value to indicate if local SHAP values are to be saved in the output location\. Use `true` to save them\. Use `false` to not save them\. The default is `false`\.
+ `"predictor"` – \(Optional\) Section on model parameters, required if `"shap"` and `"post_training_bias"` sections are present\.
  + `"model_name"` – Model name created by `CreateModel` API, with container mode as `SingleModel`\.
  + `"instance_type"` – Instance type for the shadow endpoint\.
  + `"initial_instance_count"` – Instance count for the shadow endpoint\.
  + `"content_type"` – \(Optional\) The model input format to be used for getting inferences with the shadow endpoint\. Valid values are `"text/csv"` for CSV, `"application/jsonlines"` for JSON Lines, `application/x-parquet` for Apache Parquet, and `application/x-image` to enable Computer Vision explainability\. The default value is the same as the `dataset_type` format\.
  + `"accept_type"` – \(Optional\) The model *output* format to be used for getting inferences with the shadow endpoint\. Valid values are `"text/csv"` for CSV, `"application/jsonlines"` for JSON Lines\. If omitted, SageMaker Clarify uses the response data type of the captured data\.
  + `"content_template"` – \(Optional\) A template string used to construct the model input from dataset instances\. It is only used when `"content_type"` is `"application/jsonlines"`\. The template should have only one placeholder, `$features`, which is replaced by the features list at runtime\. For example, given `"content_template":"{\"myfeatures\":$features}"`, if an instance \(no label\) is `1,2,3`, then model input becomes JSON Lines `'{"myfeatures":[1,2,3]}'`\.
  + `"label_headers"` – \(Optional\) A list of values that the `"label"` takes in the dataset\. Associates the scores returned by the model endpoint with their corresponding label values\. If it is provided, then the analysis report uses the headers instead of placeholders like `“label0”`\.

The other parameters should be provided in `EndpointInput` of the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ModelExplainabilityJobInput](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ModelExplainabilityJobInput) API\.
+ `FeaturesAttribute` – This parameter is required if endpoint input data format is `"application/jsonlines"`\. It is the JSONPath used to locate the feature columns if the dataset format is JSON Lines\.
+ `ProbabilityAttribute` – Index or JSONPath location in the model output for probabilities\. If the model output is JSON Lines with a list of labels and probabilities, for example, then the label that corresponds to the maximum probability is selected for bias computations\.

## Example JSON Configuration Files for CSV and JSON Lines Datasets<a name="clarify-config-json-monitor-model-explainability-parameters-examples"></a>

Here are examples of the JSON files used to configure CSV and JSON Lines datasets to monitor them for feature attribution drift\.

**Topics**
+ [CSV Datasets](#clarify-config-json-monitor-model-explainability-parameters-example-csv)
+ [JSON Lines Datasets](#clarify-config-json-monitor-model-explainability-parameters-example-jsonlines)

### CSV Datasets<a name="clarify-config-json-monitor-model-explainability-parameters-example-csv"></a>

Consider a dataset that has three numerical feature columns, as in the following example\.

```
0.5814568701544718, 0.6651538910132964, 0.3138080342665499
0.6711642728531724, 0.7466687034026017, 0.1215477472819713
0.0453256543003371, 0.6377430803264152, 0.3558625219713576
0.4785191813363956, 0.0265841045263860, 0.0376935084990697
```

Assume that the model output has two columns, where the first one is the predicted label and the second one is the probability, as in the following example\.

```
1, 0.5385257417814224
```

The following example JSON configuration file shows how this CSV dataset can be configured\.

```
{
                    
    "headers": [
        "feature_1",
        "feature_2",
        "feature_3"
    ],
    "methods": {
        "shap": {
            "baseline": [
                [0.4441164946610942, 0.5190374448171748, 0.20722795300473712]
            ],
            "num_samples": 100,
            "agg_method": "mean_abs"
        }
    },
    "predictor": {
        "model_name": "my_model",
        "instance_type": "ml.m5.xlarge",
        "initial_instance_count": 1
    }
}
```

The predicted label is selected by the `"ProbabilityAttribute"` parameter\. Zero\-based numbering is used, so 1 indicates the second column of the model output\.

```
"EndpointInput": {
    ...
    "ProbabilityAttribute": 1
    ...
}
```

### JSON Lines Datasets<a name="clarify-config-json-monitor-model-explainability-parameters-example-jsonlines"></a>

Consider a dataset that has four feature columns and one label column, where the first feature and the label are binary, as in the following example\.

```
{"features":[0, 0.5814568701544718, 0.6651538910132964, 0.3138080342665499], "label":0}
{"features":[1, 0.6711642728531724, 0.7466687034026017, 0.1215477472819713], "label":1}
{"features":[0, 0.0453256543003371, 0.6377430803264152, 0.3558625219713576], "label":1}
{"features":[1, 0.4785191813363956, 0.0265841045263860, 0.0376935084990697], "label":1}
```

The model input is the same as the dataset format, and the model output are JSON Lines, as in the following example\.

```
{"predicted_label":1, "probability":0.5385257417814224}
```

In the following example, the JSON configuration file shows how this JSON Lines dataset can be configured\.

```
{
    "headers": [
        "feature_1",
        "feature_2",
        "feature_3"
    ],
    "methods": {
        "shap": {
            "baseline": [
                {"features":[0.4441164946610942, 0.5190374448171748, 0.20722795300473712]}
            ],
            "num_samples": 100,
            "agg_method": "mean_abs"
        }
    },
    "predictor": {
        "model_name": "my_model",
        "instance_type": "ml.m5.xlarge",
        "initial_instance_count": 1,
        "content_template":"{\"features\":$features}"
    }
}
```

Then the `"features"` parameter value in `EndpointInput` is used to locate the features in the dataset, and the `"probability"` parameter value selects the probability value from model output\.

```
"EndpointInput": {
    ...
    "FeaturesAttribute": "features",
    "ProbabilityAttribute": "probability",
    ...
}
```