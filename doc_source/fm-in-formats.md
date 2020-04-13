# Factorization Machine Response Formats<a name="fm-in-formats"></a>

## JSON Response Format<a name="fm-json"></a>

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

Regression

```
let response =   {
    "predictions":    [
        {
            "score": 0.4
        } 
    ]
}
```

## JSONLINES Response Format<a name="fm-jsonlines"></a>

Binary classification

```
{"score": 0.4, "predicted_label": 0}
```

Regression

```
{"score": 0.4}
```

## RECORDIO Response Format<a name="fm-recordio"></a>

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