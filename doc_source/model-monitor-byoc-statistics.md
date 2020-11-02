# Schema for Statistics \(statistics\.json file\)<a name="model-monitor-byoc-statistics"></a>

The schema defined in the `statistics.json` file specifies the statistical parameters to be calculated for the baseline and data that is captured\. It also configures the bucket to be used by [KLL](https://datasketches.github.io/docs/Quantiles/KLLSketch.html), a very compact quantiles sketch with lazy compaction scheme\.

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
+ The specified metrics are recognized by SageMaker in later visualization changes\. The container can emit more metrics if required\.
+ [KLL sketch](https://datasketches.github.io/docs/Quantiles/KLLSketch.html) is the recognized sketch\. Custom containers can write their own representation, but it won’t be recognized by SageMaker in visualizations\.
+ By default, the distribution is materialized in 10 buckets\. You can't change this\.