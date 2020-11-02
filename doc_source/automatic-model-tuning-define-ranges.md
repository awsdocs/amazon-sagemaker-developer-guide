# Define Hyperparameter Ranges<a name="automatic-model-tuning-define-ranges"></a>

Hyperparameter tuning finds the best hyperparameter values for your model by searching over ranges of hyperparameters\. You specify the hyperparameters and range of values over which to search by defining hyperparameter ranges for your tuning job\. Choosing hyperparameters and ranges significantly affects the performance of your tuning job\. For guidance on choosing hyperparameters and ranges, see [Best Practices for Hyperparameter Tuning](automatic-model-tuning-considerations.md)\.

To define hyperparameter ranges by using the low\-level API, you specify the names of hyperparameters and ranges of values in the `ParameterRanges` field of the `HyperParameterTuningJobConfig` parameter that you pass to the [ `CreateHyperParameterTuningJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateHyperParameterTuningJob.html) operation\. The `ParameterRanges` field has three subfields, one for each of the categorical, integer, and continuous hyperparameter ranges\. You can define up to 20 hyperparameters to search over\. Each value of a categorical hyperparameter range counts as a hyperparameter against the limit\. Hyperparameter ranges have the following structure:

```
"ParameterRanges": {
      "CategoricalParameterRanges": [
        {
          "Name": "tree_method",
          "Values": ["auto", "exact", "approx", "hist"]
        }
      ],
      "ContinuousParameterRanges": [
        {
          "Name": "eta",
          "MaxValue" : "0.5",
          "MinValue": "0",
          "ScalingType": "Auto"
        }
      ],
      "IntegerParameterRanges": [
        {
          "Name": "max_depth",
          "MaxValue": "10",
          "MinValue": "1",
          "ScalingType": "Auto"
        }
      ]
    }
```

## Hyperparameter Scaling<a name="scaling-type"></a>

For integer and continuous hyperparameter ranges, you can choose the scale you want hyperparameter tuning to use to search the range of values by specifying a value for the `ScalingType` field of the hyperparameter range\. You can choose from the following scaling types:

Auto  
SageMaker hyperparameter tuning chooses the best scale for the hyperparameter\.

Linear  
Hyperparameter tuning searches the values in the hyperparameter range by using a linear scale\. Typically, you choose this if the range of all values from the lowest to the highest is relatively small \(within one order of magnitude\), because uniformly searching values from the range will give you a reasonable exploration of the entire range\.

Logarithmic  
Hyperparameter tuning searches the values in the hyperparameter range by using a logarithmic scale\.  
Logarithmic scaling works only for ranges that have only values greater than 0\.  
Choose logarithmic scaling when you are searching a range that spans several orders of magnitude\. For example, if you are tuning a [Tune a linear learner model](linear-learner.md) model, and you specify a range of values between \.0001 and 1\.0 for the `learning_rate` hyperparameter, searching uniformly on a logarithmic scale gives you a better sample of the entire range than searching on a linear scale would, because searching on a linear scale would, on average, devote 90 percent of your training budget to only the values between \.1 and 1\.0, leaving only 10 percent of your training budget for the values between \.0001 and \.1\.

ReverseLogarithmic  
Hyperparameter tuning searches the values in the hyperparameter range by using a reverse logarithmic scale\. reverse logarithmic scaling is supported only for continuous hyperparameter ranges\. It is not supported for integer hyperparameter ranges\.  
Reverse logarithmic scaling works only for ranges that are entirely within the range 0<=x<1\.0\.  
Choose reverse logarithmic scaling when you are searching a range that is highly sensitive to small changes that are very close to 1\.

For an example notebook that uses hyperparameter scaling, see [https://github\.com/awslabs/amazon\-sagemaker\-examples/blob/master/hyperparameter\_tuning/xgboost\_random\_log/hpo\_xgboost\_random\_log\.ipynb](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/hyperparameter_tuning/xgboost_random_log/hpo_xgboost_random_log.ipynb)\.