# Get started with Amazon SageMaker Feature Store<a name="feature-store-getting-started"></a>

 To get started using Amazon SageMaker Feature Store, review the basic concepts, learn how to ingest data for your feature store, and then walk through a Feature Store example\. The following sections explain how to create feature groups, ingest data into the groups, and how to manage security for your feature store\.  

**Topics**
+ [Feature Store Concepts](#feature-store-concepts)
+ [Create Feature Groups](feature-store-create-feature-group.md)
+ [Use Amazon SageMaker Feature Store with Amazon SageMaker Studio](feature-store-use-with-studio.md)

## Feature Store Concepts<a name="feature-store-concepts"></a>

The following list of terms are key to understanding the capabilities of Amazon SageMaker Feature Store:  
+  **Feature store** – Serves as the single source of truth to store, retrieve, remove, track, share, discover, and control access to features\. 
+  **Feature** – A measurable property or characteristic that encapsulates an observed phenomenon\. In the Amazon SageMaker Feature Store API, a feature is an attribute of a record\. You can define a name and type for every feature stored in Feature Store\. Name uniquely identifies a feature within a feature group\. Type identifies the datatype for the values of the feature\. Supported datatypes are: String, Integral and Fractional\.  
+  **Feature group** – A `FeatureGroup` is the main Feature Store resource that contains the metadata for all the data stored in Amazon SageMaker Feature Store\. A feature group is a logical grouping of features, defined in the feature store, to describe records\. A feature group’s definition is composed of a list of feature definitions, a record identifier name, and configurations for its online and offline store\.  
+  **Feature definition** – A `FeatureDefinition` consists of a name and one of the following data types: an Integral, String or Fractional\. A `FeatureGroup` contains a list of feature definitions\.  
+  **Record identifier name** – Each feature group is defined with a record identifier name\. The record identifier name must refer to one of the names of a feature defined in the feature group's feature definitions\. 
+  **Record** – A `Record` is a collection of values for features for a single record identifier value\. A combination of record identifier name and a timestamp uniquely identify a record within a feature group\.  
+  **Event time** – a point in time when a new event occurs that corresponds to the creation or update of a record in a feature group\. All records in the feature group must have a corresponding `Eventtime`\. It can be used to track changes to a record over time\. The online store contains the record corresponding to the last `Eventtime` for a record identifier name, whereas the offline store contains all historic records\. Event time must be in an ISO-8601 string in the format of either yyyy-MM-ddTHH:mm:ssZ or yyyy-MM-ddTHH:mm:ss.SSSZ
+  **Online Store** – the low latency, high availability cache for a feature group that enables real\-time lookup of records\. The online store allows quick access to the latest value for a `Record` via the `GetRecord` API\. A feature group contains an `OnlineStoreConfig` controlling where the data is stored\. 
+  **Offline store** – the `OfflineStore`, stores historical data in your S3 bucket\. It is used when low \(sub\-second\) latency reads are not needed\. For example, when you want to store and serve features for exploration, model training, and batch inference\. A feature group contains an `OfflineStoreConfig` controlling where the data is stored\. 
+  **Ingestion** – The act of populating feature groups in the feature store\. 
