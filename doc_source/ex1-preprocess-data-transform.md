# Step 3\.2\.3: Transform the Training Dataset and Upload It to S3<a name="ex1-preprocess-data-transform"></a>

For efficient model training, transform the dataset from the `numpy.array` format to the `RecordIO protobuf` format\. The `RecordIO protobuf` format is more efficient for all of the algorithms provided by Amazon SageMaker\.

**Important**  
For this and subsequent steps, you can choose to use the high\-level Python library provided by Amazon SageMaker or the low\-level AWS SDK for Python \(Boto\)\. If you're a first\-time user of Amazon SageMaker, we recommend that you follow the code examples for the high\-level Python library\. 

To transform the dataset, choose one of the following options
+ **Use the high\-level Python library provided by Amazon SageMaker**

  If you are using the high\-level Python library, you skip this step and go to the next step\. In the next section, you use the `fit` method for model training, which performs the necessary transformation and upload to S3 before starting a model training job\.
+ **Use the SDK for Python**

  The following code first uses the high\-level Python library function, `write_numpy_to_dense_tensor`, to convert the training data into the protobuf format, which is efficient for model training\. Then the code uses the SDK for Python low\-level API to upload data to S3\. 

  ```
  %%time
  from sagemaker.amazon.common import write_numpy_to_dense_tensor
  import io
  import boto3
  
  bucket = 'bucket-name' # Use the name of your s3 bucket here
  data_key = 'kmeans_lowlevel_example/data'
  data_location = 's3://{}/{}'.format(bucket, data_key)
  print('training data will be uploaded to: {}'.format(data_location))
  
  # Convert the training data into the format required by the SageMaker KMeans algorithm
  buf = io.BytesIO()
  write_numpy_to_dense_tensor(buf, train_set[0], train_set[1])
  buf.seek(0)
  
  boto3.resource('s3').Bucket(bucket).Object(data_key).upload_fileobj(buf)
  ```

**Next Step**  
[Step 3\.3: Train a Model](ex1-train-model.md)