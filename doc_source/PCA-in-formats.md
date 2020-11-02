# PCA Response Formats<a name="PCA-in-formats"></a>

All Amazon SageMaker built\-in algorithms adhere to the common input inference format described in [Common Data Formats \- Inference](https://docs.aws.amazon.com/sagemaker/latest/dg/cdf-inference.html)\. This topic contains a list of the available output formats for the SageMaker PCA algorithm\.

## JSON Response Format<a name="PCA-json"></a>

Accept—application/json

```
{
    "projections": [
        {
            "projection": [1.0, 2.0, 3.0, 4.0, 5.0]
        },
        {
            "projection": [6.0, 7.0, 8.0, 9.0, 0.0]
        },
        ....
    ]
}
```

## JSONLINES Response Format<a name="PCA-jsonlines"></a>

Accept—application/jsonlines

```
{ "projection": [1.0, 2.0, 3.0, 4.0, 5.0] }
{ "projection": [6.0, 7.0, 8.0, 9.0, 0.0] }
```

## RECORDIO Response Format<a name="PCA-recordio"></a>

Accept—application/x\-recordio\-protobuf

```
[
    Record = {
        features = {},
        label = {
            'projection': {
                keys: [],
                values: [1.0, 2.0, 3.0, 4.0, 5.0]
            }
        }
    },
    Record = {
        features = {},
        label = {
            'projection': {
                keys: [],
                values: [1.0, 2.0, 3.0, 4.0, 5.0]
            }
        }
    }  
]
```