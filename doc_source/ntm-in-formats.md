# NTM Response Formats<a name="ntm-in-formats"></a>

All Amazon SageMaker built\-in algorithms adhere to the common input inference format described in [Common Data Formats \- Inference](https://docs.aws.amazon.com/sagemaker/latest/dg/cdf-inference.html)\. This topic contains a list of the available output formats for the SageMaker NTM algorithm\.

## JSON Response Format<a name="ntm-json"></a>

```
{
    "predictions":    [
        {"topic_weights": [0.02, 0.1, 0,...]},
        {"topic_weights": [0.25, 0.067, 0,...]}
    ]
}
```

## JSONLINES Response Format<a name="ntm-jsonlines"></a>

```
{"topic_weights": [0.02, 0.1, 0,...]}
{"topic_weights": [0.25, 0.067, 0,...]}
```

## RECORDIO Response Format<a name="ntm-recordio"></a>

```
[
    Record = {
        features = {},
        label = {
            'topic_weights': {
                keys: [],
                values: [0.25, 0.067, 0, ...]  # float32
            }
        }
    },
    Record = {
        features = {},
        label = {
            'topic_weights': {
                keys: [],
                values: [0.25, 0.067, 0, ...]  # float32
            }
        }
    }  
]
```