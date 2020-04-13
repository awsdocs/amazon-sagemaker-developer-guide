# Step 4\.3: Transform the Training Dataset and Upload It to Amazon S3<a name="ex1-preprocess-data-transform"></a>

The [XGBoost Algorithm](xgboost.md) expects comma\-separated values \(CSV\) for its training input\. The format of the training dataset is numpy\.array\. Transform the dataset from numpy\.array format to the CSV format\. Then upload it to the Amazon S3 bucket that you created in [Step 1: Create an Amazon S3 Bucket](gs-config-permissions.md)

**To convert the dataset to CSV format and upload it**
+ Type the following code into a cell in your notebook and then run the cell\.

  ```
  %%time
  
  import os
  import boto3
  import re
  import copy
  import time
  import io
  import struct
  from time import gmtime, strftime
  from sagemaker import get_execution_role
  
  role = get_execution_role()
  
  region = boto3.Session().region_name
  
  bucket='myBucket' # Replace with your s3 bucket name
  prefix = 'sagemaker/xgboost-mnist' # Used as part of the path in the bucket where you store data
  
  def convert_data():
      data_partitions = [('train', train_set), ('validation', valid_set), ('test', test_set)]
      for data_partition_name, data_partition in data_partitions:
          print('{}: {} {}'.format(data_partition_name, data_partition[0].shape, data_partition[1].shape))
          labels = [t.tolist() for t in data_partition[1]]
          features = [t.tolist() for t in data_partition[0]]
          
          if data_partition_name != 'test':
              examples = np.insert(features, 0, labels, axis=1)
          else:
              examples = features
          #print(examples[50000,:])
          
          
          np.savetxt('data.csv', examples, delimiter=',')
          
          
          
          key = "{}/{}/examples".format(prefix,data_partition_name)
          url = 's3://{}/{}'.format(bucket, key)
          boto3.Session().resource('s3').Bucket(bucket).Object(key).upload_file('data.csv')
          print('Done writing to {}'.format(url))
          
  convert_data()
  ```

  After it converts the dataset to the CSV format, the code uploads the CSV file to the S3 bucket\. 

**Next Step**  
[Step 5: Train a Model](ex1-train-model.md)