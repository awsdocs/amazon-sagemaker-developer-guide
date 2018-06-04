# Random Cut Forest<a name="randomcutforest"></a>

Amazon SageMaker Random Cut Forest \(RCF\) is an unsupervised algorithm for detecting anomalous data points within a data set\. These are observations which diverge from otherwise well\-structured or patterned data\. Anomalies can manifest as unexpected spikes in time series data, breaks in periodicity, or unclassifiable data points\. They are easy to describe in that, when viewed in a plot, they are often easily distinguishable from the "regular" data\. Including these anomalies in a data set can drastically increase the complexity of a machine learning task since the "regular" data can often be described with a simple model\.

With each data point, RCF associates an anomaly score\. Low score values indicate that the data point is considered "normal\." High values indicate the presence of an anomaly in the data\. The definitions of "low" and "high" depend on the application but common practice suggests that scores beyond three standard deviations from the mean score are considered anomalous\.

While there are many applications of anomaly detection algorithms to one\-dimensional time series data such as traffic volume analysis or sound volume spike detection, RCF is designed to work with arbitrary\-dimensional input\. Amazon SageMaker RCF scales well with respect to number of features, data set size, and number of instances\.

## Input/Output Interface<a name="rcf-input_output"></a>

Amazon SageMaker Random Cut Forest supports the `train` and `test` data channels\. The optional test channel is used to compute accuracy, precision, recall, and F1\-score metrics on labeled data\. Train and test data content types can be either `application/x-recordio-protobuf` or `text/csv` formats\. For the test data, when using text/csv format, the content must be specified as text/csv;label\_size=1 where the first column of each row represents the anomaly label: "1" for an anomalous data point and "0" for a normal data point\.

Also note that the train channel only supports `S3DataDistributionType=ShardedByS3Key` and the test channel only supports `S3DataDistributionType=FullyReplicated`\. The S3 distribution type can be specified using the Python SDK as follows:

```
  import sagemaker
    
  # specify Random Cut Forest training job information and hyperparameters
  rcf = sagemaker.estimator.Estimator(...)
    
  # explicitly specify "SharededByS3Key" distribution type
  train_data = sagemaker.s3_input(
       s3_data=s3_training_data_location,
       content_type='text/csv;label_size=0',
       distribution='ShardedByS3Key')
    
  # run the training job on input data stored in S3
  rcf.fit({'train': train_data})
```

