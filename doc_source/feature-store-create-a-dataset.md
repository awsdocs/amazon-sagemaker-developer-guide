# Create a Dataset From Your Feature Groups<a name="feature-store-create-a-dataset"></a>

 After a Feature Store feature group has been created in an offline store, you can choose to use the following methods to get your data:
+ Using the Amazon SageMaker Python SDK
+ Running SQL queries in the Amazon Athena

**Important**  
Feature Store requires data to be registered in a AWS Glue data catalog\. By default, Feature Store automatically builds an AWS Glue data catalog when you create a feature group\.

After you've created feature groups for your offline store and populated them with data, you can create a dataset by running queries or using the SDK to join data stored in the offline store from different feature groups\. You can also join the feature groups to a single pandas dataframe\. You can use Amazon Athena to write and execute SQL queries\.

**Note**  
To make sure that your data is up to date, you can set up a AWS Glue crawler to run on a schedule\.  
 To set up a AWS Glue crawler, specify an IAM role that the crawler is using to access the offline store’s S3 buckets\. For more information, see [Create an IAM role](https://docs.aws.amazon.com/glue/latest/dg/create-an-iam-role.html)\.  
 For more information on how to use AWS Glue and Athena to build a training dataset for model training and inference, see [Create Feature Groups](feature-store-create-feature-group.md)\. 

## Using the Amazon SageMaker Python SDK to get your data from your feature groups<a name="feature-store-dataset-python-sdk"></a>

You can use the [Feature Store APIs](https://sagemaker.readthedocs.io/en/stable/api/prep_data/feature_store.html#dataset-builder) to create a dataset from your feature groups\. Data scientists create ML datasets for training by retrieving ML feature data from one or more feature groups in the offline store\. Use the `create_dataset()` function to create the dataset\. You can use the SDK to do the following:
+ Create a dataset from multiple feature groups\.
+ Create a dataset from the feature groups and a pandas data frame\.

By default, Feature Store doesn't include records that you've deleted from the dataset\. It also doesn't include duplicated records\. A duplicate record has the record ID and timestamp value in the event time column\.

Before you use the SDK to create a dataset, you must start a SageMaker session\. Use the following code to start the session\.

```
region = boto3.Session().region_name
boto_session = boto3.Session(region_name=region)

sagemaker_client = boto_session.client(
    service_name="sagemaker", region_name=region
 )
featurestore_runtime = boto_session.client(
    service_name="sagemaker-featurestore-runtime",region_name=region
)

feature_store_session = Session(
    boto_session=boto_session,
    sagemaker_client=sagemaker_client,
    sagemaker_featurestore_runtime_client=featurestore_runtime,
)

feature_store = FeatureStore(feature_store_session)
```

The following code shows an example of creating a dataset from multiple feature groups\. The following code snippet uses the example feature groups "*base\_fg\_name*", "*first\_fg\_name*", and "*second\_fg\_name*", which may not exist or have the same schema within your Feature Store\. It is recommended to replace these feature groups with feature groups that exist within your Feature Store\. For information on how to create a feature group, see [Step 3: Create feature groups](feature-store-introduction-notebook.md#feature-store-set-up-feature-groups-introduction)\. 

```
s3_bucket_name = "offline-store-sdk-test" 

base_fg_name = "base_fg_name"
base_fg = FeatureGroup(name=base_fg_name, sagemaker_session=feature_store_session)

first_fg_name = "first_fg_name"
first_fg = FeatureGroup(name=first_fg_name, sagemaker_session=feature_store_session)

second_fg_name = "second_fg_name"
second_fg = FeatureGroup(name=second_fg_name, sagemaker_session=feature_store_session)

feature_store = FeatureStore(feature_store_session)
builder = feature_store.create_dataset(
    base=base_fg,
    output_path=f"s3://{DOC-EXAMPLE-BUCKET1}",
).with_feature_group(first_fg
).with_feature_group(second_fg, "base_id", ["base_feature_1"])
```

The following code shows an example of creating a dataset from multiple feature groups and a pandas dataframe\.

```
base_data = [[1, 187512346.0, 123, 128],
             [2, 187512347.0, 168, 258],
             [3, 187512348.0, 125, 184],
             [1, 187512349.0, 195, 206]]
base_data_df = pd.DataFrame(
    base_data, 
    columns=["base_id", "base_time", "base_feature_1", "base_feature_2"]
)

builder = feature_store.create_dataset(
    base=base_data_df, 
    event_time_identifier_feature_name='base_time', 
    record_identifier_feature_name='base_id',
    output_path=f"s3://{s3_bucket_name}"
).with_feature_group(first_fg
).with_feature_group(second_fg, "base_id", ["base_feature_1"])
```

The [Feature Store APIs](https://sagemaker.readthedocs.io/en/stable/api/prep_data/feature_store.html#dataset-builder) provides you with helper methods for the `create_dataset` function\. You can use them to do the following:
+ Create a dataset from multiple feature groups\.
+ Create a dataset from multiple feature groups and a pandas dataframe\.
+ Create a dataset from a single feature group and a pandas dataframe\.
+ Create a dataset using a point in time accurate join where records in the joined feature group follow sequentially\.
+ Create a dataset with the duplicated records, instead of following the default behavior of the function\.
+ Create a dataset with the deleted records, instead of following the default behavior of the function\.
+ Create a dataset for time periods that you specify\.
+ Save the dataset as a CSV file\.
+ Save the dataset as a pandas dataframe\.

The *base* feature group is an important concept for joins\. The base feature group is the feature group that has other feature groups or the pandas dataframe joined to it\. For each dataset

You can add the following optional methods to the `create_dataset` function to configure how you're creating dataset:
+ `with_feature_group` – Performs an inner join between the base feature group and another feature group using the record identifier and the target feature name in the base feature group\. The following provides information about the parameters that you specify:
  + `feature_group` – The feature group that you're joining\.
  + `target_feature_name_in_base` – The name of the feature in the base feature group that you're using as a key in the join\. The record identifier in the other feature groups are the other keys that Feature Store uses in the join\.
  + `included_feature_names` – A list of strings representing the feature names of the base feature group\. You can use the field to specify the features that you want to include in the dataset\.
  + `feature_name_in_target` – Optional string representing the feature in the target feature group that will be compared to the target feature in the base feature group\.
  + `join_comparator` – Optional `JoinComparatorEnum` representing the comparator used when joining the target feature in the base feature group and the feature in the target feature group\. These `JoinComparatorEnum` values can be `GREATER_THAN`, `GREATER_THAN_OR_EQUAL_TO`, `LESS_THAN`, `LESS_THAN_OR_EQUAL_TO`, `NOT_EQUAL_TO` or `EQUALS` by default\.
  + `join_type` – Optional `JoinTypeEnum` representing the type of join between the base and target feature groups\. These `JoinTypeEnum` values can be `LEFT_JOIN`, `RIGHT_JOIN`, `FULL_JOIN`, `CROSS_JOIN` or `INNER_JOIN` by default\.
+ `with_event_time_range` – Creates a dataset using the event time range that you specify\.
+ `as_of` – Creates a dataset up to a timestamp that you specify\. For example, if you specify `datetime(2021, 11, 28, 23, 55, 59, 342380)` as the value, creates a dataset up to November 28th, 2021\.
+ `point_time_accurate_join` – Creates a dataset where all of the event time values of the base feature group is less than all the event time values of the feature group or pandas dataframe that you're joining\.
+ `include_duplicated_records` – Keeps duplicated values in the feature groups\.
+ `include_deleted_records` – Keeps deleted values in the feature groups\.
+ `with_number_of_recent_records_by_record_identifier` – An integer that you specify to determine how many of the most recent records appear in the dataset\.
+ `with_number_of_records_by_record_identifier` – An integer that represents how many records appear in the dataset\.

After you've configured the dataset, you can specify the output using one of the following methods:
+ `to_csv_file` – Saves the dataset as a CSV file\.
+ `to_dataframe` – Saves the dataset as a pandas dataframe\.

You can retrieve data that comes after a specific period in time\. The following code retrieves data after a timestamp\.

```
fg1 = FeatureGroup("example-feature-group-1")
feature_store.create_dataset(
    base=fg1, 
    output_path="s3://example-S3-path"
).with_number_of_records_from_query_results(5).to_csv_file()
```

You can also retrieve data from a specific time period\. You can use the following code to get data for a specific time range:

```
fg1 = FeatureGroup("fg1")
feature_store.create_dataset(
    base=fg1, 
    output_path="example-S3-path"
).with_event_time_range(
    datetime(2021, 11, 28, 23, 55, 59, 342380), 
    datetime(2020, 11, 28, 23, 55, 59, 342380)
).to_csv_file() #example time range specified in datetime functions
```

You might want to join multiple feature groups to a pandas dataframe where the event time values of the feature group happen no later than the event time of the data frame\. Use the following code as a template to help you perform the join\.

```
fg1 = FeatureGroup("fg1")
fg2 = FeatureGroup("fg2")
events = [['2020-02-01T08:30:00Z', 6, 1],
          ['2020-02-02T10:15:30Z', 5, 2],
          ['2020-02-03T13:20:59Z', 1, 3],
          ['2021-01-01T00:00:00Z', 1, 4]]
df = pd.DataFrame(events, columns=['event_time', 'customer-id', 'title-id']) 
feature_store.create_dataset(
    base=df, 
    event_time_identifier_feature_name='event_time', 
    record_identifier_feature_name='customer_id',
    output_path="s3://example-S3-path"
).with_feature_group(fg1, "customer-id"
).with_feature_group(fg2, "title-id"
).point_in_time_accurate_join(
).to_csv_file()
```

You can also retrieve data that comes after a specific period in time\. The following code retrieves data after the time specified by the timestamp in the `as_of` method\.

```
fg1 = FeatureGroup("fg1")
feature_store.create_dataset(
    base=fg1, 
    output_path="s3://example-s3-file-path"
).as_of(datetime(2021, 11, 28, 23, 55, 59, 342380)
).to_csv_file() # example datetime values
```

## Sample Amazon Athena Queries<a name="feature-store-athena-sample-queries"></a>

You can write queries in Amazon Athena to create a dataset from your feature groups\. You can also write queries that create a dataset from feature groups and a single pandas dataframe\.

 **Interactive Exploration** 

 This query selects the first 1000 records\.  

```
SELECT *
FROM <FeatureGroup.DataCatalogConfig.DatabaseName>.<FeatureGroup.DataCatalogConfig.TableName>
LIMIT 1000
```

 **Latest snapshot without duplicates** 

 This query selects the latest non\-duplicate records\. 

```
SELECT *
FROM
    (SELECT *,
         row_number()
        OVER (PARTITION BY <RecordIdentiferFeatureName>
    ORDER BY  <EventTimeFeatureName> desc, Api_Invocation_Time DESC, write_time DESC) AS row_num
    FROM <FeatureGroup.DataCatalogConfig.DatabaseName>.<FeatureGroup.DataCatalogConfig.TableName>)
WHERE row_num = 1;
```

 **Latest snapshot without duplicates and deleted records in the offline store** 

 This query filters out any deleted records and selects non\-duplicate records from the offline store\.  

```
SELECT *
FROM
    (SELECT *,
         row_number()
        OVER (PARTITION BY <RecordIdentiferFeatureName>
    ORDER BY  <EventTimeFeatureName> desc, Api_Invocation_Time DESC, write_time DESC) AS row_num
    FROM <FeatureGroup.DataCatalogConfig.DatabaseName>.<FeatureGroup.DataCatalogConfig.TableName>)
WHERE row_num = 1 and 
NOT is_deleted;
```

 **Time Travel without duplicates and deleted records in the offline store** 

 This query filters out any deleted records and selects non\-duplicate records from a particular point in time\.

```
SELECT *
FROM
    (SELECT *,
         row_number()
        OVER (PARTITION BY <RecordIdentiferFeatureName>
    ORDER BY  <EventTimeFeatureName> desc, Api_Invocation_Time DESC, write_time DESC) AS row_num
    FROM <FeatureGroup.DataCatalogConfig.DatabaseName>.<FeatureGroup.DataCatalogConfig.TableName>
    where <EventTimeFeatureName> <= timestamp '<timestamp>')
    -- replace timestamp '<timestamp>' with just <timestamp>  if EventTimeFeature is of type fractional
WHERE row_num = 1 and
NOT is_deleted
```