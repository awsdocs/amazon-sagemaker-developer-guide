# Query Feature Store with Athena and AWS Glue<a name="feature-store-athena-glue-integration"></a>

 After a Feature Store feature group has been created in an offline feature store, you can choose to run queries using Amazon Athena on a AWS Glue catalog\. This requires data to be registered in a data catalog with other catalog details which is auto\-registered for you in Feature Store\. In other words, Feature Store automatically builds an AWS Glue data catalog when feature groups are created and you can turn them off\. This is particularly useful when you want to build a dataset by executing SQL queries and then train a model for inference\.  

 After your `FeatureStore` has been created and populated with your data in the offline store, you have the capability to write SQL queries to join data stored in the offline store from different `FeatureGroups`\. To do this, you can use Amazon Athena to write and execute SQL queries\. You can set up a AWS Glue crawler to run on a schedule to ensure your catalog is always up to date as well\.  

 If you want to do this please define a role which can used by the AWS Glue crawler to access the offline store’s S3 buckets\. For more information, see [Create an IAM role](https://docs.aws.amazon.com/glue/latest/dg/create-an-iam-role.html)\.  

 For more information on how to use AWS Glue and Athena to build a training dataset for model training and inference, see [Build Training Dataset: Create Feature Groups](https://docs.aws.amazon.com/sagemaker/latest/dg/feature-store-create-feature-group.html)\. 

## Sample Athena Queries<a name="feature-store-athena-sample-queries"></a>

 Below we provide some sample queries that act as a template for you to quickly write queries using Athena\.  

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