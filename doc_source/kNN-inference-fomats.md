# K\-NN Request and Response Formats<a name="kNN-inference-fomats"></a>

## INPUT: CSV<a name="kNN-input-csv"></a>

content\-type: text/csv

```
1.2,1.3,9.6,20.3
```

This accepts a `label_size` or encoding parameter\. It assumes a `label_size` of 0 and a utf\-8 encoding\.

## INPUT: JSON<a name="kNN-input-json"></a>

content\-type: application/json

```
{
  "instances": [
    {"data": {"features": {"values": [-3, -1, -4, 2]}}},
    {"features": [3.0, 0.1, 0.04, 0.002]}]
}
```

## INPUT: JSONLINES<a name="kNN-input-jsonlines"></a>

content\-type: application/jsonlines

```
{"features": [1.5, 16.0, 14.0, 23.0]}
{"data": {"features": {"values": [1.5, 16.0, 14.0, 23.0]}}
```

## INPUT: RECORDIO<a name="kNN-input-recordio"></a>

content\-type: application/x\-recordio\-protobuf

```
[
    Record = {
        features = {
            'values': {
                values: [-3, -1, -4, 2]  # float32
            }
        },
        label = {}
    },
    Record = {
        features = {
            'values': {
                values: [3.0, 0.1, 0.04, 0.002]  # float32
            }
        },
        label = {}
    },
]
```

## OUTPUT: JSON<a name="kNN-output-json"></a>

accept: application/json

```
{
  "predictions": [
    {"predicted_label": 0.0},
    {"predicted_label": 2.0}
  ]
}
```

## OUTPUT: JSONLINES<a name="kNN-output-jsonlines"></a>

accept: application/jsonlines

```
{"predicted_label": 0.0}
{"predicted_label": 2.0}
```

## OUTPUT: VERBOSE JSON<a name="kNN-output-recordio"></a>

In verbose mode, the API provides the search results with the distances vector sorted from smallest to largest, with corresponding elements in the labels vector\. In this example, k is set to 3\.

accept: application/json; verbose=true

```
{
  "predictions": [
    {
        "predicted_label": 0.0,
        "distances": [3.11792408, 3.89746071, 6.32548437],
        "labels": [0.0, 1.0, 0.0]
    },
    {
        "predicted_label": 2.0,
        "distances": [1.08470316, 3.04917915, 5.25393973],
        "labels": [2.0, 2.0, 0.0]
    }
  ]
}
```

## OUTPUT: RECORDIO\-PROTOBUF<a name="kNN-output-recordio"></a>

content\-type: application/x\-recordio\-protobuf

```
[
    Record = {
        features = {},
        label = {
            'predicted_label': {
                values: [0.0]  # float32
            }
        }
    },
    Record = {
        features = {},
        label = {
            'predicted_label': {
                values: [2.0]  # float32
            }
        }
    }
]
```

## OUTPUT: VERBOSE RECORDIO\-PROTOBUF<a name="kNN-output-verbose-recordio"></a>

In verbose mode, the API provides the search results with the distances vector sorted from smallest to largest, with corresponding elements in the labels vector\. In this example, k is set to 3\.

accept: application/x\-recordio\-protobuf; verbose=true

```
[
    Record = {
        features = {},
        label = {
            'predicted_label': {
                values: [0.0]  # float32
            },
            'distances': {
                values: [3.11792408, 3.89746071, 6.32548437]  # float32
            },
            'labels': {
                values: [0.0, 1.0, 0.0]  # float32
            }
        }
    },
    Record = {
        features = {},
        label = {
            'predicted_label': {
                values: [0.0]  # float32
            },
            'distances': {
                values: [1.08470316, 3.04917915, 5.25393973]  # float32
            },
            'labels': {
                values: [2.0, 2.0, 0.0]  # float32
            }
        }
    }
]
```

## SAMPLE OUTPUT<a name="kNN-sample-output"></a>

For regressor:

```
[06/08/2018 20:15:33 INFO 140026520049408] #test_score (algo-1) : ('mse', 0.013333333333333334)
```

For classifier:

```
[06/08/2018 20:15:46 INFO 140285487171328] #test_score (algo-1) : ('accuracy', 0.98666666666666669)
```