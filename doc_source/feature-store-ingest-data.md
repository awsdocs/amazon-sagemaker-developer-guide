# Data Sources and Ingestion<a name="feature-store-ingest-data"></a>

 There are multiple ways to bring your data into Amazon SageMaker Feature Store\. Feature Store offers a single API call for data ingestion called `PutRecord` that enables you to ingest data in batches or from streaming sources\. You can also use Amazon SageMaker Data Wrangler to engineer features and then ingest your features into your Feature Store\.

**Topics**
+ [Stream Ingestion](#feature-store-ingest-data-stream)
+ [Data Wrangler with Feature Store](#feature-store-data-wrangler-integration)

## Stream Ingestion<a name="feature-store-ingest-data-stream"></a>

 You can use streaming sources such as Kafka or Kinesis as a data source where features are extracted from there and directly fed to the online feature store for training, inference or feature creation\. Records can be pushed into the feature store by calling the synchronous `PutRecord` API call\. Since this is a synchronous API call it allows small batches of updates to be pushed in a single API call\. This enables you to maintain high freshness of the feature values and publish values as soon an update is detected\. These are also called *streaming* features\. 

## Data Wrangler with Feature Store<a name="feature-store-data-wrangler-integration"></a>

Data Wrangler is a feature of Studio that provides an end\-to\-end solution to import, prepare, transform, featurize, and analyze data\. Data Wrangler enables you to engineer your features and ingest them into a feature store\.  

 In Studio, after interacting with Data Wrangler, choose the **Export** tab, choose **Export Step**, and the choose **Feature Store**, as shown in the following screenshot\. This exports a Jupyter notebook that has all the source code in it to create a Feature Store feature group that adds yours features from Data Wrangler to an offline or online feature store\. 

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/feature-store-data-sources-and-ingestion.png) 

 After the  feature group has been created, you can also select and join data across multiple feature groups to create new engineered features in Data Wrangler and then export your data set to an S3 bucket\.  

 For more information on how to export to Feature Store, see [Export to SageMaker Feature Store](https://docs.aws.amazon.com/sagemaker/latest/dg/data-wrangler-data-export.html#data-wrangler-data-export-feature-store)\. 