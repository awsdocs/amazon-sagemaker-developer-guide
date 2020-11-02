# Data Formats for k\-NN Training Input<a name="kNN-in-formats"></a>

All Amazon SageMaker built\-in algorithms adhere to the common input training formats described in [Common Data Formats \- Training](https://docs.aws.amazon.com/sagemaker/latest/dg/cdf-training.html)\. This topic contains a list of the available input formats for the SageMaker k\-nearest\-neighbor algorithm\.

## CSV Data Format<a name="kNN-training-data-csv"></a>

content\-type: text/csv; label\_size=1

```
4,1.2,1.3,9.6,20.3
```

The first `label_size` columns are interpreted as the label vector for that row\.

## RECORDIO Data Format<a name="kNN-training-data-recordio"></a>

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