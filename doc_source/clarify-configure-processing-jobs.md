# Configure an Amazon SageMaker Clarify Processing Jobs for Fairness and Explainability<a name="clarify-configure-processing-jobs"></a>

This topic describes how to configure an Amazon SageMaker Clarify processing job capable of computing bias metrics and feature attributions for explainability\. It is implemented using a specialized SageMaker Clarify container image\. Instructions are provided for how to locate and download one of these container images\. A brief overview of how SageMaker Clarify works is sketched\. The parameters needed to configure the processing job and type of analysis are described\. Prerequisites are outlined and some advice about compute resources consumed by SageMaker Clarify processing job is provided\.

**Topics**
+ [Prerequisites](#clarify-processing-job-configure-prerequisites)
+ [Getting Started with a SageMaker Clarify Container](#clarify-processing-job-configure-container)
+ [How It Works](#clarify-processing-job-configure-how-it-works)
+ [Configure a Processing Job Container's Input and Output Parameters](#clarify-processing-job-configure-parameters)
+ [Configure the Analysis](#clarify-processing-job-configure-analysis)

## Prerequisites<a name="clarify-processing-job-configure-prerequisites"></a>

Before you begin, you need to meet the following prerequisites: 
+ You need to provide an input dataset as tabular files in CSV or JSONLines format\. The input dataset must include a label column for bias analysis\. The dataset should be prepared for machine learning with any pre\-processing needed, such as data cleaning or feature engineering, already completed\.
+ You need to provide a model artifact that supports either the CSV or JSONLines file format as one of its content type inputs\. For posttraining bias metrics and explainability, we use the dataset to make inferences with the model artifact\. Each row minus the label column must be ready to be used as payload for inferences\.
+ When creating processing jobs with the SageMaker container image, you need the following:
  + Network isolation must be disabled for the processing job\.
  + If the model is in a VPC, the processing job must be in the same VPC as the model\.
  + The IAM user/role of the caller must have permissions for SageMaker APIs\. We recommend that you use the `“arn:aws:iam::aws:policy/AmazonSageMakerFullAccess”` managed policy\.

## Getting Started with a SageMaker Clarify Container<a name="clarify-processing-job-configure-container"></a>

Amazon SageMaker provides prebuilt SageMaker Clarify container images that include the libraries and other dependencies needed to compute bias metrics and feature attributions for explainability\. This image has been enabled to run SageMaker [Process Data and Evaluate Models](processing-job.md) in your account\. 

The image URIs for the containers are in the following form:

```
<ACCOUNT_ID>.dkr.ecr.<REGION_NAME>.amazonaws.com/sagemaker-clarify-processing:1.0
```

For example:

```
205585389593.dkr.ecr.us-east-1.amazonaws.com/sagemaker-clarify-processing:1.0
```

The following table lists the addresses by AWS Region\.


**Docker Images for Pretraining Bias Detection**  

| Region | Image address | 
| --- | --- | 
| us\-east\-1 | 205585389593\.dkr\.ecr\.us\-east\-1\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| us\-east\-2 | 211330385671\.dkr\.ecr\.us\-east\-2\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| us\-west\-1 | 740489534195\.dkr\.ecr\.us\-west\-1\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| us\-west\-2 | 306415355426\.dkr\.ecr\.us\-west\-2\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| ap\-east\-1 | 098760798382\.dkr\.ecr\.ap\-east\-1\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| ap\-south\-1 | 452307495513\.dkr\.ecr\.ap\-south\-1\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| ap\-northeast\-2 | 263625296855\.dkr\.ecr\.ap\-northeast\-2\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| ap\-southeast\-1 | 834264404009\.dkr\.ecr\.ap\-southeast\-1\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| ap\-southeast\-2 | 007051062584\.dkr\.ecr\.ap\-southeast\-2\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| ap\-northeast\-1 | 377024640650\.dkr\.ecr\.ap\-northeast\-1\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| ca\-central\-1 | 675030665977\.dkr\.ecr\.ca\-central\-1\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| eu\-central\-1 | 017069133835\.dkr\.ecr\.eu\-central\-1\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| eu\-west\-1 | 131013547314\.dkr\.ecr\.eu\-west\-1\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| eu\-west\-2 | 440796970383\.dkr\.ecr\.eu\-west\-2\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| eu\-west\-3 | 341593696636\.dkr\.ecr\.eu\-west\-3\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| eu\-north\-1 | 763603941244\.dkr\.ecr\.eu\-north\-1\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| me\-south\-1 | 835444307964\.dkr\.ecr\.me\-south\-1\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| sa\-east\-1 | 520018980103\.dkr\.ecr\.sa\-east\-1\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| af\-south\-1 | 811711786498\.dkr\.ecr\.af\-south\-1\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 
| eu\-south\-1 | 638885417683\.dkr\.ecr\.eu\-south\-1\.amazonaws\.com/sagemaker\-clarify\-processing:1\.0 | 

## How It Works<a name="clarify-processing-job-configure-how-it-works"></a>

A SageMaker processing job used the SageMaker Clarify container at several stages in the lifecycle of the machine learning workflow\. You can use the SageMaker Clarify container with your datasets and models to compute the following types of analysis:
+ pretraining bias metrics
+ posttraining bias metrics
+ SHAP values for explainability

You can control which of these analyses are computed when you configure the processing job\. For pretraining bias metrics, you need to provide the dataset\. You can compute posttraining bias metrics and explainability after your model has been trained by providing the dataset and model name\. You must configure the necessary parameters in the form of a JSON configuration file and provide this as an input to the processing job\.

After the processing job completes, the result of the analyses is saved in the output location specified in the `ProcessingOutput` parameters of the processing job\. You can then download it from there and view the outputs or you can view the results in Studio if you have run a notebook there\.

In order to compute posttraining bias metrics and SHAP values, the computation needs to get inferences for the model name provided\. To accomplish this, the processing job creates an ephemeral endpoint with the model name, known as a *shadow endpoint*\. The processing job deletes the shadow endpoint after the computations are completed\. 

At a high level, the processing job completes the following steps:

1. Validate inputs and parameters\.

1. Create the shadow endpoint\.

1. Compute pretraining bias metrics\.

1. Compute posttraining bias\-metrics\.

1. Compute local and global feature attributions\.

1. Delete shadow endpoint\.

1. Generate output files\.

## Configure a Processing Job Container's Input and Output Parameters<a name="clarify-processing-job-configure-parameters"></a>

The Processing Job requires that you specify the following input parameters: a dataset files with input name `"dataset"` as S3 object or prefix, and an analysis configuration file with input name `"analysis_config"` as an S3 object\. The job also requires an output parameter: the output location as an S3 prefix\.

You can create and run a processing job with SageMaker [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateProcessingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateProcessingJob.html) API using the AWS SDK or CLI or [SageMaker Python SDK](https://sagemaker.readthedocs.io/)\.

Using SageMaker Python SDK, create a `Processor` using the SageMaker Clarify container image URI:

```
from sagemaker import clarify
clarify_processor = clarify.SageMakerClarifyProcessor(role=role,
                         instance_count=1,
                         instance_type='ml.c5.xlarge',
                         max_runtime_in_seconds=1200,
                         volume_size_in_gb=100)
```

Then, you must also provide the input and output parameters for the `SageMakerClarifyProcessor`\. If you provide the `"dataset_uri"`, the dataset input is optional\.

```
    dataset_path = "s3://my_bucket/my_folder/train.csv"
    analysis_config_path = "s3://my_bucket/my_folder/analysis_config.json"
    analysis_result_path = "s3://my_bucket/my_folder/output"
    
    analysis_config_input = ProcessingInput(
                        input_name="analysis_config",
                        source=analysis_config_path,
                        destination="/opt/ml/processing/input/config",
                        s3_data_type="S3Prefix",
                        s3_input_mode="File",
                        s3_compression_type="None")
    dataset_input = ProcessingInput(
                        input_name="dataset",
                        source=dataset_path,
                        destination="/opt/ml/processing/input/data",
                        s3_data_type="S3Prefix",
                        s3_input_mode="File",
                        s3_compression_type="None")
    analysis_result_output = ProcessingOutput(
                        source="/opt/ml/processing/output",
                        destination=analysis_result_path,
                        output_name="analysis_result",
                        s3_upload_mode="EndOfJob")
```

## Configure the Analysis<a name="clarify-processing-job-configure-analysis"></a>

Parameters for the analysis configuration must be provided in a JSON file named "analysis\_config\.json"\. The path must be provided in the `ProcessingInput` parameter named “analysis\_config"\. Examples of analysis configuration files are provided below\. In this JSON configuration file, you specify the following parameters:
+ `"version"` – \(Optional\) Schema version of the configuration file\. The latest supported version is used if not provided\.
+ `"dataset_type"` – Format of the dataset\. Valid values are `"text/csv"` for CSV and `"application/jsonlines"` for JSON Lines\.
+ `"dataset_uri"` – \(Optional\) Dataset S3 prefix/object URI \(if not given as `ProcessingInput`\)\. If it is a prefix, then processing job recursively collects all S3 files under the prefix\.
+ `"headers"` – \(Optional\) A list of column names in the dataset\. If the `dataset_type` is `"application/jsonlines"` and `"label"` is specified, then the last header shall be the header of label column\. 
+ `"label"` – \(Optional\) Target attribute for the model to be used for *bias metrics* specified as a column name or index if the dataset format is CSV or as JSONPath if the dataset format is JSONLines\. 
+ `"probability_threshold"` – \(Optional\) A float value to indicate the threshold to select the binary label in the case of binary classification\. The default value is 0\.5\.
+ `"features"` – \(Optional\) JSONPath for locating the feature columns for bias metrics if the dataset format is JSONLines\.
+ `"label_values_or_threshold"` – \(Optional\) List of label values or threshold to indicate positive outcome used for bias metrics\.
+ `"facet"` – \(Optional\) A list of features that are sensitive attributes, referred to as facets, to be used for bias metrics in the form of pairs to include the following:
  + `"name_or_index"` – Facet column name or index\.
  + `"value_or_threshold"` – \(Optional\) List of values or threshold that the facet column can take which indicates the sensitive group, i\.e\. the group that is used to measure bias against\. If not provided, bias metrics are computed as one group versus all for every unique value\. If the facet column is numeric this threshold value is applied as the lower bound to select the sensitive group\.
+ `"group_variable"` – \(Optional\) A column name or index to indicate group variable to be used for the *bias metric* *Conditional Demographic Disparity\.*
+ `"methods"` – A list of methods and their parameters for the analyses and reports\. If any section is omitted then it is not computed\.
  + `"pre_training_bias"` – \(Optional\) Section on pretraining bias metrics\.
    + `"methods"` – A list of pretraining metrics to be computed\.
  + `"post_training_bias"` – \(Optional\) Section on posttraining bias metrics\.
    + `"methods"` – A list of posttraining metrics to be computed\.
  + `"shap"` – \(Optional\) Section on SHAP value computation\.
    + `"baseline"` – A list of rows \(at least one\) or S3 object URI to be used as the baseline dataset \(also known as a background dataset\) in the Kernel SHAP algorithm\. The format should be the same as the dataset format\. Each row should contain only the feature columns/values and omit the label column/values
    + `"num_samples"` – Number of samples to be used in the Kernel SHAP algorithm\. This number determines the size of the generated synthetic dataset to compute the SHAP values\.
    + `"agg_method"` – Aggregation method for global SHAP values\. Valid values are as follows:
      + `"mean_abs"` – Mean of absolute SHAP values for all instances\.
      + `"median"` – Median of SHAP values for all instances\.
      + `"mean_sq"` – Mean of squared SHAP values for all instances\.
    + `"use_logit"` – \(Optional\) Boolean value to indicate if logit function is to be applied to the model predictions\. The default value is "false"\. If "use\_logit" is `"true"` then the SHAP values have log\-odds units\.
    + `"save_local_shap_values"` – \(Optional\) Boolean value to indicate if local SHAP values are to be saved in the output location\. Default is `"true"`\.
+ `"predictor"` – \(Optional\) Section on model parameters, required if `"shap"` and `"post_training_bias"` sections are present\.
  + `"model_name"` – Model name \(as created by `CreateModel` API with container mode as `SingleModel`\.
  + `"instance_type"` – Instance type for the shadow endpoint\.
  + `"initial_instance_count"` – Instance count for the shadow endpoint\.
  + `"content_type"` – \(Optional\) The model input format to be used for getting inferences with the shadow endpoint\. Valid values are `"text/csv"` for CSV and `"application/jsonlines"`\. The default value is the same as dataset format\.
  + `"accept_type"` – \(Optional\) The model *output* format to be used for getting inferences with the shadow endpoint\. Valid values are `"text/csv"` for CSV and `"application/jsonlines"`\. The default value is the same as `content_type`\.
  + `"label"` – \(Optional\) Index or JSONPath location in the model output for the target attribute to be used bias metrics\. In CSV `accept_type` case, if it is not provided then assume that the model output is a single numeric value corresponding to score or probability\. 
  + `"probability"` – \(Optional\) Index or JSONPath location in the model output for probabilities or scores to be used for explainability\. For example, if the model output is JSONLines with a list of labels and probabilities, then for bias methods, the label that corresponds to the maximum probability is selected for bias computations\. For explainability method, currently all probabilities are explained\.
  + `"label_headers"` – \(Optional\) A list of values that the `"label"` takes in the dataset that describe which label value each of the scores returned by the model endpoint correspond to\. It is used to extract the label value with the highest score as predicted label for bias computations\.
  + `"content_template"` – \(Optional\) A template string to be used to construct the model input from dataset instances\. It is only used when `"content_type"` is `"application/jsonlines"`\. The template should have one and only one placeholder, `$features`, which is replaced by features list at runtime\. For example, given `"content_template":"{\"myfeatures\":$features}"`, if an instance \(no label\) is 1,2,3 then model input becomes JSONline `'{"myfeatures":[1,2,3]}'`\.
+ `"report"` – \(Optional\) Section on report parameters\. A report is generated if this section is present\.
  + `"name"` – \(Optional\) Filename prefix for the report notebook and PDF file\. The default name is `"report"`\. 
  + `"title"` – \(Optional\) Title string for the report notebook and PDF file\. The default title is `"SageMaker Analysis Report"`\.

### Example Analysis Configuration JSON File for a CSV Dataset<a name="clarify-analysis-configure-csv-example"></a>

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
            "save_local_shap_values": true
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
        "label": 0,
        "probability": 1
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

### Example Analysis Configuration JSON File for a JSONLines Dataset<a name="clarify-analysis-configure-JSONLines-example"></a>

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