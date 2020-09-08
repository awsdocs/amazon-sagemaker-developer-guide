# Common Data Formats for Inference<a name="cdf-inference"></a>

Amazon SageMaker algorithms accept and produce several different MIME types for the http payloads used in retrieving online and mini\-batch predictions\. You can use various AWS services to transform or preprocess records prior to running inference\. At a minimum, you need to convert the data for the following:
+ Inference request serialization \(handled by you\) 
+ Inference request deserialization \(handled by the algorithm\) 
+ Inference response serialization \(handled by the algorithm\) 
+ Inference response deserialization \(handled by you\) 

## Convert Data for Inference Request Serialization<a name="ir-serialization"></a>

Content type options for Amazon SageMaker algorithm inference requests include: `text/csv`, `application/json`, and `application/x-recordio-protobuf`\. Algorithms that don't support all of these types can support other types\. XGBoost, for example, only supports `text/csv` from this list, but also supports `text/libsvm`\.

For `text/csv` the value for the Body argument to `invoke_endpoint` should be a string with commas separating the values for each feature\. For example, a record for a model with four features might look like: `1.5,16.0,14,23.0`\. Any transformations performed on the training data should also be performed on the data before obtaining inference\. The order of the features matters, and must remain unchanged\. 

`application/json` is significantly more flexible and provides multiple possible formats for developers to use in their applications\. At a high level, in JavaScript, the payload might look like: 

```
let request = {
  // Instances might contain multiple rows that predictions are sought for.
  "instances": [
    {
      // Request and algorithm specific inference parameters.
      "configuration": {},
      // Data in the specific format required by the algorithm.
      "data": {
         "<field name>": dataElement
       }
    }
  ]
}
```

You have the following options for specifying the `dataElement`: 

Protocol buffers equivalent:

```
// Has the same format as the protocol buffers implementation described for training.
let dataElement = {
  "keys": [],
  "values": [],
  "shape": []
}
```

Simple numeric vector: 

```
// An array containing numeric values is treated as an instance containing a
// single dense vector.
let dataElement = [1.5, 16.0, 14.0, 23.0]

// It will be converted to the following representation by the SDK.
let converted = {
  "features": {
    "values": dataElement
  }
}
```

And, for multiple records:

```
let request = {
  "instances": [
    // First instance.
    {
      "features": [ 1.5, 16.0, 14.0, 23.0 ]
    },
    // Second instance.
    {
      "features": [ -2.0, 100.2, 15.2, 9.2 ]
    }
  ]
}
```

## Convert Data for Inference Response Deserialization<a name="ir-deserialization"></a>

Amazon SageMaker algorithms return JSON in several layouts\. At a high level, the structure is:

```
let response = {
  "predictions": [{
    // Fields in the response object are defined on a per algorithm-basis.
  }]
}
```

The fields that are included in predictions differ across algorithms\. The following are examples of output for the k\-means algorithm\.

Single\-record inference: 

```
let response = {
  "predictions": [{
    "closest_cluster": 5,
    "distance_to_cluster": 36.5
  }]
}
```

Multi\-record inference: 

```
let response = {
  "predictions": [
    // First instance prediction.
    {
      "closest_cluster": 5,
      "distance_to_cluster": 36.5
    },
    // Second instance prediction.
    {
      "closest_cluster": 2,
      "distance_to_cluster": 90.3
    }
  ]
}
```

Multi\-record inference with protobuf input: 

```
{
  "features": [],
  "label": {
    "closest_cluster": {
      "values": [ 5.0 ] // e.g. the closest centroid/cluster was 1.0
    },
    "distance_to_cluster": {
      "values": [ 36.5 ]
    }
  },
  "uid": "abc123",
  "metadata": "{ "created_at": '2017-06-03' }"
}
```

SageMaker algorithms also support jsonlines format, where the per\-record response content is same as that in JSON format\. The multi\-record structure is a concatenation of per\-record response objects separated by newline characters\. The response content for the built\-in KMeans algorithm for 2 input data points is:

