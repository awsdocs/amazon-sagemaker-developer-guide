# Linear Learner Response Formats<a name="LL-in-formats"></a>

## JSON<a name="LL-json"></a>

Binary classification

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

Multiclass classification

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

Regression

```
let response =   {
    "predictions":    [
        {
            "score": 0.4
        } 
    ]
            "predicted_label":  {
                    "values":   [3]
            }
       },
    "uid":  "abc123",
    "metadata": "{created_at: '2017-06-03'}"
   }
]
}
```

## JSONLINES<a name="LL-jsonlines"></a>

Binary classification

```
{"score": 0.4, "predicted_label": 0}
```

Multiclass classification

```
{"score": 0.4, "predicted_label": 0}
```

Regression

```
{"score": 0.4}
```

## RECORDIO<a name="LL-recordio"></a>

Binary classification

```
[
    Record = {
        features = {},
        label = {
            'score’: {
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

Multiclass classification

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

Regression

```
[
    Record = {
        features = {},
        label = {
            'score’: {
                keys: [],
                values: [0.4]  # float32
            }   
        }
    }
]
```