# Data Sources and Ingestion<a name="feature-store-ingest-data"></a>

 There are multiple ways to bring your data into Amazon SageMaker Feature Store\. Feature Store offers a single API call for data ingestion called `PutRecord` that enables you to ingest data in batches or from streaming sources\. You can also use Amazon SageMaker Data Wrangler to engineer features and then ingest your features into your Feature Store\.

**Topics**
+ [Stream Ingestion](#feature-store-ingest-data-stream)
+ [Data Wrangler with Feature Store](#feature-store-data-wrangler-integration)
+ [Athena and AWS Glue with Feature Store](#feature-store-athena-glue-integration)

## Stream Ingestion<a name="feature-store-ingest-data-stream"></a>

 You can use streaming sources such as Kafka or Kinesis as a data source where features are extracted from there and directly fed to the online feature store for training, inference or feature creation\. Records can be pushed into the feature store by calling the synchronous `PutRecord` API call\. Since this is a synchronous API call it allows small batches of updates to be pushed in a single API call\. This enables you to maintain high freshness of the feature values and publish values as soon an update is detected\. These are also called *streaming* features\. 

## Data Wrangler with Feature Store<a name="feature-store-data-wrangler-integration"></a>

Data Wrangler is a feature of Studio that provides an end\-to\-end solution to import, prepare, transform, featurize, and analyze data\. Data Wrangler enables you to engineer your features and ingest them into a feature store\.  

 In Studio, after interacting with Data Wrangler, choose the **Export** tab, choose **Export Step**, and the choose **Feature Store**, as shown in the following screenshot\. This exports a Jupyter notebook that has all the source code in it to create a Feature Store feature group that adds yours features from Data Wrangler to an offline or online feature store\. 

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/feature-store-data-sources-and-ingestion.png) 

 After the  feature group has been created, you can also select and join data across multiple feature groups to create new engineered features in Data Wrangler and then export your data set to an S3 bucket\.  

 For more information on how to export to Feature Store, see [Export to SageMaker Feature Store](https://docs.aws.amazon.com/sagemaker/latest/dg/data-wrangler-data-export.html#data-wrangler-data-export-feature-store)\. 

## Athena and AWS Glue with Feature Store<a name="feature-store-athena-glue-integration"></a>

 After a Feature Store feature group has been created in an offline feature store, you can choose to run queries using Amazon Athena on a AWS Glue catalog\. This requires data to be registered in a data catalog with other catalog details which is auto\-registered for you in Feature Store\. In other words, Feature Store automatically builds an AWS Glue data catalog when feature groups are created and you can turn them off\. This is particularly useful when you want to build a dataset by executing SQL queries and then train a model for inference\.  

 After your `FeatureStore` has been created and populated with your data in the offline store, you have the capability to write SQL queries to join data stored in the offline store from different `FeatureGroups`\. To do this, you can use Amazon Athena to write and execute SQL queries\. You can set up a AWS Glue crawler to run on a schedule to ensure your catalog is always up to date as well\.  

 If you want to do this please define a role which can used by the AWS Glue crawler to access the offline store’s S3 buckets\. For more information, see [Create an IAM role](https://docs.aws.amazon.com/glue/latest/dg/create-an-iam-role.html)\.  

 For more information on how to use AWS Glue and Athena to build a training dataset for model training and inference, see [Build Training Dataset: Create Feature Groups](https://docs.aws.amazon.com/sagemaker/latest/dg/feature-store-create-feature-group.html)\. 

### Sample Athena Queries<a name="feature-store-athena-sample-queries"></a>

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