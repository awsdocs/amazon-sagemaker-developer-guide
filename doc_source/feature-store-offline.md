# Amazon SageMaker Feature Store Offline Store Data Format<a name="feature-store-offline"></a>

Amazon SageMaker Feature Store supports the AWS Glue and Apache Iceberg table formats for the offline store\. You can choose the table format when you’re creating a new feature group\. AWS Glue is the default format\.

 Amazon SageMaker Feature Store offline store data is stored in an Amazon S3 bucket within your account\. When you call `PutRecord`, your data is buffered, batched, and written into Amazon S3 within 15 minutes\. Feature Store only supports the Parquet file format\. Specifically, when your data is written to your offline store, the data can only be retrieved from your Amazon S3 bucket in Parquet format\. Each file can contain multiple `Records`\.

For the Iceberg format, Feature Store saves the table’s metadata in the same Amazon S3 bucket that you’re using to store the offline store data\. You can find it under the `metadata` prefix\.

 Feature Store also exposes the [OfflineStoreConfig\.S3StorageConfig\.ResolvedOutputS3Uri ](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_S3StorageConfig.html#sagemaker-Type-S3StorageConfig-ResolvedOutputS3Uri) field, which can be found from in the [DescribeFeatureGroup](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeFeatureGroup.html) API call\. This is the S3 path under which the files for the specific feature group are written\.

The following additional fields are added by Feature Store to each Record when they persist in the offline store: 
+  **api\_invocation\_time** – The timestamp when the service receives the `PutRecord` or `DeleteRecord` call\. If using managed ingestion \(e\.g\. Data Wrangler\), this is the timestamp when data was written into the offline store\.
+  **write\_time** – The timestamp when data was written into the offline store\. Can be used for constructing time\-travel related queries\. 
+  **is\_deleted** – `False` by default\. If `DeleteRecord` is called, a new `Record` is inserted into `RecordIdentifierValue` and set to `True` in the offline store\. 

The following information shows the organization of a Parquet file using the AWS Glue format:

```
s3://DOC-EXAMPLE-BUCKET/example-prefix-name/111122223333/sagemaker/AWS Region/offline-store/example-feature-group-account-id/data/year=year/month=month/day=day/hour=hour/timestamp_of_latest_event_time_in_file_16-random-alphanumeric-digits.parquet
```

Records in the offline store are partitioned by event time into hourly partitions\. You can’t configure the partitioning scheme\. The following shows an example of the output location of a Parquet file:

```
s3://DOC-EXAMPLE-BUCKET/example-prefix/111122223333/sagemaker/AWS Region/offline-store/customer-purchase-history-patterns-1593511200/data/year=2020/month=06/day=31/hour=00/20200631T064401Z_108934320012Az11.parquet
```

The following shows the organization of the data files saved in the Iceberg table format\.

```
s3://DOC-EXAMPLE-BUCKET/example-prefix/account-id/sagemaker/AWS Region/offline-store/feature-group-name-feature-group-creation-time/data/8-random-alphanumeric-digits/event-time-feature-name_trunc=event-time-year-event-time-month-event-time-day/timestamp-of-latest-event-time-in-file_16-random-alphanumeric-digits.parquet
```

Records in the offline store are partitioned by event time into daily partitions\. You can’t configure the partitioning scheme\. The following shows an example of the output location of a Parquet file where the event time feature name is `EventTime`:

```
s3://DOC-EXAMPLE-BUCKET/example-prefix/sagemaker/AWS Region/offline-store/customer-purchase-history-patterns-1593511200/data/0aec19ca/EventTime_trunc=2022-11-09/20221109T215231Z_yolTtpyuWbkaeGIl.parquet
```

The following shows the example location of a metadata file for data files saved in the Iceberg table format\.

s3://*DOC\-EXAMPLE\-BUCKET*/*example\-prefix*/account\-id/sagemaker/AWS Region/offline\-store/*feature\-group\-name*\-*feature\-group\-creation\-time*/metadata/