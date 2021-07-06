# Create, Store, and Share Features with Amazon SageMaker Feature Store<a name="feature-store"></a>

The machine learning \(ML\) development process often begins with extracting data signals also known as *features* from data to train ML models\. Amazon SageMaker Feature Store makes it easy for data scientists, machine learning engineers, and general practitioners to create, share, and manage features for machine learning \(ML\) development\. Feature Store accelerates this process by reducing repetitive data processing and curation work required to convert raw data into features for training an ML algorithm\.

Further, the processing logic for your data is authored only once, and features generated are used for both training and inference, reducing the training\-serving skew\. Feature Store is a centralized store for features and associated metadata so features can be easily discovered and reused\. You can create an online or an offline store\. The online store is used for low latency real\-time inference use cases, and the offline store is used for training and batch inference\.   

The following diagram shows how you can use Amazon SageMaker Feature Store as part of your machine learning pipeline\. First, you read in your raw data and process it\. You can ingest data via streaming to the online and offline store, or in batches directly to the offline store\. You first create a `FeatureGroup`  and configure it to an online or offline store, or both\. Then, you can ingest data into your `FeatureGroup` and store it in your store\. A `FeatureGroup`  is a group of features that is defined via a schema in Feature Store to describe a record\.

 Online store is primarily designed for supporting real\-time predictions that need low millisecond latency reads and high throughput writes\. Offline store is primarily intended for batch predictions and model training\. Offline store is an append only store and can be used to store and access historical feature data\. The offline store can help you store and serve features for exploration and model training\. The online store retains only the latest feature data\. Feature group definitions are immutable after they are created\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/feature-store-overview.png)

## How Feature Store Works<a name="how-feature-store-works"></a>

In Feature Store, features are stored in a collection called a *feature group*\. You can visualize a feature group as a table in which each column is a feature, with a unique identifier for each row\. In principle, a feature group is composed of features and values specific to each feature\. A `Record` is a collection of values for features that correspond to a unique `RecordIdentifier`\. Altogether, a `FeatureGroup` is a group of features defined in your `FeatureStore` to describe a `Record`\.  

 You can use Feature Store in the following modes:  
+  **Online** – In online mode, features are read with low latency \(milliseconds\) reads and used for high throughput predictions\. This mode requires a feature group to be stored in an online store\.  
+  **Offline** – In offline mode, large streams of data are fed to an offline store, which can be used for training and batch inference\. This mode requires a feature group to be stored in an offline store\. The offline store uses your S3 bucket for storage and can also fetch data using Athena queries\.  
+  **Online and Offline** – This includes both online and offline modes\. 

You can ingest data into feature groups in Feature Store in two ways: streaming or in batches\. When you ingest data through streaming, a collection of records are pushed to Feature Store by calling a synchronous `PutRecord` API call\. This API enables you to maintain the latest feature values in Feature Store and to push new feature values as soon an update is detected\. 

Alternatively, Feature Store can process and ingest data in batches\. You can author features using Amazon SageMaker Data Wrangler, create feature groups in Feature Store and ingest features in batches using a SageMaker Processing job with a notebook exported from Data Wrangler\. This mode allows for batch ingestion into the offline store\. It also supports ingestion into the online store if the feature group is configured for both online and offline use\.  

## Create Feature Groups<a name="create-feature-groups"></a>

To ingest features into Feature Store, you must first define the feature group and the feature definitions \(feature name and data type\) for all features that belong to the feature group\. After they are created, feature groups are immutable\. Feature group names are unique within an AWS Region and AWS account\. When creating a feature group, you can also create the metadata for the feature group, such as a short description, storage configuration, features for identifying each record, and the event time, as well as tags to store information such as the author, data source, version, and more\. 

## Find, Discover, and Share Features<a name="Find-discover-share-features"></a>

After you create a feature group in Feature Store, other authorized users of the feature store can share and discover it\. Users can browse through a list of all feature groups in Feature Store or discover existing feature groups by searching by feature group name, description, record identifier name, creation date, and tags\.  

## Real\-Time Inference for Features Stored in the Online Store <a name="real-time-inference"></a>

With Feature Store, you can enrich your features stored in the online store in real time with data from a streaming source \(clean stream data from another application\) and serve the features with low millisecond latency for real\-time inference\.  

You can also perform joins across different `FeatureGroups` for real\-time inference by querying two different `FeatureGroups` in the client application\.  

## Offline Store for Model Training and Batch Inference<a name="offline-store-for-model-training"></a>

Feature Store provides offline storage for feature values in your S3 bucket\. Your data is stored in your S3 bucket using a prefixing scheme based on event time\. The offline store is an append\-only store, enabling Feature Store to maintain a historical record of all feature values\. Data is stored in the offline store in Parquet format for optimized storage and query access\.

You can query, explore, and visualize features using Data Wrangler from Amazon SageMaker Studio\.  Feature Store supports combining data to produce, train, validate, and test data sets, and allows you to extract data at different points in time\. 

## Feature Data Ingestion<a name="feature-data-ingestion"></a>

Feature generation pipelines can be created to process large batches \(1 million rows of data or more\) or small batches, and to write feature data to the offline or online store\. Streaming sources such as Amazon Managed Streaming for Apache Kafka or Amazon Kinesis can also be used as data sources from which features are extracted and directly fed to the online store for training, inference, or feature creation\.  

You can push records to Feature Store by calling the synchronous `PutRecord` API call\. Since this is a synchronous API call, it allows small batches of updates to be pushed in a single API call\. This enables you to maintain high freshness of the feature values and publish values as soon as an update is detected\. These are also called *streaming features*\. 

When feature data is ingested and updated, Feature Store stores historical data for all features in the offline store\. For batch ingest, you can pull feature values from your S3 bucket or use Athena to query\. You can also use Data Wrangler to process and engineer new features that can then be exported to a chosen S3 bucket to be accessed by Feature Store\. For batch ingestion, you can configure a processing job to batch ingest your data into Feature Store, or you can pull feature values from your S3 bucket using Athena\.  

## <a name="w1855aac25c25"></a>