# Schema for Statistics \(statistics\.json file\)<a name="model-monitor-interpreting-statistics"></a>

Amazon SageMaker Model Monitor pre\-built container computes per column/feature statistics\. The statistics are calculated for the baseline dataset and also for the current dataset that is being analyzed\.

```
{
    "version": 0,
    # dataset level stats
    "dataset": {
        "item_count": number
    },
    # feature level stats
    "features": [
        {
            "name": "feature-name",
            "inferred_type": "Fractional" | "Integral",
            "numerical_statistics": {
                "common": {
                    "num_present": number,
                    "num_missing": number
                },
                "mean": number,
                "sum": number,
                "std_dev": number,
                "min": number,
                "max": number,
                "distribution": {
                    "kll": {
                        "buckets": [
                            {
                                "lower_bound": number,
                                "upper_bound": number,
                                "count": number
                            }
                        ],
                        "sketch": {
                            "parameters": {
                                "c": number,
                                "k": number
                            },
                            "data": [
                                [
                                    num,
                                    num,
                                    num,
                                    num
                                ],
                                [
                                    num,
                                    num
                                ][
                                    num,
                                    num
                                ]
                            ]
                        }#sketch
                    }#KLL
                }#distribution
            }#num_stats
        },
        {
            "name": "feature-name",
            "inferred_type": "String",
            "string_statistics": {
                "common": {
                    "num_present": number,
                    "num_missing": number
                },
                "distinct_count": number,
                "distribution": {
                    "categorical": {
                         "buckets": [
                                {
                                    "value": "string",
                                    "count": number
                                }
                          ]
                     }
                }
            },
            #provision for custom stats
        }
    ]
}
```

Note the following:
+ The prebuilt containers compute [KLL sketch](https://datasketches.github.io/docs/Quantiles/KLLSketch.html) which is a compact quantiles sketch\.
+ By default, we materialize the distribution in 10 buckets\. This is not currently configurable\.