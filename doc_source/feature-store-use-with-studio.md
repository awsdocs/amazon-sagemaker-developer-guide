# Use Amazon SageMaker Feature Store with Amazon SageMaker Studio<a name="feature-store-use-with-studio"></a>

 You can use Studio to create and view details about your feature groups\. 

**Topics**
+ [Create a Feature Group in Studio](#feature-store-create-feature-group-studio)
+ [View Feature Group Details in Studio](#feature-store-view-feature-group-detail-studio)

## Create a Feature Group in Studio<a name="feature-store-create-feature-group-studio"></a>

 The create feature group process in Studio has four steps: enter details, definitions, required features, and tags\. 

 Consider the following options before you start: 
+  If you plan to only use an online store, you just need the schema for your features\. This is your columns and each column’s data type\.  
+  If you plan to use an offline store, you need an S3 bucket URI and a Role ARN\.  
+  If you plan to use encryption, you need a KMS key\. You can use the same one for both the online store and your offline store, or have a unique key for each\. 
+  If you plan to use AWS Glue integration, be prepared to provide a data catalog name, database name, and table name\. 

**To create a feature group in Studio**

1. Sign in to Studio\. For more information, see [Onboard to Amazon SageMaker Studio](gs-studio-onboard.md)\.

1. In the left navigation pane, choose the **Components and registries** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Components_registries.png) \)\.

    ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/Use-Feature-Store-with-Studio-2020-11-29_07-31-36.png)

1.  In the file and resource browser, choose **Feature Store**\. 

    ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/Use-Feature-Store-with-Studio-2020-11-29_07-08-56.png) 

1.  The **Feature Store** tab lists your feature groups\. 

    ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/Use-Feature-Store-with-Studio-2020-11-29_07-11-18.png) 

1. The **Feature Store** tab, choose **Create Feature Group**\. 

1.  Enter the feature group details\. 

    ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/Use-Feature-Store-with-Studio-2020-11-29_07-13-03.png) 

1. \(Optional\) If you’re using Glue, enter the data catalog details\. 

    ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/Use-Feature-Store-with-Studio-2020-11-29_07-13-21.png) 

1. Enter feature definitions\. You have two options for providing a schema for your features: a JSON editor, or a Table editor\.  

    ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/Use-Feature-Store-with-Studio-2020-11-29_07-14-52.png) 

1.  The table editor accepts a column name and a data type\. In a minimal example, you will need at least two for the next step: one for a record identifier and one for a time event feature\. You can have up to 2,500 feature definitions\. 

    ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/Use-Feature-Store-with-Studio-2020-11-29_07-16-37.png) 

1.  Set a record identifier and a time feature to use for this feature group\. 

    ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/Use-Feature-Store-with-Studio-2020-11-29_07-17-13.png) 

1. \(Optional\) Enter tags as key value pairs\. 

    ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/Use-Feature-Store-with-Studio-2020-11-29_07-17-39.png) 

1.  Choose **Create feature group**\. 

    ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/Use-Feature-Store-with-Studio-2020-11-29_07-19-31.png) 

1. In the **Actions** column, choose **Open feature store**\.  

    ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/Use-Feature-Store-with-Studio-2020-11-29_07-20-24.png) 

1. When the feature group is finished being created, it appears in your feature groups list\. Choose the refresh button to refresh the list\. 

## View Feature Group Details in Studio<a name="feature-store-view-feature-group-detail-studio"></a>

 You can view details about your feature groups, and get sample queries to run against your data sources to gather the data for features you have defined\. 

**To view feature group details in Studio**

1.  Double\-click or right\-click a feature group from your list, and then choose **Open feature group detail**\. 

    ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/Use-Feature-Store-with-Studio-2020-11-29_07-21-01.png) 

1. In the details view, you can review your feature group summary, feature definitions, feature group tags, and sample query\.  

    ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/Use-Feature-Store-with-Studio-2020-11-29_08-16-43.png) 

1.  On the **Feature definitions** tab, you can search for features by name\. 

    ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/Use-Feature-Store-with-Studio-2020-11-29_08-16-14.png) 

1.  On the **Feature group tags** tab, you can add and remove tags\. 

    ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/Use-Feature-Store-with-Studio-2020-11-29_08-16-20.png) 

1.  On the **Sample query** tab, you can see a variety of sample queries: interactive exploration, time travel, remove tombstone, and remove duplicates\. 

    ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/Use-Feature-Store-with-Studio-2020-11-29_07-21-22.png) 