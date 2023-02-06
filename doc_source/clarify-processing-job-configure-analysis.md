# Configure the Analysis<a name="clarify-processing-job-configure-analysis"></a>

The inputs for the analysis are configured by the parameters of the `ProcessingInput` API\. The `"analysis_config"` value of the `input_name` specifies the JSON file named **analysis\_config\.json** that contains the configuration values\. The path to the JSON file is provided in the `source` parameter of `ProcessingInput`\. Examples of analysis configuration JSON files are provided for JSON files for CSV, JSON Lines, image, and text datasets following the parameter descriptions\. 

**Topics**
+ [Parameters for JSON configuration files](#clarify-processing-job-configure-analysis-parameters)
+ [Example JSON configuration files](#clarify-processing-job-configure-analysis-examples)

## Parameters for JSON configuration files<a name="clarify-processing-job-configure-analysis-parameters"></a>

In the JSON configuration file, you can specify the following parameters\.
+ `"dataset_type"` – \(Required\) Format of the dataset\. Valid values are `"text/csv"` for CSV, `"application/jsonlines"` for JSON Lines, `application/x-parquet` for Apache Parquet, and `application/x-image` to enable computer vision explainability\. For more information see [Common Data Formats for Inference](https://docs.aws.amazon.com/sagemaker/latest/dg/cdf-inference.html)\.
+ `"dataset_uri"` – \(Optional\) Dataset S3 prefix/object URI \(if not given as `ProcessingInput`\)\. If it is a prefix, the processing job recursively collects all S3 files under the prefix\. For computer vision, the URI is required and it can either be a path to the image manifest file, or to the bucket of images to be explained\.
+ `"excluded_columns"` – A list of names or indices of the columns which are to be excluded from making model inference API calls\.
+ `"features"` – \(Optional\) JMESPath for locating the feature columns for bias metrics, if the dataset format is JSON Lines\.
+ `"facet"` – \(Optional\) A list of features that are sensitive attributes, referred to as facets\. Facets are used for *bias metrics* in the form of pairs, and include the following:
  + `"name_or_index"` – Facet column name or index\.
  + `"value_or_threshold"` – \(Optional\) List of values or threshold that the facet column can take\. Indicates the sensitive group, such as the group that is used to measure bias against\. If not provided, bias metrics are computed as one group for every unique value \(rather than all values\)\. If the facet column is numeric, this threshold value is applied as the lower bound to select the sensitive group\.
+ `"group_variable"` – \(Optional\) A column name or index to indicate the group variable to be used for the *bias metric* *Conditional Demographic Disparity\.*
+ `"headers"` – \(Optional\) A list of column names in the dataset\. If the `dataset_type` is `"application/jsonlines"` and `"label"` is specified, then the last header shall be the header of label column\. 
+ `"joinsource_name_or_index"` – \(Optional\) The name or index of the column in the dataset that acts as an identifier column, for example, while performing a join\. This column is only used as an identifier, and not used for any other computations\. This is an optional field in all cases except when the dataset contains more than one file, and `"save_local_shap_values"` is set to `true`\.
+ `"label"` – \(Optional\) Target attribute for the model to be used for *bias metrics*\. Specified as a column name, or as index if the dataset format is CSV, or as JMESPath if the dataset format is JSON Lines\.
+ `"label_values_or_threshold"` – \(Optional\) List of label values or threshold\. Indicates positive outcome used for bias metrics\.
+ `"methods"` – A list of methods and their parameters for the analyses and reports\. If any section is omitted, then it is not computed\.
  + `"pdp"` – \(Optional\) Section for configuring partial dependence plots \(PDP\)\. Shows the dependence of the target response on a set of input features of interest\. Marginalizes over the values of all other input features\. 
    + `"features": ["pdp_feature_1", "pdp_feature_2"…] `– \(Optional\) The list of feature names or indices for which partial dependence plots are to be computed and plotted\. If SHAP is not requested, the features must be provided\.
    + `"grid_resolution" ` – \(Required\) Used for numerical features\. Represents that number of buckets into which the range of numerical values is divided\. This specifies the granularity of the grid for the PDP plot\.
    + `"top_k_features" ` – \(Optional\) If the `features` parameter is not provided, and `shap` is provided, Clarify chooses the top `k` features based on SHAP attributions\. You can set this value to specify how many of the top features must be used for PDP plots\. The default is 10\.
  + `"post_training_bias"` – \(Optional\) Section on post\-training bias metrics\.
    + `"methods"` – A list of post\-training metrics to be computed\.
  + `"pre_training_bias"` – \(Optional\) Section on pre\-training bias metrics\.
    + `"methods"` – A list of pre\-training metrics to be computed\.
  + `"shap"` – \(Optional\) Section on SHAP value computation\.
    + `"agg_method"` – Aggregation method for global SHAP values\. Valid values are as follows:
      + `"mean_abs"` – Mean of absolute SHAP values for all instances\.
      + `"mean_sq"` – Mean of squared SHAP values for all instances\.
      + `"median"` – Median of SHAP values for all instances\.
    + `"baseline"` – \(Optional\) A list of rows \(at least one\), or an S3 object URI\. To be used as the baseline dataset \(also known as a background dataset\) in the Kernel SHAP algorithm\. The format should be the same as the dataset format\. Each row should contain only the feature columns \(or values\) and omit any column that must be excluded before being sent to the model\. This includes `label` column, `joinsource` column, and columns included in the `"excluded_columns"` field\.
      + For Computer Vision, a path to the baseline image that is used to mask out features from the input image\. Default: RGB Noise mask\. 
      + For natural language processing \(NLP\) of text columns, the baseline value should be the value used to **replace** the unit of text specified by the `granularity` in the `"text_config"`\. Valid values for the unit of text are `token`, `sentence`, and `paragraph`\.
    + `"num_clusters"` – \(Optional\) If `"baseline"` is not provided, Clarify attempts to compute a baseline by clustering the dataset\. The `"num_clusters"` determines the number of clusters that the dataset is clustered into\. Each cluster contributes to a baseline, so the number of clusters directly affects the runtime of SHAP explanations\.
    + `"num_samples"` – Number of samples to be used in the Kernel SHAP algorithm\. This number determines the size of the generated synthetic dataset to compute the SHAP values\.
    + `"save_local_shap_values"` – \(Optional\) Boolean value to indicate if local SHAP values are to be saved in the output location\. Use `true` to save them; `false` to not save them\. The default is `false`\.
    + `"seed"` – \(Optional\) Seed value for the random number generator to obtain a deterministic SHAP result\.
    + `"text_config"` – \(Required\) The configuration that specifies the natural language processing \(NLP\) features\. If this config is provided, text features are treated as text and explanations are provided for individual units of text\. The unit of text is specified by the `"granularity"`\.
      + `"granularity"` – \(Required\) For NLP, determines the unit of granularity for the analysis of text features\. Valid values are `"token"`, `"sentence"`, or `"paragraph"`\. Each of these units is considered a feature, and SHAP values are computed for the feature specified\.
      + `"language"` – \(Required\) Specifies the language of the text features for natural language processing \(NLP\)\. Valid value are `"chinese"`, `"danish"`, `"dutch"`, `"english"`, `"french"`, `"german"`, `"greek"`, `"italian"`, `"japanese"`, `"lithuanian"`, `"multi-language"`, `"norwegian bokmål"`, `"polish"`, `"portuguese"`, `"romanian"`, `"russian"`, `"spanish"`, `"afrikaans"`, `"albanian"`, `"arabic"`, `"armenian"`, `"basque"`, `"bengali"`, `"bulgarian"`, `"catalan"`, `"croatian"`, `"czech"`, `"estonian"`, `"finnish"`, `"gujarati"`, `"hebrew"`, `"hindi"`, `"hungarian"`, `"icelandic"`, `"indonesian"`, `"irish"`, `"kannada"`, `"kyrgyz"`, `"latvian"`, `"ligurian"`, `"luxembourgish"`, `"macedonian"`, `"malayalam"`, `"marathi"`, `"nepali"`, `"persian"`, `"sanskrit"`, `"serbian"`, `"setswana"`, `"sinhala"`, `"slovak"`, `"slovenian"`, `"swedish"`, `"tagalog"`, `"tamil"`, `"tatar"`, `"telugu"`, `"thai"`, `"turkish"`, `"ukrainian"`, `"urdu"`, `"vietnamese"`, `"yoruba"`\. Use `"multi-language" `for a mix of multiple languages\.
      + `"image_config"` – \(Optional\) Section for configuring SHAP configuration parameters for computer vision explainability\. 
        + `"context"` – \(Optional\) Masks the area around the bounding box of the detected object when running SHAP\. Valid values are 0 to mask everything, or 1 to mask nothing\. The default value is 1\.
        + `"iou_threshold"` – \(Optional\) Minimum intersection over union \(IOU\) metric for evaluating predictions against the original detections\. Used because detection boxes will shift during masking\. The default value is 0\.5\.
        + `"max_objects"` – \(Optional\) Used to filter objects detected by the computer vision model by top confidence score\. The default is 3\.
        + `"model_type"` – Either an object detection model or an image classification model\.
        + `"num_segments"` – \(Optional\) Determines the approximate number of segments to be labeled in the input image\. The default is 20\.
        + `"segment_compactness"` – \(Optional\) Determines the shape and size of the image segments generated by SKLearn’s slic method\. For more details, see [scikit\-image slic](https://scikit-image.org/docs/dev/api/skimage.segmentation.html?highlight=slic#skimage.segmentation.slic) documentation\. The default is 5\.
    + `"use_logit"` – \(Optional\) Boolean value to indicate if the logit function is to be applied to the model predictions\. If `"use_logit"` is `true`, then the SHAP values have log\-odds units\. The default value is `false`\. 
+ `"predictor"` – \(Optional\) Section on model parameters, required if `"shap"` and `"post_training_bias"` sections are present\.
  + `"accept_type"` – \(Optional\) The model *output* format to be used for getting inferences with the shadow endpoint\. Valid values are `"text/csv"` for CSV, `"application/jsonlines"` for JSON Lines, `application/x-parquet` for Apache Parquet, and `application/x-image` to enable Computer Vision explainability\. The default value is the same as `content_type`\.
  + `"accelerator_type"` – \(Optional\) The type of Elastic Inference \(EI\) accelerator to attach to the instance\. Example: `ml.eia1.medium`\. For more information, see [Use Amazon SageMaker Elastic Inference \(EI\) ](ei.md)\.
  + `"content_template"` – \(Optional\) A template string used to construct the model input from dataset instances\. It is only used when `"content_type"` is `"application/jsonlines"`\. The template should have only one placeholder, `$features`, which is replaced by the features list at runtime\. For example, given `"content_template":"{\"myfeatures\":$features}"`, if an instance \(no label\) is `1,2,3`, then model input becomes JSON Line `'{"myfeatures":[1,2,3]}'`\.
  + `"content_type"` – \(Optional\) The model input format to be used for getting inferences with the shadow endpoint\. Valid values are `"text/csv"` for CSV, `"application/jsonlines"` for JSON Lines, `application/x-parquet` for Apache Parquet, and `application/x-image` to enable Computer Vision explainability\. The default value is the same as the `dataset_type` format\.
  + `"custom_attributes"` – \(Optional\) Provides additional information about a request for an inference that is submitted to a model hosted on an Amazon SageMaker endpoint\. The information is an opaque value that is forwarded verbatim\. You can use this value, for example, to provide an ID that tracks a request\. You can also use this value to provide metadata that a service endpoint was programmed to process\. The value must consist of no more than 1024 visible US\-ASCII characters, as specified in [Section 3\.3\.6\. Field Value Components](http://tools.ietf.org/html/rfc7230#section-3.2.6) of the Hypertext Transfer Protocol \(HTTP/1\.1\)\.
  + `"endpoint_name"` – \(Optional\) Specifies an existing endpoint to use for getting predictions, Using an existing endpoint reduces the bootstrap time, but can cause a significant load increase for existing endpoints\. Be cautious when specifying an existing production endpoint\.
  + `"endpoint_name-prefix"` – \(Optional\) Provides a custom prefix to the name of the temporary endpoint\.
  + `"initial_instance_count"` – Instance count for the shadow endpoint\.
  + `"instance_type"` – Instance type for the shadow endpoint\.
  + `"label"` – \(Optional\) Index or JMESPath location in the model output for the target attribute that is used by the bias metrics\. If the `label` is not provided in the CSV `accept_type` case, then Clarify assumes that the model output is a single numeric value corresponding to the score or probability\. 
  + `"label_headers"` – \(Optional\) A list of values that the `"label"` takes in the dataset\. Associates the scores returned by the model endpoint with their corresponding label values\. Used to extract the label value with the highest score as the predicted label\.
  + `"model_name"` – Model name created by `CreateModel` API, with container mode as `SingleModel`\.
  + `"probability"` – \(Optional\) Index or JMESPath location in the model output for probabilities or scores to be used for explainability\. If the model output is JSON Lines with a list of labels and probabilities, for example, then the label that corresponds to the maximum probability is selected for bias computations\. For explainability method, currently all probabilities are explained\.
  + `"target_model"` – \(Optional\) Sets the target model name when using a multi\-model endpoint\. For more information about multi\-model endpoints, see [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_runtime_InvokeEndpoint.html#API_runtime_InvokeEndpoint_RequestSyntax](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_runtime_InvokeEndpoint.html#API_runtime_InvokeEndpoint_RequestSyntax)\.
+ `"probability_threshold"` – \(Optional\) A float value to indicate the threshold to select the binary label in the case of binary classification\. This parameter is used in object detection to filter out objects detected with confidence scores lower than the `probability_threshold` value\. The default value is 0\.5\.
+ `"report"` – \(Optional\) Section on report parameters\. A report is generated if this section is present\.
  + `"name"` – \(Optional\) Filename prefix for the report notebook and PDF file\. The default name is `"report"`\. 
  + `"title"` – \(Optional\) Title string for the report notebook and PDF file\. The default title is `"SageMaker Analysis Report"`\.
+ `"version"` – \(Optional\) Schema version of the configuration file\. If not provided, the latest supported version is used\.

## Example JSON configuration files<a name="clarify-processing-job-configure-analysis-examples"></a>

Here are examples of analysis configuration JSON files for CSV, JSON Lines, image, and text datasets\.

**Topics**
+ [Analysis configuration JSON file for a CSV dataset](#clarify-analysis-configure-csv-example)
+ [Analysis configuration JSON file for a JSON Lines dataset](#clarify-analysis-configure-JSONLines-example)
+ [Analysis configuration JSON file for an image dataset](#clarify-analysis-configure-computer-vision-example)
+ [NLP analysis configuration JSON file for a text dataset](#clarify-analysis-configure-nlp-example)

### Analysis configuration JSON file for a CSV dataset<a name="clarify-analysis-configure-csv-example"></a>

The following code sample shows how to configure an analysis for a CSV dataset\.

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

### Analysis configuration JSON file for a JSON Lines dataset<a name="clarify-analysis-configure-JSONLines-example"></a>

The following code sample shows how to configure an analysis for a JSON Lines dataset\.

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

Model output as JSON Lines is as follows:

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

### Analysis configuration JSON file for an image dataset<a name="clarify-analysis-configure-computer-vision-example"></a>

The following code sample shows how to configure an analysis for object detection with Computer Vision\.

```
"dataset_type": "application/x-image",
"dataset_uri": "s3://<BUCKET/KEY">
"probability_threshold": 0.7,
"methods": {
    "shap": {
        "num_samples": 500,
        "baseline": "s3://path/to/baseline/image/noise_rgb.png",
        "image_config": {
            "model_type": "(OBJECT_DETECTION|IMAGE_CLASSIFICATION)",
            "num_segments": 20,
            "segment_compactness": (5|10|100),
            "max_objects" : 3,
            "overlap": 0.5,
            "context": 1.0
        }
    }
},

"predictor": {
    "endpoint_name": "sagemaker-endpoint-name",
    "content_type": "(image/jpeg | image/png | application/x-npy)",
    "label_headers": [...] # Required for CV
}
```

### NLP analysis configuration JSON file for a text dataset<a name="clarify-analysis-configure-nlp-example"></a>

The following code sample shows how to configure an analysis for natural processing\.

```
{
  "dataset_type":...
  .
  .
  "methods": {
      "shap" : {
         "baseline": ".."
         "num_samples": 100
         *"text_config": {
            "granularity": "(token|sentence|paragraph)"
          }*
       }
   .
   .
  }
```