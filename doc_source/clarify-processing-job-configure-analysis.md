# Configure the Analysis<a name="clarify-processing-job-configure-analysis"></a>

The inputs for the analysis are configured by the parameters of the `ProcessingInput` API\. The `"analysis_config"` value of the `input_name` specifies the JSON file named "analysis\_config\.json" that contains the configuration values\. The path to the JSON file is provided in the `source` parameter of `ProcessingInput`\. Examples of analysis configuration JSON files are provided for CSV and JSONLInes datasets following the parameter descriptions\. 

**Topics**
+ [Example Analysis Configuration JSON File for a CSV Dataset](#clarify-analysis-configure-csv-example)
+ [Example Analysis Configuration JSON File for a JSONLines Dataset](#clarify-analysis-configure-JSONLines-example)

In the JSON configuration file, you can specify the following parameters\.
+ `"version"` – \(Optional\) Schema version of the configuration file\. If not provided, the latest supported version is used\.
+ `"dataset_type"` – Format of the dataset\. Valid values are `"text/csv"` for CSV, `"application/jsonlines"` for JSON Lines, `application/x-parquet` for Apache Parquet, and `application/x-image` to enable Computer Vision explainability\.
+ `"dataset_uri"` – \(Optional\) Dataset S3 prefix/object URI \(if not given as `ProcessingInput`\)\. If it is a prefix, the processing job recursively collects all S3 files under the prefix\.
+ `"headers"` – \(Optional\) A list of column names in the dataset\. If the `dataset_type` is `"application/jsonlines"` and `"label"` is specified, then the last header shall be the header of label column\. 
+ `"label"` – \(Optional\) Target attribute for the model to be used for *bias metrics*\. Specified as a column name, or as index if the dataset format is CSV, or as JSONPath if the dataset format is JSONLines\. 
+ `"probability_threshold"` – \(Optional\) A float value to indicate the threshold to select the binary label in the case of binary classification\. The default value is 0\.5\.
+ `"features"` – \(Optional\) JSONPath for locating the feature columns for bias metrics if the dataset format is JSONLines\.
+ `"label_values_or_threshold"` – \(Optional\) List of label values or threshold to indicate positive outcome used for bias metrics\.
+ `"facet"` – \(Optional\) A list of features that are sensitive attributes, referred to as facets\. Facets are used for *bias metrics* in the form of pairs, and include the following:
  + `"name_or_index"` – Facet column name or index\.
  + `"value_or_threshold"` – \(Optional\) List of values or threshold that the facet column can take\. Indicates the sensitive group, such as the group that is used to measure bias against\. If not provided, bias metrics are computed as one group for every unique value \(rather than all values\)\. If the facet column is numeric, this threshold value is applied as the lower bound to select the sensitive group\.
+ `"group_variable"` – \(Optional\) A column name or index to indicate the group variable to be used for the *bias metric* *Conditional Demographic Disparity\.*
+ `"joinsource_name_or_index"` – \(Optional\) The name or index of the column in the dataset that acts as an identifier column, for example, while performing a join\. This column is only used as an identifier, and not used for any other computations\. This is an optional field in all cases except when the dataset contains more than one file, and `"save_local_shap_values"` is set to `true`\.
+ `"methods"` – A list of methods and their parameters for the analyses and reports\. If any section is omitted, then it is not computed\.
  + `"pre_training_bias"` – \(Optional\) Section on pretraining bias metrics\.
    + `"methods"` – A list of pretraining metrics to be computed\.
  + `"post_training_bias"` – \(Optional\) Section on posttraining bias metrics\.
    + `"methods"` – A list of posttraining metrics to be computed\.
  + `"shap"` – \(Optional\) Section on SHAP value computation\.
    + `"agg_method"` – Aggregation method for global SHAP values\. Valid values are as follows:
      + `"mean_abs"` – Mean of absolute SHAP values for all instances\.
      + `"median"` – Median of SHAP values for all instances\.
      + `"mean_sq"` – Mean of squared SHAP values for all instances\.
    + `"baseline"` – \(Optional\) A list of rows \(at least one\), or an S3 object URI, to be used as the baseline \(background\) dataset in the Kernel SHAP algorithm\. The format should be the same as the dataset format\. Each row should contain only the feature columns/values, and it should omit the label column/values\.
    + `"num_clusters"` – \(Optional\) If `"baseline"` is not provided, Clarify attempts to compute a baseline by clustering the dataset\. The `"num_clusters"` determines the number of clusters that the dataset is clustered into\. Each cluster contributes to a baseline, so the number of clusters directly affects the run time of SHAP explanations\.
    + `"num_samples"` – Number of samples to be used in the Kernel SHAP algorithm\. This number determines the size of the generated synthetic dataset to compute the SHAP values\.
    + `"seed"` – \(Optional\) Seed value for the random number generator to obtain a deterministic SHAP result\.
    + `"use_logit"` – \(Optional\) Boolean value to indicate if the logit function is to be applied to the model predictions\. If `"use_logit"` is `true`, then the SHAP values have log\-odds units\. The default value is `false`\. 
    + `"save_local_shap_values"` – \(Optional\) Boolean value to indicate if local SHAP values are to be saved in the output location\. Use `true` to save them; `false` to not save them\. The default is `false`\.
+ `"pdp"` – \(Optional\) Section for configuring partial dependence plots \(PDP\)\. Shows the dependence of the target response on a set of input features of interest, marginalizing over the values of all other input features\. 
  + `"features": ["pdp_feature_1", "pdp_feature_2"…] `– \(Optional\) The list of feature names or indices for which partial dependence plots are to be computed and plotted\. If SHAP is not requested, the features must be provided\.
  + `"grid_resolution" ` – \(Required for numerical columns only\.\) In case of numerical features, this number represents that number of buckets into which the range of numerical values is divided\. This specifies the granularity of the grid for the PDP plot\.
  + `"top_k_features" ` – \(Optional\) If the `features` parameter is not provided, and `shap` is provided, Clarify chooses the top `k` features based on SHAP attributions\. You can set this value to specify how many of the top features must be used for PDP plots\. The default is 10\.
+ `"predictor"` – \(Optional\) Section on model parameters, required if `"shap"` and `"post_training_bias"` sections are present\.
  + `"model_name"` – Model name \(as created by `CreateModel` API with container mode as `SingleModel`\.
  + `"instance_type"` – Instance type for the shadow endpoint\.
  + `"initial_instance_count"` – Instance count for the shadow endpoint\.
  + `"content_type"` – \(Optional\) The model input format to be used for getting inferences with the shadow endpoint\. Valid values are `"text/csv"` for CSV, `"application/jsonlines"` for JSON Lines, `application/x-parquet` for Apache Parquet, and `application/x-image` to enable Computer Vision explainability\. The default value is the same as the `dataset_type` format\.
  + `"accept_type"` – \(Optional\) The model *output* format to be used for getting inferences with the shadow endpoint\. Valid values are `"text/csv"` for CSV, `"application/jsonlines"` for JSON Lines, `application/x-parquet` for Apache Parquet, and `application/x-image` to enable Computer Vision explainability\. The default value is the same as `content_type`\.
  + `"accelerator_type"` – \(Optional\) The type of Elastic Inference \(EI\) accelerator to attach to the instance, for example, ml\.eia1\.medium\. For more information, see [Use Amazon SageMaker Elastic Inference \(EI\) ](ei.md)\.
  + `"custom_attributes"` – \(Optional\) Provides additional information about a request for an inference that is submitted to a model hosted on an Amazon SageMaker endpoint\. The information is an opaque value that is forwarded verbatim\. You can use this value, for example, to provide an ID that you can use to track a request\. You can also use this value to provide metadata that a service endpoint was programmed to process\. The value must consist of no more than 1024 visible US\-ASCII characters, as specified in [Section 3\.3\.6\. Field Value Components](http://tools.ietf.org/html/rfc7230#section-3.2.6) of the Hypertext Transfer Protocol \(HTTP/1\.1\)\.
  + `"label"` – \(Optional\) Index or JSONPath location in the model output for the target attribute that is used by the bias metrics\. If the `label` is not provided in the CSV `accept_type` case, then Clarify assumes that the model output is a single numeric value corresponding to the score or probability\. 
  + `"probability"` – \(Optional\) Index or JSONPath location in the model output for probabilities or scores to be used for explainability\. For example, if the model output is JSONLines with a list of labels and probabilities, then for bias methods, the label that corresponds to the maximum probability is selected for bias computations\. For explainability method, currently all probabilities are explained\.
  + `"endpoint_name-prefix"` – \(Optional\) Provides a custom prefix to the name of the temporary endpoint\.
  + `"target_model"` – \(Optional\) Sets the target model name when using a multi\-model endpoint\. For more information about multi\-model endpoints, see [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_runtime_InvokeEndpoint_RequestSyntax](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_runtime_InvokeEndpoint_RequestSyntax)\.
  + `"label_headers"` – \(Optional\) A list of values that the `"label"` takes in the dataset\. Associates the scores returned by the model endpoint with their corresponding label values\. It is used to extract the label value with the highest score as predicted label\.
  + `"content_template"` – \(Optional\) A template string to be used to construct the model input from dataset instances\. It is only used when `"content_type"` is `"application/jsonlines"`\. The template should have only one placeholder, `$features`, which is replaced by the features list at runtime\. For example, given `"content_template":"{\"myfeatures\":$features}"`, if an instance \(no label\) is `1,2,3`, then model input becomes JSONline `'{"myfeatures":[1,2,3]}'`\.
+ `"report"` – \(Optional\) Section on report parameters\. A report is generated if this section is present\.
  + `"name"` – \(Optional\) Filename prefix for the report notebook and PDF file\. The default name is `"report"`\. 
  + `"title"` – \(Optional\) Title string for the report notebook and PDF file\. The default title is `"SageMaker Analysis Report"`\.

## Example Analysis Configuration JSON File for a CSV Dataset<a name="clarify-analysis-configure-csv-example"></a>

```
{
    "dataset_type": "text/csv",
    "headers": [
        "feature_0",
        "feature_1",
        "feature_2",
        "feature_3",
        "target"
    ],
    "label": "target",
    "label_values_or_threshold": [1],
    "probability_threshold" : 0.7,
    "facet": [
        {
         "name_or_index" : "feature_1",
         "value_or_threshold": [1]
        },
        {
         "name_or_index" : "feature_2",
         "value_or_threshold": [0.7]
        }
    ],
    "group_variable": "feature_3",
    "joinsource_name_or_index":'column_name',
    "methods": {
        "shap": {
            "baseline": [
                [
                    "yes",
                    3,
                    0.9,
                    1
                ]
            ],
            "num_samples": 1000,
            "agg_method": "mean",
            "use_logit": true,
            "save_local_shap_values": true,
            "num_clusters": 5,
            "seed": 1234
        },
        "pre_training_bias": {
            "methods": "all"
        },
        "post_training_bias": {
            "methods": "all"
        },
        "report": {
            "name": "report",
            "title": "Analysis Report"
        }
    },
    "predictor": {
        "model_name": "my_model",
        "instance_type": "ml.m5.xlarge",
        "initial_instance_count": 1,
        "content_type": "text/csv",
        "accept_type": "text/csv",
        "accelerator_type":"ml.eia1.medium",
        "custom_attributes": "c000b4f9-df62-4c85-a0bf-7c525f9104a4",
        "label": 0,
        "probability": 1,
        "endpoint_name_prefix": "myendpointprefix",
        "target_model": ""
    }
}
```

Model output as CSV is as follows:

```
Current,"[0.028986845165491104, 0.8253824710845947, 0.028993206098675728, 0.02898673340678215, 0.029557107016444206, 0.0290389321744442, 0.02905467338860035]"
```

Corresponding predictor configuration is as follows:

```
    "predictor": {
        ...,
        "accept_type":  "text/csv",
        "label": 0,
        "probability": 1,
        ...
    }
```

## Example Analysis Configuration JSON File for a JSONLines Dataset<a name="clarify-analysis-configure-JSONLines-example"></a>

```
{
    "dataset_type": "application/jsonlines",
    "dataset_uri": "s3://my_bucket/my_folder/dataset.jsonl",
    "headers": ["Label", "Feature1", "Feature2"],
    "label": "data.label",
    "features": "data.features.values",
    "facet":[
        {
            "name_or_index": "Feature1",
            "value_or_threshold": [1,5]
        },
        {
            "name_or_index": "Feature2",
            "value_or_threshold": [2,6]
        }
    ],
    "methods": {
        "shap": {
            "baseline": [
                {"data":{"features":{"values":[9,10]},"label":0}},
                {"data":{"features":{"values":[11,12]},"label":1}}
            ]
        }
    },
    "predictor": {
        "model_name": "my_jsonl_model",
        "instance_type": "ml.m5.xlarge",
        "initial_instance_count": 1
    }
}
```

Dataset as S3 prefix is as follows:

```
"dataset_uri": "s3://my_bucket/my_folder"
```

Dataset as S3 object is as follows:

```
"dataset_uri": "s3://my_bucket/my_folder/train.csv"
```

Baseline as S3 object is as follows:

```
"baseline": "s3://my_bucket/my_folder/baseline.csv"
```

Model output as JSONLines is as follows:

```
{"predicted_label": "Current", "score": "[0.028986845165491104, 0.8253824710845947, 0.028993206098675728, 0.02898673340678215, 0.029557107016444206, 0.0290389321744442, 0.02905467338860035]"}
```

Corresponding predictor configuration is as follows:

```
    "predictor": {
        ...,
        "accept_type":  "application/jsonlines",
        "label": "predicted_label",
        "probability": "score",
        ...
    }
```