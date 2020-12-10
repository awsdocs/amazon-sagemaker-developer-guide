# Amazon SageMaker Feature Store Offline Store Data Format<a name="feature-store-offline"></a>

 Amazon SageMaker Feature Store offline store data is stored in an Amazon S3 bucket within your account\. When you call `PutRecord`, your data is buffered, batched, and written into Amazon S3 within 15 minutes The only file format supported for the private beta is Parquet\. Each parquet file can contain multiple `Records`\.  

 Files are organized with the following naming convention: 

```
S3://bucket-name/<customer-prefix>/<account-id>/sagemaker/<aws-region>/OfflineStore/<feature-group-name>/event-time-year/
     event-time-month/event-time-day/event-time-hour/<account-id>_sagemaker_<aws_region>_offline-store_<feature-group-name>_<timestamp_of_earliest_event_time_in_file>_<random_stuff>.parquet
```

 For example: 

```
S3://my-bucket/my-prefix/123456789012/sagemaker/us-east-1/offline-store/
     customer-purchase-history-patterns/2020/06/31/00/
     123456789012_sagemaker_us-east-1_offline-store_customer-purchase-history-patterns_20130917T064401Z_108934320012Az11.parquet
```

  The following additional fields are added by Feature Store to each `Record` when they persist in the offline store:  
+  **write\_time** – The timestamp when data was written into the offline store\. Can be used for constructing time\-travel related queries\. 
+  **event\_time** – The timestamp provided in the `PutRecord` API call\. 
+  **is\_deleted** – `False` by default\. If `DeleteRecord` is called, a new `Record` is inserted into `RecordIdentifierValue` and set to `True` in the offline store\. 