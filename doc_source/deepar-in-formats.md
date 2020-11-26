# DeepAR Inference Formats<a name="deepar-in-formats"></a>

## DeepAR JSON Request Formats<a name="deepar-json-request"></a>

Query a trained model by using the model's endpoint\. The endpoint takes the following JSON request format\. 

In the request, the `instances` field corresponds to the time series that should be forecast by the model\. 

If the model was trained with categories, you must provide a `cat` for each instance\. If the model was trained without the `cat` field, it should be omitted\.

If the model was trained with a custom feature time series \(`dynamic_feat`\), you have to provide the same number of `dynamic_feat`values for each instance\. Each of them should have a length given by `length(target) + prediction_length`, where the last `prediction_length` values correspond to the time points in the future that will be predicted\. If the model was trained without custom feature time series, the field should not be included in the request\.

```
{
    "instances": [
        {
            "start": "2009-11-01 00:00:00",
            "target": [4.0, 10.0, "NaN", 100.0, 113.0],
            "cat": [0, 1],
            "dynamic_feat": [[1.0, 1.1, 2.1, 0.5, 3.1, 4.1, 1.2, 5.0, ...]]
        },
        {
            "start": "2012-01-30",
            "target": [1.0],
            "cat": [2, 1],
            "dynamic_feat": [[2.0, 3.1, 4.5, 1.5, 1.8, 3.2, 0.1, 3.0, ...]]
        },
        {
            "start": "1999-01-30",
            "target": [2.0, 1.0],
            "cat": [1, 3],
            "dynamic_feat": [[1.0, 0.1, -2.5, 0.3, 2.0, -1.2, -0.1, -3.0, ...]]
        }
    ],
    "configuration": {
         "num_samples": 50,
         "output_types": ["mean", "quantiles", "samples"],
         "quantiles": ["0.5", "0.9"]
    }
}
```

The `configuration` field is optional\. `configuration.num_samples` sets the number of sample paths that the model generates to estimate the mean and quantiles\. `configuration.output_types` describes the information that will be returned in the request\. Valid values are `"mean"` `"quantiles"` and `"samples"`\. If you specify `"quantiles"`, each of the quantile values in `configuration.quantiles` is returned as a time series\. If you specify `"samples"`, the model also returns the raw samples used to calculate the other outputs\.

## DeepAR JSON Response Formats<a name="deepar-json-response"></a>

The following is the format of a response, where `[...]` are arrays of numbers:

```
{
    "predictions": [
        {
            "quantiles": {
                "0.9": [...],
                "0.5": [...]
            },
            "samples": [...],
            "mean": [...]
        },
        {
            "quantiles": {
                "0.9": [...],
                "0.5": [...]
            },
            "samples": [...],
            "mean": [...]
        },
        {
            "quantiles": {
                "0.9": [...],
                "0.5": [...]
            },
            "samples": [...],
            "mean": [...]
        }
    ]
}
```

DeepAR has a response timeout of 60 seconds\. When passing multiple time series in a single request, the forecasts are generated sequentially\. Because the forecast for each time series typically takes about 300 to 1000 milliseconds or longer, depending on the model size, passing too many time series in a single request can cause time outs\. It's better to send fewer time series per request and send more requests\. Because the DeepAR algorithm uses multiple workers per instance, you can achieve much higher throughput by sending multiple requests in parallel\.

By default, DeepAR uses one worker per CPU for inference, if there is sufficient memory per CPU\. If the model is large and there isn't enough memory to run a model on each CPU, the number of workers is reduced\. The number of workers used for inference can be overwritten using the environment variable `MODEL_SERVER_WORKERS` For example, by setting `MODEL_SERVER_WORKERS=1`\) when calling the SageMaker [ `CreateModel`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html) API\.

## Batch Transform with the DeepAR Algorithm<a name="deepar-batch"></a>

DeepAR forecasting supports getting inferences by using batch transform from data using the JSON Lines format\. In this format, each record is represented on a single line as a JSON object, and lines are separated by newline characters\. The format is identical to the JSON Lines format used for model training\. For information, see [Input/Output Interface for the DeepAR Algorithm](deepar.md#deepar-inputoutput)\. For example:

```
{"start": "2009-11-01 00:00:00", "target": [4.3, "NaN", 5.1, ...], "cat": [0, 1], "dynamic_feat": [[1.1, 1.2, 0.5, ..]]}
{"start": "2012-01-30 00:00:00", "target": [1.0, -5.0, ...], "cat": [2, 3], "dynamic_feat": [[1.1, 2.05, ...]]}
{"start": "1999-01-30 00:00:00", "target": [2.0, 1.0], "cat": [1, 4], "dynamic_feat": [[1.3, 0.4]]}
```

**Note**  
When creating the transformation job with [ `CreateTransformJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTransformJob.html), set the `BatchStrategy` value to `SingleRecord` and set the `SplitType` value in the [ `TransformInput`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_TransformInput.html) configuration to `Line`, as the default values currently cause runtime failures\.

Similar to the hosted endpoint inference request format, the `cat` and the `dynamic_feat` fields for each instance are required if both of the following are true:
+ The model is trained on a dataset that contained both the `cat` and the `dynamic_feat` fields\.
+ The corresponding `cardinality` and `num_dynamic_feat` values used in the training job are not set to `"".`

Unlike hosted endpoint inference, the configuration field is set once for the entire batch inference job using an environment variable named `DEEPAR_INFERENCE_CONFIG`\. The value of `DEEPAR_INFERENCE_CONFIG` can be passed when the model is created by calling [ `CreateTransformJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTransformJob.html) API\. If `DEEPAR_INFERENCE_CONFIG` is missing in the container environment, the inference container uses the following default:

```
{
    "num_samples": 100,
    "output_types": ["mean", "quantiles"],
    "quantiles": ["0.1", "0.2", "0.3", "0.4", "0.5", "0.6", "0.7", "0.8", "0.9"]
}
```

The output is also in JSON Lines format, with one line per prediction, in an order identical to the instance order in the corresponding input file\. Predictions are encoded as objects identical to the ones returned by responses in online inference mode\. For example:

```
{ "quantiles": { "0.1": [...], "0.2": [...] }, "samples": [...], "mean": [...] }
```

Note that in the [ `TransformInput`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_TransformInput.html) configuration of the SageMaker [ `CreateTransformJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTransformJob.html) request clients must explicitly set the `AssembleWith` value to `Line`, as the default value `None` concatenates all JSON objects on the same line\.

For example, here is a SageMaker [ `CreateTransformJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTransformJob.html) request for a DeepAR job with a custom `DEEPAR_INFERENCE_CONFIG`:

```
{
   "BatchStrategy": "SingleRecord",
   "Environment": { 
      "DEEPAR_INFERENCE_CONFIG" : "{ \"num_samples\": 200, \"output_types\": [\"mean\"] }",
      ...
   },
   "TransformInput": {
      "SplitType": "Line",
      ...
   },
   "TransformOutput": { 
      "AssembleWith": "Line",
      ...
   },
   ...
}
```