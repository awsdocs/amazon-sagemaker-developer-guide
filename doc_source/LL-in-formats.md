# Linear learner response formats<a name="LL-in-formats"></a>

## JSON response formats<a name="LL-json"></a>

All Amazon SageMaker built\-in algorithms adhere to the common input inference format described in [Common Data Formats \- Inference](https://docs.aws.amazon.com/sagemaker/latest/dg/cdf-inference.html)\. The following are the available output formats for the SageMaker linear learner algorithm\.

**Binary Classification**

```
let response =   {
    "predictions":    [
        {
            "score": 0.4,
            "predicted_label": 0
        } 
    ]
}
```

**Multiclass Classification**

```
let response =   {
    "predictions":    [
        {
            "score": [0.1, 0.2, 0.4, 0.3],
            "predicted_label": 2
        } 
    ]
}
```

**Regression**

```
let response =   {
    "predictions":    [
        {
            "score": 0.4
        } 
    ]
}
```

## JSONLINES response formats<a name="LL-jsonlines"></a>

**Binary Classification**

```
{"score": 0.4, "predicted_label": 0}
```

**Multiclass Classification**

```
{"score": [0.1, 0.2, 0.4, 0.3], "predicted_label": 2}
```

**Regression**

```
{"score": 0.4}
```

## RECORDIO response formats<a name="LL-recordio"></a>

**Binary Classification**

```
[
    Record = {
        features = {},
        label = {
            'score': {
                keys: [],
                values: [0.4]  # float32
            },
            'predicted_label': {
                keys: [],
                values: [0.0]  # float32
            }
        }
    }
]
```

**Multiclass Classification**

```
[
    Record = {
    "features": [],
    "label":    {
            "score":  {
                    "values":   [0.1, 0.2, 0.3, 0.4]   
            },
            "predicted_label":  {
                    "values":   [3]
            }
       },
    "uid":  "abc123",
    "metadata": "{created_at: '2017-06-03'}"
   }
]
```

**Regression**

```
[
    Record = {
        features = {},
        label = {
            'score': {
                keys: [],
                values: [0.4]  # float32
            }   
        }
    }
]
```