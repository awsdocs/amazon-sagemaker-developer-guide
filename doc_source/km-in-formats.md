# K\-Means Response Formats<a name="km-in-formats"></a>

All SageMaker built\-in algorithms adhere to the common input inference format described in [Common Data Formats \- Inference](https://docs.aws.amazon.com/sagemaker/latest/dg/cdf-inference.html)\. This topic contains a list of the available output formats for the SageMaker k\-means algorithm\.

## JSON Response Format<a name="km-json"></a>

```
{
    "predictions": [
        {
            "closest_cluster": 1.0,
            "distance_to_cluster": 3.0,
        },
        {
            "closest_cluster": 2.0,
            "distance_to_cluster": 5.0,
        },

        ....
    ]
}
```

## JSONLINES Response Format<a name="km-jsonlines"></a>

```
{"closest_cluster": 1.0, "distance_to_cluster": 3.0}
{"closest_cluster": 2.0, "distance_to_cluster": 5.0}
```

## RECORDIO Response Format<a name="km-recordio"></a>

```
[
    Record = {
        features = {},
        label = {
            'closest_cluster': {
                keys: [],
                values: [1.0, 2.0]  # float32
            },
            'distance_to_cluster': {
                keys: [],
                values: [3.0, 5.0]  # float32
            },
        }
    }
]
```

## CSV Response Format<a name="km-csv"></a>

The first value in each line corresponds to `closest_cluster`\.

The second value in each line corresponds to `distance_to_cluster`\.

```
1.0,3.0
2.0,5.0
```