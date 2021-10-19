# Amazon SageMaker Feature Store Offline Store Data Format<a name="feature-store-offline"></a>

 Amazon SageMaker Feature Store offline store data is stored in an Amazon S3 bucket within your account\. When you call `PutRecord`, your data is buffered, batched, and written into Amazon S3 within 15 minutes\. Feature Store only supports the Parquet file format\. Specifically, when your data is written to your offline store, the data can only be retrieved from your Amazon S3 bucket in Parquet format\. Each file can contain multiple `Records`\.

 Files are organized with the following naming convention:

```
s3://<bucket-name>/<customer-prefix>/<account-id>/sagemaker/<aws-region>/offline-store/<feature-group-name>-<feature-group-creation-time>/data/year=<event-time-year>/
    month=<event-time-month>/day=<event-time-day>/hour=<event-time-hour>/<timestamp_of_latest_event_time_in_file>_<16-random-alphanumeric-digits>.parquet
```

`Records` in the offline store are partitioned by event time\. All `Records` in your store will have an event time within that day your data was ingested\.

For example: 

```
s3://my-bucket/my-prefix/123456789012/sagemaker/us-east-1/offline-store/
      customer-purchase-history-patterns-1593511200/data/year=2020/month=06/day=31/hour=00/20200631T064401Z_108934320012Az11.parquet
```

 Feature Store also exposes the [OfflineStoreConfig\.S3StorageConfig\.ResolvedOutputS3Uri ](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_S3StorageConfig.html#sagemaker-Type-S3StorageConfig-ResolvedOutputS3Uri) field, which can be found from in the [DescribeFeatureGroup](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeFeatureGroup.html) API call\. This is the S3 path under which the files for the specific feature group are written\.

Example value of `ResolvedOutputS3Uri`:

```
s3://my-bucket/my-prefix/123456789012/sagemaker/us-east-1/offline-store/customer-purchase-history-patterns-1593511200/data
```

The following additional fields are added by Feature Store to each Record when they persist in the offline store: 
+  **api\_invocation\_time** – The timestamp when the service receives the `PutRecord` or `DeleteRecord` call\. If using managed ingestion \(e\.g\. Data Wrangler\), this is the timestamp when data was written into the offline store\.
+  **write\_time** – The timestamp when data was written into the offline store\. Can be used for constructing time\-travel related queries\. 
+  **is\_deleted** – `False` by default\. If `DeleteRecord` is called, a new `Record` is inserted into `RecordIdentifierValue` and set to `True` in the offline store\. 