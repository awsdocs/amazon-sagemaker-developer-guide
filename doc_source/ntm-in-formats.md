# NTM Response Formats<a name="ntm-in-formats"></a>

## JSON<a name="ntm-json"></a>

```
{
    "predictions":    [
        {"topic_weights": [0.02, 0.1, 0,...]},
        {"topic_weights": [0.25, 0.067, 0,...]}
    ]
}
```

## RECORDIO<a name="ntm-recordio"></a>

```
[
    Record = {
        features = {},
        labels = {
            'topic_weights': {
                keys: [],
                values: [0.25, 0.067, 0, ...]  # float32
            }
        }
    },
    Record = {
        features = {},
        labels = {
            'topic_weights': {
                keys: [],
                values: [0.25, 0.067, 0, ...]  # float32
            }
        }
    }  
]
```