See the [Amazon SageMaker Data Types documentation](https://docs.aws.amazon.com/sagemaker/latest/dg/API_S3DataSource.html) for more information on customizing the S3 data source attributes\. Finally, in order to take advantage of multi\-instance training the training data must be partitioned into at least as many files as instances\.

For inference, RCF supports `application/x-recordio-protobuf`, `text/csv` and `application/json` input data content types\. See the [ Algorithms Provided by Amazon SageMaker: Common Data FormatsCommon Data Formats  TBD   The following topics explain the data formats for the algorithms provided by Amazon SageMaker\.    Common Data Formats—Training   TBD   To prepare for training, you can preprocess your data using a variety of AWS services, including AWS Glue, Amazon EMR, Amazon Redshift, Amazon Relational Database Service, and Amazon Athena\. After preprocessing, publish the data to an Amazon S3 bucket\. For training, the data need to go through a series of conversions and transformations, including:  When using Amazon SageMaker in the training portion of the algorithm, make sure to upload all data at once\. If more data is added to that location, a new training call would need to be made to construct a brand new model\.  For training, it needs to go through a series of conversions and transformations, including:    Training data serialization \(handled by you\)    Training data deserialization \(handled by the algorithm\)    Training model serialization \(handled by the algorithm\)    Trained model deserialization \(optional, handled by you\)      Training Data Serialization   Many Amazon SageMaker algorithms support training with data in the CSV format\. Please see the individual algorithm pages, or this [page](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-algo-docker-registry-paths.html) for a summary of supported data formats by algorithm\. To use CSVs for training, please specify a [ContentType](https://docs.aws.amazon.com/sagemaker/latest/dg/API_Channel.html#SageMaker-Type-Channel-ContentType) of 'text/csv' in the input data channel specification\. Amazon SageMakerr requires the CSV to not have a header record and assumes the target variable is in the first column, with features in subsequent columns\. This can be changed for unsupervised learning tasks like k\-means, which do not have a target column, by specifying the number of label columns in the content type\. For example, 'text/csv;label\_size=0'\.  Amazon SageMaker algorithms work best with the optimized protobuf [recordIO]( https://mxnet.incubator.apache.org/architecture/note_data_loading.html#data-format ) format\. In the following code, each observation in the dataset is converted into a binary representation as a set of 4\-byte floats and is then loaded to the protobuf “values” field\.    [Step 3\.2\.3: Transform the Training Dataset and Upload It to S3](ex1-preprocess-data-transform.md) shows how to convert a numPy array into the protobuf recordIO format\.   The schema for the protocol buffers is:  

```
syntax = "proto2";
 
 package aialgs.data;
 
 option java_package = "com.amazonaws.aialgorithms.proto";
 option java_outer_classname = "RecordProtos";
 
 // A sparse or dense rank-R tensor that stores data as doubles (float64).
 message Float32Tensor   {
     // Each value in the vector. If keys is empty this is treated as a
     // dense vector.
     repeated float values = 1 [packed = true];
 
     // If not empty then the vector is treated as sparse with
     // each key specifying the location of the value in the sparse vector.
     repeated uint64 keys = 2 [packed = true];
 
     // Optional shape which will allow the vector to represent a matrix.
     // e.g. if shape = [ 10, 20 ] then floor(keys[i] / 10) will give the row
     // and keys[i] % 20 will give the column.
     // This also supports n-dimensonal tensors.
     // NB. this must be specified if the tensor is sparse.
     repeated uint64 shape = 3 [packed = true];
 }
 
 // A sparse or dense rank-R tensor that stores data as doubles (float64).
 message Float64Tensor {
     // Each value in the vector. If keys is empty this is treated as a
     // dense vector.
     repeated double values = 1 [packed = true];
 
     // If not empty then the vector is treated as sparse with
     // each key specifying the location of the value in the sparse vector.
     repeated uint64 keys = 2 [packed = true];
 
     // Optional shape which will allow the vector to represent a matrix.
     // e.g. if shape = [ 10, 20 ] then floor(keys[i] / 10) will give the row
     // and keys[i] % 20 will give the column.
     // This also supports n-dimensonal tensors.
     // NB. this must be specified if the tensor is sparse.
     repeated uint64 shape = 3 [packed = true];
 }
 
 // A sparse or dense rank-R tensor that stores data as 32-bit ints (int32).
 message Int32Tensor {
     // Each value in the vector. If keys is empty this is treated as a
     // dense vector.
     repeated int32 values = 1 [packed = true];
 
     // If not empty then the vector is treated as sparse with
     // each key specifying the location of the value in the sparse vector.
     repeated uint64 keys = 2 [packed = true];
 
     // Optional shape which will allow the vector to represent a matrix.
     // e.g. if shape = [ 10, 20 ] then floor(keys[i] / 10) will give the row
     // and keys[i] % 20 will give the column.
     // This also supports n-dimensonal tensors.
     // NB. this must be specified if the tensor is sparse.
     repeated uint64 shape = 3 [packed = true];
 }
 
 // Support for storing binary data for parsing in other ways (such as JPEG/etc).
 // This is an example of another type of value and may not immediately be supported.
 message Bytes {
     repeated bytes value = 1;
 
     // Stores the content type of the data if known.
     // This will allow the possibility of using decoders for common formats
     // in the future.
     optional string content_type = 2;
 }
 
 message Value {
     oneof value {
         // The numbering assumes the possible use of:
         // - float16, float128
         // - int8, int16, int32
         Float32Tensor float32_tensor = 2;
         Float64Tensor float64_tensor = 3;
         Int32Tensor int32_tensor = 7;
         Bytes bytes = 9;
     }
 }
 
 message Record {
     // Map from the name of the feature to the value.
     //
     // For vectors and libsvm-like datasets,
     // a single feature with the name `values`
     // should be specified.
     map<string, Value> features = 1;
 
     // Optional set of labels for this record.
     // Similar to features field above, the key used for
     // generic scalar / vector labels should ve 'values'
     map<string, Value> label = 2;
 
     // Unique identifier for this record in the dataset.
     //
     // Whilst not necessary, this allows better
     // debugging where there are data issues.
     //
     // This is not used by the algorithm directly.
     optional string uid = 3;
 
     // Textual metadata describing the record.
     //
     // This may include JSON-serialized information
     // about the source of the record.
     //
     // This is not used by the algorithm directly.
     optional string metadata = 4;
 
     // Optional serialized JSON object that allows per-record
     // hyper-parameters/configuration/other information to be set.
     //
     // The meaning/interpretation of this field is defined by
     // the algorithm author and may not be supported.
     //
     // This is used to pass additional inference configuration
     // when batch inference is used (e.g. types of scores to return).
     optional string configuration = 5;
 }
``` After creating the protocol buffer, store it in an S3 location that is accessible by Amazon SageMaker, and passed as part of `InputDataConfig` in `create_training_job`\.   For all Amazon SageMaker algorithms, the `ChannelName` in `InputDataConfig` must be set to `train`\. Some algorithms also support a validation `input channel`\.      Trained Model Deserialization   Amazon SageMaker models are stored as model\.tar\.gz in the S3 bucket specified in `OutputDataConfig` `S3OutputPath` parameter of the `create_training_job` EASE call\. You can specify most of these model artifacts when creating a hosting model\. You can also open and review them in your notebook instance\. When `model.tar.gz` is untarred, it contains `model_algo-1`, which is a serialized Apache MXNet object\. For example, you use the following to load the k\-means model into memory and view it:  

```
import mxnet as mx
print(mx.ndarray.load('model_algo-1'))
```    Common Data Formats—Inference   TBD   Amazon SageMaker algorithms accept and produce several different MIME types for the http payloads used in retrieving online and mini\-batch predictions\. You can use various AWS services to transform or preprocess records prior to running inference\. At a minimum, you need to convert the data for the following:   Inference request serialization \(handled by you\)    Inference request deserialization \(handled by the algorithm\)    Inference response serialization \(handled by the algorithm\)    Inference response deserialization \(handled by you\)      Inference Request Serialization  Content type options for Amazon SageMaker algorithm inference requests include: `text/csv`, `application/json`, and `application/x-recordio-protobuf`\. Algorithms that don't support these types, such as XGBoost, which is incompatible, support other types, such as `text/x-libsvm`\. For `text/csv` the value for the Body argument to `invoke_endpoint` should be a string with commas separating the values for each feature\. For example, a record for a model with four features might look like: `1.5,16.0,14,23.0`\. Any transformations performed on the training data should also be performed on the data before obtaining inference\. The order of the features matters, and must remain unchanged\.  `application/json` is significantly more flexible and provides multiple possible formats for developers to use in their applications\. At a high level, in JavaScript, the payload might look like:  

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
``` You have the following options for specifying the `dataElement`:  Protocol buffers equivalent: 

```
// Has the same format as the protocol buffers implementation described for training.
let dataElement = {
  "keys": [],
  "values": [],
  "shape": []
}
``` Simple numeric vector:  

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
``` And, for multiple records: 

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
```    Inference Response Deserialization  Amazon SageMaker algorithms return JSON in several layouts\. At a high level, the structure is: 

```
let response = {
  "predictions": [{
    // Fields in the response object are defined on a per algorithm-basis.
  }]
}
``` The fields that are included in predictions differ across algorithms\. The following are examples of output for the k\-means algorithm\. Single\-record inference:  

```
let response = {
  "predictions": [{
    "closest_cluster": 5,
    "distance_to_cluster": 36.5
  }]
}
``` Multi\-record inference:  

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
``` Multi\-record inference with protobuf input:  

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
  "metadata": "{ created_at: '2017-06-03' }"
}
```   Common Request Formats for All Algorithms  Most algorithms use several of the following inference request formats\.  JSON  Content\-type: application/json Dense Format 

```
let request =   {
    "instances":    [
        {
            "features": [   1.5,    16.0,   14.0,   23.0    ]
        }
    ]
}


let request =   {
    "instances":    [
        {
            "data": {
                "features": {
                    "values": [   1.5,    16.0,   14.0,   23.0    ]
                }
            }
        }
    ]
}
``` Sparse Format 

```
{
	"instances": [
		{"data": {"features": {
					"keys": [26, 182, 232, 243, 431],
					"shape": [2000],
					"values": [1, 1, 1, 4,1]
				}
			}
		},
		{"data": {"features": {
					"keys": [0, 182, 232, 243, 431],
					"shape": [2000],
					"values": [13, 1, 1, 4,1]
				}
			}
		},
	]
}
```   CSV  Content\-type: text/csv;label\_size=0  CSV support is not available for factorization machines\.    RECORDIO  Content\-type: application/x\-recordio\-protobuf  For more information on response formats for specific algorithms, see the following:   [PCA Response Formats](PCA-in-formats.md)   [Linear Learner Response Formats](LL-in-formats.md)   [NTM Response Formats](ntm-in-formats.md)   [k\-means Response Formats](km-in-formats.md)   [Factorization Machine Response Formats](fm-in-formats.md)     ](sagemaker-algo-common-data-formats.md) documentation for more information\. RCF inference returns `appplication/x-recordio-protobuf` or `application/json` formatted output\. Each record in these output data contains the corresponding anomaly scores for each input data point\. See [Common Data Formats\-\-Inference](https://docs.aws.amazon.com/sagemaker/latest/dg/cdf-inference.html) for more information\.

Please see the [example notebooks](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/random_cut_forest/random_cut_forest.ipynb) for demonstrations of training and inference with different content types\.

## Instance Recommendations<a name="rcf-instance-recommend"></a>

For training, we recommend the `ml.m4`, `ml.c4`, and `ml.c5` instance families\. For inference we recommend using a `ml.c5.xl` instance type in particular, for maximum performance as well as minimized cost per hour of usage\. Although the algorithm could technically run on GPU instance types it does not take advantage of GPU hardware\.

**Topics**
+ [Input/Output Interface](#rcf-input_output)
+ [Instance Recommendations](#rcf-instance-recommend)
+ [How RCF Works](rcf_how-it-works.md)
+ [RCF Hyperparameters](rcf_hyperparameters.md)
+ [RCF Response Formats](rcf-in-formats.md)