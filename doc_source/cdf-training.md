# Common Data Formats—Training<a name="cdf-training"></a>

To prepare for training, you can preprocess your data using a variety of AWS services, including AWS Glue, Amazon EMR, Amazon Redshift, Amazon Relational Database Service, and Amazon Athena\. After preprocessing, publish the data to an Amazon S3 bucket\. For training, the data need to go through a series of conversions and transformations, including: 

When using Amazon SageMaker in the training portion of the algorithm, make sure to upload all data at once\. If more data is added to that location, a new training call would need to be made to construct a brand new model\.

 For training, it needs to go through a series of conversions and transformations, including: 

+ Training data serialization \(handled by you\) 

+ Training data deserialization \(handled by the algorithm\) 

+ Training model serialization \(handled by the algorithm\) 

+ Trained model deserialization \(optional, handled by you\) 

## Training Data Serialization<a name="td-serialization"></a>

Amazon SageMaker algorithms work best with the optimized protobuf [recordIO]( https://mxnet.incubator.apache.org/architecture/note_data_loading.html#data-format ) format\. In the following code, each observation in the dataset is converted into a binary representation as a set of 4\-byte floats and is then loaded to the protobuf “values” field\. 

**Note**  
 [Step 3\.2\.3: Transform the Training Dataset and Upload It to S3](ex1-preprocess-data-transform.md) shows how to convert a numPy array into the protobuf recordIO format\. 

The schema for the protocol buffers is: 

```
syntax = "proto2";
package AmazonAIAlgorithmsIO;

// A sparse or dense rank-R tensor that stores data as doubles (float64).
message Float32Tensor {
    // Each value in the vector. If keys is empty, this is treated as a
    // dense vector.
    repeated float values = 1 [packed = true];

    // If not empty, then the vector is treated as sparse with
    // each key specifying the location of the value in the sparse vector.
    repeated uint64 keys = 2 [packed = true];

    // Optional shape which allows the vector to represent a matrix.
    // For example, if shape = [ 10, 20 ], then floor(keys[i] / 10) gives the row
    //      and keys[i] %, 20 gives the column.
    // This also supports n-dimensonal tensors.
    // Note: This must be specified if the tensor is sparse.
    repeated uint64 shape = 3 [packed = true];
}

// A sparse or dense rank-R tensor that stores data as doubles (float64).
message Float64Tensor {
    // Each value in the vector. If keys is empty, this is treated as a
    // dense vector.
    repeated double values = 1 [packed = true];

    // If not empty, then the vector is treated as sparse with
    // each key specifying the location of the value in the sparse vector.
    repeated uint64 keys = 2 [packed = true];

    // Optional shape that allows the vector to represent a matrix.
    // For example, if shape = [ 10, 20 ], then floor(keys[i] / 10) gives the row
    //      and keys[i] %, 20 gives the column.
    // This also supports n-dimensonal tensors.
    // Note: This must be specified if the tensor is sparse.
    repeated uint64 shape = 3 [packed = true];
}

// A sparse or dense rank-R tensor that stores data as 32-bit ints (int32).
message Int32Tensor {
    // Each value in the vector. If keys is empty, this is treated as a
    // dense vector.
    repeated int32 values = 1 [packed = true];

    // If not empty, then the vector is treated as sparse with
    // each key specifying the location of the value in the sparse vector.
    repeated uint64 keys = 2 [packed = true];

    // Optional shape that allows the vector to represent a matrix.
    // For example, if shape = [ 10, 20 ], then floor(keys[i] / 10) gives the row
    //      and keys[i] %, 20 gives the column.
    // This also supports n-dimensonal tensors.
    // Note: This must be specified if the tensor is sparse.
    repeated uint64 shape = 3 [packed = true];
}

// Support for storing binary data for parsing in other ways (such as JPEG, etc.).
// This is an example of another type of value, and it might not immediately be supported.
message Bytes {
  repeated byte value          = 1 [packed = true];
  // Stores the content type of the data, if known.
  // This allows the possibility of using decoders for common formats
  // in the future.
  optional string content_type = 2;
}

message Value {
  oneof value {
    // The numbering assumes the possible use of:
    //   - float16, float128
    //   - int8, int16, int64
    Float32Tensor float32_tensor  = 2;
    Float64Tensor float64_tensor  = 3;
    Int32Tensor   int64_tensor    = 7;
    Bytes         bytes           = 9;
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
  map<string, Value> label    = 2;

  // Unique identifier for this record in the dataset.
  //
  // Although it's not necessary, this allows better
  // debugging where there are data issues.
  //
  // This is not used by the algorithm directly.
  optional string uid         = 3;

  // Textual metadata describing the record.
  //
  // This might include JSON-serialized information
  // about the source of the record.
  //
  // This is not used by the algorithm directly.
  optional string metadata    = 4;

  // Optional serialized JSON object that allows per-record
  // hyperparameters, configuration, or other information to be set.
  //
  // The meaning and interpretation of this field is defined by
  // the algorithm author and might not be supported.
  //
  // This is used to pass additional inference configuration
  // when batch inference is used (for example, types of scores to return).
  optional string configuration = 5;
}
```

After creating the protocol buffer is created, store it in an S3 location that is accessible by Amazon SageMaker, and passed as part of `InputDataConfig` in `create_training_job`\. 

**Note**  
For all Amazon SageMaker algorithms, the `ChannelName` in `InputDataConfig` must be set to `train`\. Some algorithms also support a validation `input channel`\. 

## Trained Model Deserialization<a name="td-deserialization"></a>

Amazon SageMaker models are stored as model\.tar\.gz in the S3 bucket specified in `OutputDataConfig` `S3OutputPath` parameter of the `create_training_job` EASE call\. You can specify most of these model artifacts when creating a hosting model\. You can also open and review them in your notebook instance\. When `model.tar.gz` is untarred, it contains `model_algo-1`, which is a serialized Apache MXNet object\. For example, you use the following to load the k\-means model into memory and view it: 

```
import mxnet as mx
print(mx.ndarray.load('model_algo-1'))
```