# DeepAR Request and Response Formats<a name="deepar-in-formats"></a>

Query a trained model by using the model's endpoint\. The endpoint takes the following JSON request format\. 

## <a name="ntm-json"></a>

```
{
 "instances": [
  { "start": "2009-11-01 00:00:00", "target": [4.0, 10.0, 50.0, 100.0, 113.0], "cat": 0},
  { "start": "2012-01-30", "target": [1.0], "cat": 2 },
  { "start": "1999-01-30", "target": [2.0, 1.0], "cat": 1 }
 ],
 "configuration": {
  "num_samples": 50,
  "output_types": ["mean", "quantiles", "samples"],
  "quantiles": ["0.5", "0.9"]
 }
}
```

In the request, the `instances` field corresponds to the time series that should be forecast by the model\. If the model was trained with categories \(`embedding_dimension` and `cardinality` > 0\), you must provide `cat` in the request\. If the model was trained without the `cat` field, it can be omitted\.

The `configuration` field is optional\. `configuration.num_samples` sets the number of sample paths that the model generates to estimate the mean and quantiles\. `configuration.output_types` describes the information that will be returned in the request\. Valid values are `"mean"` `"quantiles"` and `"samples"`\. If you specify `"quantiles"`, each of the quantile values in `configuration.quantiles` is returned as a time series\. If you specify `"samples"`, the model also returns the raw samples used to calculate the other outputs\.

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