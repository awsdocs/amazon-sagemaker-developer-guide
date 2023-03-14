# Use Amazon SageMaker Feature Store with Amazon SageMaker Studio<a name="feature-store-use-with-studio"></a>

 You can use Studio to create and view details about your feature groups\.

**Topics**
+ [Create a Feature Group in Studio](#feature-store-create-feature-group-studio)
+ [View Feature Group Details in Studio](#feature-store-view-feature-group-detail-studio)

## Create a Feature Group in Studio<a name="feature-store-create-feature-group-studio"></a>

 The create feature group process has four steps: enter feature group details, feature definitions, required features, and feature group tags\. 

 Consider the following options before you start: 
+  If you plan to only use an online store, you just need the schema for your features\. This is your columns and each column’s data type\.
+  If you plan to use an offline store, you need an S3 bucket URI and a Role ARN\.  
+  If you plan to use encryption, you need a KMS key\. You can use the same one for both the online store and your offline store, or have a unique key for each\.
+  If you plan to use AWS Glue integration, be prepared to provide a data catalog name, database name, and table name\.

**To create a feature group in Studio**

1. Sign in to Studio\. For more information, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.

1. Choose **Studio**\.

1. Choose **Launch app**\.

1. From the dropdown list, select **Studio**\.

1. Choose the Home icon\.

1. Choose **Data**\.

1. Choose **Feature Store**\.

1. Choose **Create feature group**\.

1. Specify the storage configuration for the feature group\. You can also specify a data catalog if you are using AWS Glue\.

   1. \(Optional\) For **Table format**, you can specify **Iceberg** to use the Apache Iceberg table format\. For more information about the Iceberg table format, see [Create Feature Groups](feature-store-create-feature-group.md)\.

1. Choose **Continue**\.

1. Enter your feature definitions, then choose **Continue**\.

   You have two options for providing a schema for your features: a JSON editor, or a Table editor\. The table editor accepts a column name and a data type\. In a minimal example, you will need at least two for the next step: one for a record identifier and one for a time event feature\. You can have up to 2,500 feature definitions\. 

    ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/feature-store-group-feature-def.png) 

1. Set a record identifier and a time feature to use for this feature group, then choose **Continue**\.

    ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/feature-store-group-required-features.png) 

1. \(Optional\) Enter tags as key value pairs, then choose **Continue**\. 

    ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/feature-store-group-tags.png) 

1. Review the feature group information and choose **Create feature group**\.

   When the feature group is successfully created, it appears in your feature groups list\.

## View Feature Group Details in Studio<a name="feature-store-view-feature-group-detail-studio"></a>

You can view details of your feature groups, and get sample queries to run against your data sources to gather the data for features you have defined\.

**To view feature group details in Studio**

1.  From the **Amazon SageMaker Feature Store** landing page, in the **Feature group catalog**, choose your feature group name from the list\. This will open the feature group page\.

1. On the **Details** tab, you can review your feature group information and tags\. Choose the **Add new tag** to add new tags or **remove button** to remove a tag\. 

1. On the **Features** tab, you can find a list of all features\. Use the filter to refine your list\. Choose a feature to view its details\.