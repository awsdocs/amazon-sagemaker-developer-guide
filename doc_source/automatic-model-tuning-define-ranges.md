# Defining Hyperparameter Ranges<a name="automatic-model-tuning-define-ranges"></a>

Hyperparameter tuning finds the best hyperparameter values for your model by searching over ranges of hyperparameters\. You specify the hyperparameters and range of values over which to search by defining hyperparameter ranges for your tuning job\. Choosing hyperparameters and ranges significantly affects the performance of your tuning job\. For guidance on choosing hyperparameters and ranges, see [Design Considerations](automatic-model-tuning-considerations.md)\.

To define hyperparameter ranges by using the low\-level API, you specify the names of hyperparameters and ranges of values in the `ParameterRanges` field of the `HyperParameterTuningJobConfig` parameter that you pass to the [CreateHyperParameterTuningJob](API_CreateHyperParameterTuningJob.md) operation\. The `ParameterRanges` field has three subfields, one for each of the categorical, integer, and continuous hyperparameter ranges\. You can define up to 20 hyperparameters to search over, but each value of a categorical hyperparameter range counts against that limit\. Hyperparameter ranges have the following structure:

```
"ParameterRanges": {
      "CategoricalParameterRanges": [
        {
          "Name": "tree_method",
          "Values": ["auto", "exact", "approx", "hist"]
      ],
      "ContinuousParameterRanges": [
        {
          "Name": "eta",
          "Type": "Continuous",
          "MinValue": "0"
        }
      ],
      "IntegerParameterRanges": [
        {
          "Name": "max_depth",
          "MaxValue": "10",
          "MinValue": "1", 
        }
      ]
    }
```