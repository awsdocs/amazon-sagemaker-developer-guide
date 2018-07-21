# K\-Nearest Neighbors Training Input Data Formats<a name="kNN-in-formats"></a>

## CSV<a name="kNN-training-data-csv"></a>

content\-type: text/csv; label\_size=1

```
4,1.2,1.3,9.6,20.3
```

The first `label_size` columns are interpreted as the label vector for that row\.

## RECORDIO<a name="kNN-training-data-recordio"></a>

content\-type: application/x\-recordio\-protobuf

```
[
    Record = {
        features = {
            'values': {
                values: [1.2, 1.3, 9.6, 20.3]  # float32
            }
        },
        label = {
            'values': {
                values: [4]  # float32
            }
        }
    }
] 

                
}
```