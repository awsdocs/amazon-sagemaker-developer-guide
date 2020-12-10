# Model Quality Metrics<a name="model-monitor-model-quality-metrics"></a>

Model quality monitoring jobs compute different metrics depending on the ML problem type\. The following sections list the metrics analyzed for each ML problem type\.

**Note**  
Standard deviation for metrics are provided only when at least 200 samples are available\. Model Monitor computes standard deviation by randomly sampling 80% of the data 5 times, computing the metric, and taking the standard deviation for those results\.

## Regression Metrics<a name="model-monitor-model-quality-metrics-regression"></a>

The following shows an example of the metrics that model quality monitor computes for a regression problem\.

```
"regression_metrics" : {
    "mae" : {
      "value" : 0.3711832061068702,
      "standard_deviation" : 0.0037566388129940394
    },
    "mse" : {
      "value" : 0.3711832061068702,
      "standard_deviation" : 0.0037566388129940524
    },
    "rmse" : {
      "value" : 0.609248066149471,
      "standard_deviation" : 0.003079253267651125
    },
    "r2" : {
      "value" : -1.3766111872212665,
      "standard_deviation" : 0.022653980022771227
    }
  }
```

## Binary Classification Metrics<a name="model-monitor-model-quality-metrics-binary"></a>

The following shows an example of the metrics that model quality monitor computes for a binary classification problem\.

```
"binary_classification_metrics" : {
    "confusion_matrix" : {
      "0" : {
        "0" : 1,
        "1" : 2
      },
      "1" : {
        "0" : 0,
        "1" : 1
      }
    },
    "recall" : {
      "value" : 1.0,
      "standard_deviation" : "NaN"
    },
    "precision" : {
      "value" : 0.3333333333333333,
      "standard_deviation" : "NaN"
    },
    "accuracy" : {
      "value" : 0.5,
      "standard_deviation" : "NaN"
    },
    "recall_best_constant_classifier" : {
      "value" : 1.0,
      "standard_deviation" : "NaN"
    },
    "precision_best_constant_classifier" : {
      "value" : 0.25,
      "standard_deviation" : "NaN"
    },
    "accuracy_best_constant_classifier" : {
      "value" : 0.25,
      "standard_deviation" : "NaN"
    },
    "true_positive_rate" : {
      "value" : 1.0,
      "standard_deviation" : "NaN"
    },
    "true_negative_rate" : {
      "value" : 0.33333333333333337,
      "standard_deviation" : "NaN"
    },
    "false_positive_rate" : {
      "value" : 0.6666666666666666,
      "standard_deviation" : "NaN"
    },
    "false_negative_rate" : {
      "value" : 0.0,
      "standard_deviation" : "NaN"
    },
    "receiver_operating_characteristic_curve" : {
      "false_positive_rates" : [ 0.0, 0.0, 0.0, 0.0, 0.0, 1.0 ],
      "true_positive_rates" : [ 0.0, 0.25, 0.5, 0.75, 1.0, 1.0 ]
    },
    "precision_recall_curve" : {
      "precisions" : [ 1.0, 1.0, 1.0, 1.0, 1.0 ],
      "recalls" : [ 0.0, 0.25, 0.5, 0.75, 1.0 ]
    },
    "auc" : {
      "value" : 1.0,
      "standard_deviation" : "NaN"
    },
    "f0_5" : {
      "value" : 0.3846153846153846,
      "standard_deviation" : "NaN"
    },
    "f1" : {
      "value" : 0.5,
      "standard_deviation" : "NaN"
    },
    "f2" : {
      "value" : 0.7142857142857143,
      "standard_deviation" : "NaN"
    },
    "f0_5_best_constant_classifier" : {
      "value" : 0.29411764705882354,
      "standard_deviation" : "NaN"
    },
    "f1_best_constant_classifier" : {
      "value" : 0.4,
      "standard_deviation" : "NaN"
    },
    "f2_best_constant_classifier" : {
      "value" : 0.625,
      "standard_deviation" : "NaN"
    }
  }
```

## Multiclass Metrics<a name="model-monitor-model-quality-metrics-multi"></a>

The following shows an example of the metrics that model quality monitor computes for a multiclass classification problem\.

```
"multiclass_classification_metrics" : {
    "confusion_matrix" : {
      "0" : {
        "0" : 1180,
        "1" : 510
      },
      "1" : {
        "0" : 268,
        "1" : 138
      }
    },
    "accuracy" : {
      "value" : 0.6288167938931297,
      "standard_deviation" : 0.00375663881299405
    },
    "weighted_recall" : {
      "value" : 0.6288167938931297,
      "standard_deviation" : 0.003756638812994008
    },
    "weighted_precision" : {
      "value" : 0.6983172269629505,
      "standard_deviation" : 0.006195912915307507
    },
    "weighted_f0_5" : {
      "value" : 0.6803947317178771,
      "standard_deviation" : 0.005328406973561699
    },
    "weighted_f1" : {
      "value" : 0.6571162346664904,
      "standard_deviation" : 0.004385008075019733
    },
    "weighted_f2" : {
      "value" : 0.6384024354394601,
      "standard_deviation" : 0.003867109755267757
    },
    "accuracy_best_constant_classifier" : {
      "value" : 0.19370229007633588,
      "standard_deviation" : 0.0032049848450732355
    },
    "weighted_recall_best_constant_classifier" : {
      "value" : 0.19370229007633588,
      "standard_deviation" : 0.0032049848450732355
    },
    "weighted_precision_best_constant_classifier" : {
      "value" : 0.03752057718081697,
      "standard_deviation" : 0.001241536088657851
    },
    "weighted_f0_5_best_constant_classifier" : {
      "value" : 0.04473443104152011,
      "standard_deviation" : 0.0014460485504284792
    },
    "weighted_f1_best_constant_classifier" : {
      "value" : 0.06286421244683643,
      "standard_deviation" : 0.0019113576884608862
    },
    "weighted_f2_best_constant_classifier" : {
      "value" : 0.10570313141262414,
      "standard_deviation" : 0.002734216826748117
    }
  }
```