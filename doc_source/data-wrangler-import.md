# Import<a name="data-wrangler-import"></a>

You can use Amazon SageMaker Data Wrangler to import data from the following *data sources*: Amazon Simple Storage Service \(Amazon S3\), Amazon Athena, and Amazon Redshift\. 

Some data sources allow you to add multiple *data connections*:
+ You can connect to multiple Amazon Redshift clusters\. Each cluster becomes a data source\. 
+ You can query any Athena database in your account to import data from that database\.

When you import a dataset from a data source, it appears in your data flow\. Data Wrangler automatically infers the data type of each column in your dataset\. To modify these types, select the **Data types** step and select **Edit data types**\.

When you import data from Athena or Amazon Redshift, the imported data is automatically stored in the default SageMaker S3 bucket for the AWS Region in which you are using Studio\. Additionally, Athena stores data you preview in Data Wrangler in this bucket\. To learn more, see [Imported Data Storage](#data-wrangler-import-storage)\.

**Topics**
+ [Import data from Amazon S3](#data-wrangler-import-s3)
+ [Import data from Athena](#data-wrangler-import-athena)
+ [Import data from Amazon Redshift](#data-wrangler-import-redshift)
+ [Imported Data Storage](#data-wrangler-import-storage)

## Import data from Amazon S3<a name="data-wrangler-import-s3"></a>

Amazon Simple Storage Service \(Amazon S3\) can be used to store and retrieve any amount of data at any time, from anywhere on the web\. You can accomplish these tasks using the AWS Management Console, which is a simple and intuitive web interface, and the Amazon S3 API\. If your dataset is stored locally, we recommend that you add it to an S3 bucket for import into Data Wrangler\. To learn how, see [Uploading an object to a bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/PuttingAnObjectInABucket.html) in the Amazon Simple Storage Service Developer Guide\. 

Data Wrangler uses [S3 Select](http://aws.amazon.com/s3/features/#s3-select) to allow you to preview your Amazon S3 files in Data Wrangler\. You incur standard charges for each file preview\. To learn more about pricing, see the **Requests & data retrievals** tab on [Amazon S3 pricing](http://aws.amazon.com/s3/pricing/)\. 

**Important**  
If you plan to export a data flow and launch a Data Wrangler job, ingest data into a SageMaker feature store, or create a SageMaker pipeline, be aware that these integrations require Amazon S3 input data to be located in the same AWS region used to set up the integration \(run the Jupyter Notebook exported from Data Wrangler\)\. 

You can browse all buckets in your AWS account and import CSV and Parquet files using the Amazon S3 import in Data Wrangler\. 

When you choose a dataset for import, you can rename it, specify the file type, and identify the first row as a header\. 

**To import a dataset into Data Wrangler from Amazon S3:**

1. If you are not currently on the **Import** tab, choose **Import**\.

1. Under **Data Preparation**, choose **Amazon S3** to see the **Import S3 Data Source** view\. 

1. From the table of available S3 buckets, select a bucket and navigate to the dataset you want to import\. 

1. Select the file that you want to import\. You can import CSV and Parquet files\. If your dataset does not have a \.csv or \.parquet extension, select the data type from the **File Type** dropdown list\.

1. If your CSV file has a header, select the check box next to **Add header to table**\.

1. Use the **Preview** table to preview your dataset\. This table shows up to 500 rows\. 

1. In the **Details** pane, verify or change the **Name** and **File Type** for your dataset\. If you add a **Name** that contains spaces, these spaces are replaced with underscores when your dataset is imported\. 

1. **Enable sampling** is selected by default\. If you do not uncheck this box, Data Wrangler will sample and import up to 100 MB of data\. To disable sampling, uncheck this check box\. 

1. Choose **Import dataset**\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/import-s3.png)

## Import data from Athena<a name="data-wrangler-import-athena"></a>

Amazon Athena is an interactive query service that makes it easy to analyze data directly in Amazon S3 using standard SQL\. With a few actions in the AWS Management Console, you can point Athena at your data stored in Amazon S3 and begin using standard SQL to run ad\-hoc queries and get results in seconds\. To learn more, see [What is Amazon Athena?](https://docs.aws.amazon.com/athena/latest/ug/what-is.html) in the Amazon Athena User Guide\. 

You can query Athena databases and import the results in Data Wrangler\. To use this import option, you must create at least one database in Athena\. To learn how, see [Getting Started](https://docs.aws.amazon.com/athena/latest/ug/getting-started.html) in the Amazon Athena User Guide\. 

Note the following about the Athena import option in Data Wrangler:
+ Data Wrangler supports using an Athena primary workgroup\. Other workgroups are not supported\. 
+ Data Wrangler does not support federate queries\.

Data Wrangler uses the default S3 bucket in the same AWS Region in which your Studio instance is located to store Athena query results\. When you import from Athena, Data Wrangler creates a new database in your Athena account named `sagemaker_data_wrangler` if one does not already exist\. Temporary tables are created in this database to move the query output to this S3 bucket\. These tables are deleted after data has been imported, however the database, `sagemaker_data_wrangler`, persists\. To learn more, see [Imported Data Storage](#data-wrangler-import-storage)\.

If you use AWS Lake Formation with Athena, make sure your Lake Formation IAM permissions do not override IAM permissions for the database `sagemaker_data_wrangler`\.



**To import a dataset into Data Wrangler from Athena:**

1. On the import screen, choose **Amazon Athena**\.

1. For **Catalog**, choose **AWSDataCatalog**\.

1. Use the **Database** dropdown list to select the database that you want to query\. When you select a database, you can preview all tables in your database using the **Table**s listed under **Details**\.

1. Enter a query in the code box\.

1. Under **Advanced configuration**, **Enable sampling** is selected by default\. If you do not uncheck this box, Data Wrangler samples and imports approximately 50% of the queried data\. Unselect this check box to disable sampling\. 

1. Enter your query in the query editor and use the **Run** button to run the query\. After a successful query, you can preview your result under the editor\.

1. To import the queried results, select **Import dataset**\.

1. Enter a **Dataset name**\. If you add a **Dataset name** that contains spaces, these spaces are replaced with underscores when your dataset is imported\. 

1. Select **Add**\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/import-athena.png)

## Import data from Amazon Redshift<a name="data-wrangler-import-redshift"></a>

Amazon Redshift is a fully managed, petabyte\-scale data warehouse service in the cloud\. The first step to create a data warehouse is to launch a set of nodes, called an Amazon Redshift cluster\. After you provision your cluster, you can upload your dataset and then perform data analysis queries\. 

You can connect to and query one or more Amazon Redshift clusters in Data Wrangler\. To use this import option, you must create at least one cluster in Amazon Redshift\. To learn how, see [Getting started with Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/gsg/getting-started.html)\. 

Data Wrangler uses the default S3 bucket in the same AWS Region in which your Studio instance is located to store Amazon Redshift query results\. To learn more, see [Imported Data Storage](#data-wrangler-import-storage)\.

If you use the IAM managed policy, `AmazonSageMakerFullAccess`, to grant a role permission to use Data Wrangler in Studio, your **Database User** name must have the prefix `sagemaker_access`\.

Use the following procedures to learn how to add a new cluster\. 

**Note**  
Data Wrangler uses the Amazon Redshift Data API with temporary credentials\. To learn more about this API, refer to [Using the Amazon Redshift Data API](https://docs.aws.amazon.com/redshift/latest/mgmt/data-api.html) in the Amazon Redshift Cluster Management Guide\. 

**To connect to a Amazon Redshift cluster:**

1. Choose **Import**\.

1. Choose **`+`** under **Add data connection**\.

1. Choose **Amazon Redshift**\.

1. Choose **Temporary credentials \(IAM\)** for **Type**\.

1. Enter a **Connection Name**\. This is a name used by Data Wrangler to identify this connection\. 

1. Enter the **Cluster Identifier** to specify to which cluster you want to connect\.

1. Enter the **Database Name** of the database that you want to connect\. 

1. Enter a **Database User** to identify the user you want to use to connect to the database\. 

1. For **UNLOAD IAM Role**, enter the IAM role ARN of the role that the Amazon Redshift cluster should assume to move and write data to Amazon S3\. For more information about this role, see [Authorizing Amazon Redshift to access other AWS services on your behalf](https://docs.aws.amazon.com/redshift/latest/mgmt/authorizing-redshift-service.html) in the Amazon Redshift Cluster Management Guide\. 

1. Choose **Connect**\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/redshift-connection.png)

After your connection is successfully established, it appears as a data source under **Data Import**\. Select this data source to query your database and import data\.

**To query and import data from Redshift:**

1. Select the connection that you want to query from **Data Sources**\.

1. Select a **Schema**\. To learn more about Redshift Schemas, see [Schemas](https://docs.aws.amazon.com/redshift/latest/dg/r_Schemas_and_tables.html) in the Amazon Redshift Database Developer Guide\.

1. Under **Advanced configuration**, **Enable sampling** is selected by default\. If you do not uncheck this box, Data Wrangler samples and imports approximately 50% of the queried data\. Unselect this check box to disable sampling\. 

1. Enter your query in the query editor and use the **Run** button to run the query\. After a successful query, you can preview your result under the editor\.

1. Select **Import dataset** to import the dataset that has been queried\. 

1. Enter a **Dataset name**\. If you add a **Dataset name** that contains spaces, these spaces are replaced with underscores when your dataset is imported\. 

1. Select **Add**\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/import-redshift.png)

## Imported Data Storage<a name="data-wrangler-import-storage"></a>

When you query data from Amazon Athena or Amazon Redshift, the queried dataset is automatically stored in Amazon S3\. Data is stored in the default SageMaker S3 bucket for the AWS Region in which you are using Studio\.

Default S3 buckets have the following naming convention: sagemaker\-*region*\-*account number*\. For example, if your account number is 111122223333 and you are using Studio in us\-east\-1, your imported datasets are stored in sagemaker\-us\-east\-1\-111122223333\. 

 Data Wrangler flows depend on this S3 dataset location, so you should not modify this dataset in Amazon S3 while you are using a dependent flow\. If you do modify this S3 location, and you want to continue using your data flow, you must remove all objects in `trained_parameters` in your \.flow file\. To do this, download the \.flow file from Studio and for each instance of `trained_parameters`, delete all entries\. When you are done, `trained_parameters` should be an empty JSON object:

```
"trained_parameters": {}
```

When you export and use your data flow to process your data, the \.flow file you export refers to this dataset in Amazon S3\. Use the following sections to learn more\. 

### Amazon Redshift Import Storage<a name="data-wrangler-import-storage-redshift"></a>

Data Wrangler stores the datasets that result from your query in a Parquet file in your default SageMaker S3 bucket\. 

This file is stored under the following prefix \(folder\): redshift/*uuid*/data/, where *uuid* is a unique identifier that gets created for each query\. 

For example, if your default bucket is sagemaker\-us\-east\-1\-111122223333, a single dataset queried from Amazon Redshift is located in s3://sagemaker\-us\-east\-1\-111122223333/redshift/*uuid*/data/\.

### Amazon Athena Import Storage<a name="data-wrangler-import-storage-athena"></a>

When you query an Athena database and import a dataset, Data Wrangler stores the dataset, as well as a subset of that dataset, or *preview files*, in Amazon S3\. 

The dataset you import by selecting **Import dataset** is stored in Parquet format in Amazon S3\. 

Preview files are written in CSV format when you select **Run** on the Athena import screen, and contain up to 100 rows from your queried dataset\. 

The dataset you query is located under the prefix \(folder\): athena/*uuid*/data/, where *uuid* is a unique identifier that gets created for each query\. 

For example, if your default bucket is sagemaker\-us\-east\-1\-111122223333, a single dataset queried from Athena is located in s3://sagemaker\-us\-east\-1\-111122223333/redshift/*uuid*/data/*example\_dataset\.parquet*\. 

The subset of the dataset that is stored to preview dataframes in Data Wrangler is stored under the prefix: athena/\.