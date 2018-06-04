# Common Data Formats—Training<a name="cdf-training"></a>

To prepare for training, you can preprocess your data using a variety of AWS services, including AWS Glue, Amazon EMR, Amazon Redshift, Amazon Relational Database Service, and Amazon Athena\. After preprocessing, publish the data to an Amazon S3 bucket\. For training, the data need to go through a series of conversions and transformations, including: 

When using Amazon SageMaker in the training portion of the algorithm, make sure to upload all data at once\. If more data is added to that location, a new training call would need to be made to construct a brand new model\.

 For training, it needs to go through a series of conversions and transformations, including: 
+ Training data serialization \(handled by you\) 
+ Training data deserialization \(handled by the algorithm\) 
+ Training model serialization \(handled by the algorithm\) 
+ Trained model deserialization \(optional, handled by you\) 

## Training Data Serialization<a name="td-serialization"></a>

Many Amazon SageMaker algorithms support training with data in the CSV format\. Please see the individual algorithm pages, or this [page](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-algo-docker-registry-paths.html) for a summary of supported data formats by algorithm\. To use CSVs for training, please specify a [ContentType](https://docs.aws.amazon.com/sagemaker/latest/dg/API_Channel.html#SageMaker-Type-Channel-ContentType) of 'text/csv' in the input data channel specification\. Amazon SageMakerr requires the CSV to not have a header record and assumes the target variable is in the first column, with features in subsequent columns\. This can be changed for unsupervised learning tasks like k\-means, which do not have a target column, by specifying the number of label columns in the content type\. For example, 'text/csv;label\_size=0'\. 

Amazon SageMaker algorithms work best with the optimized protobuf [recordIO]( https://mxnet.incubator.apache.org/architecture/note_data_loading.html#data-format ) format\. In the following code, each observation in the dataset is converted into a binary representation as a set of 4\-byte floats and is then loaded to the protobuf “values” field\. 

**Note**  
 [Step 3\.2\.3: Transform the Training Dataset and Upload It to S3](ex1-preprocess-data-transform.md) shows how to convert a numPy array into the protobuf recordIO format\. 

The schema for the protocol buffers is: 

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
```

After creating the protocol buffer, store it in an S3 location that is accessible by Amazon SageMaker, and passed as part of `InputDataConfig` in `create_training_job`\. 

**Note**  
For all Amazon SageMaker algorithms, the `ChannelName` in `InputDataConfig` must be set to `train`\. Some algorithms also support a validation `input channel`\. 

## Trained Model Deserialization<a name="td-deserialization"></a>

Amazon SageMaker models are stored as model\.tar\.gz in the S3 bucket specified in `OutputDataConfig` `S3OutputPath` parameter of the `create_training_job` EASE call\. You can specify most of these model artifacts when creating a hosting model\. You can also open and review them in your notebook instance\. When `model.tar.gz` is untarred, it contains `model_algo-1`, which is a serialized Apache MXNet object\. For example, you use the following to load the k\-means model into memory and view it: 

```
import mxnet as mx
print(mx.ndarray.load('model_algo-1'))
```