```
{"distance_to_cluster": 23.40593910217285, "closest_cluster": 0.0}
{"distance_to_cluster": 27.250282287597656, "closest_cluster": 0.0}
```

While running batch transform, it is recommended to use `jsonlines` response type by setting the `Accept` field in the `CreateTransformJobRequest` to `application/jsonlines`\.

## Common Request Formats for All Algorithms<a name="common-in-formats"></a>

Most algorithms use several of the following inference request formats\.

### JSON Request Format<a name="cm-json"></a>

Content\-type: application/json

Dense Format

```
let request =   {
    "instances":    [
        {
            "features": [1.5, 16.0, 14.0, 23.0]
        }
    ]
}


let request =   {
    "instances":    [
        {
            "data": {
                "features": {
                    "values": [ 1.5, 16.0, 14.0, 23.0]
                }
            }
        }
    ]
}
```

Sparse Format

```
{
	"instances": [
		{"data": {"features": {
					"keys": [26, 182, 232, 243, 431],
					"shape": [2000],
					"values": [1, 1, 1, 4, 1]
				}
			}
		},
		{"data": {"features": {
					"keys": [0, 182, 232, 243, 431],
					"shape": [2000],
					"values": [13, 1, 1, 4, 1]
				}
			}
		},
	]
}
```

### JSONLINES Request Format<a name="cm-jsonlines"></a>

Content\-type: application/jsonlines

Dense Format

A single record in dense format can be represented as either:

```
{ "features": [1.5, 16.0, 14.0, 23.0] }
```

or:

```
{ "data": { "features": { "values": [ 1.5, 16.0, 14.0, 23.0] } }
```

Sparse Format

A single record in sparse format is represented as:

```
{"data": {"features": { "keys": [26, 182, 232, 243, 431], "shape": [2000], "values": [1, 1, 1, 4, 1] } } }
```

Multiple records are represented as a concatenation of the above single\-record representations, separated by newline characters:

```
{"data": {"features": { "keys": [0, 1, 3], "shape": [4], "values": [1, 4, 1] } } }
{ "data": { "features": { "values": [ 1.5, 16.0, 14.0, 23.0] } }
{ "features": [1.5, 16.0, 14.0, 23.0] }
```

### CSV Request Format<a name="cm-csv"></a>

Content\-type: text/csv;label\_size=0

**Note**  
CSV support is not available for factorization machines\.

### RECORDIO Request Format<a name="cm-recordio"></a>

Content\-type: application/x\-recordio\-protobuf

## Use Batch Transform with Build\-in Algorithms<a name="cm-batch"></a>

While running batch transform, it's recommended to use jsonlines response type instead of JSON, if supported by the algorithm\. This is accomplished by setting the `Accept` field in the `CreateTransformJobRequest` to `application/jsonlines`\.

When you create a transform job, the `SplitType` must be set according to the `ContentType` of the input data\. Similarly, depending on the `Accept` field in the `CreateTransformJobRequest`, `AssembleWith` must be set accordingly\. Please use the following table to help appropriately set these fields:


| ContentType | Recommended SplitType | 
| --- | --- | 
| Accept | Recommended AssembleWith | 
| --- | --- | 
| application/x\-recordio\-protobuf | RecordIO | 
| text/csv | Line | 
| application/jsonlines | Line | 
| application/json | None | 
| application/x\-image | None | 
| image/\* | None | 
| application/x\-recordio\-protobuf | None | 
| application/json | None | 
| application/jsonlines | Line | 

For more information on response formats for specific algorithms, see the following:
+ [PCA Response Formats](PCA-in-formats.md)
+ [Linear learner response formats](LL-in-formats.md)
+ [NTM Response Formats](ntm-in-formats.md)
+ [K\-Means Response Formats](km-in-formats.md)
+ [Factorization Machine Response Formats](fm-in-formats.md)