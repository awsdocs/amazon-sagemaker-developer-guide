# Import<a name="data-wrangler-import"></a>

You can use Amazon SageMaker Data Wrangler to import data from the following *data sources*: Amazon Simple Storage Service \(Amazon S3\), Amazon Athena, Amazon Redshift, and Snowflake\. The dataset that you import can include up to 1000 columns\.

**Topics**
+ [Import data from Amazon S3](#data-wrangler-import-s3)
+ [Import data from Athena](#data-wrangler-import-athena)
+ [Import data from Amazon Redshift](#data-wrangler-import-redshift)
+ [Import data from Amazon EMR](#data-wrangler-emr)
+ [Import data from Databricks \(JDBC\)](#data-wrangler-databricks)
+ [Import data from Snowflake](#data-wrangler-snowflake)
+ [Import Data From Software as a Service \(SaaS\) Platforms](#data-wrangler-import-saas)
+ [Imported Data Storage](#data-wrangler-import-storage)

Some data sources allow you to add multiple *data connections*:
+ You can connect to multiple Amazon Redshift clusters\. Each cluster becomes a data source\. 
+ You can query any Athena database in your account to import data from that database\.



When you import a dataset from a data source, it appears in your data flow\. Data Wrangler automatically infers the data type of each column in your dataset\. To modify these types, select the **Data types** step and select **Edit data types**\.

When you import data from Athena or Amazon Redshift, the imported data is automatically stored in the default SageMaker S3 bucket for the AWS Region in which you are using Studio\. Additionally, Athena stores data you preview in Data Wrangler in this bucket\. To learn more, see [Imported Data Storage](#data-wrangler-import-storage)\.

**Important**  
The default Amazon S3 bucket may not have the least permissive security settings, such as bucket policy and server\-side encryption \(SSE\)\. We strongly recommend that you [ Add a Bucket Policy To Restrict Access to Datasets Imported to Data Wrangler](https://docs.aws.amazon.com/sagemaker/latest/dg/data-wrangler-security.html#data-wrangler-security-bucket-policy)\. 

**Important**  
In addition, if you use the managed policy for SageMaker, we strongly recommend that you scope it down to the most restrictive policy that allows you to perform your use case\. For more information, see [Grant an IAM Role Permission to Use Data Wrangler](data-wrangler-security.md#data-wrangler-security-iam-policy)\.

All data sources except for Amazon Simple Storage Service \(Amazon S3\) require you to specify a SQL query to import your data\. For each query, you must specify the following:
+ **Data catalog**
+ **Database**
+ **Table**

You can specify the name of the database or the data catalog in either the drop down menus or within the query\. The following are example queries:
+ `select * from example-data-catalog-name.example-database-name.example-table-name` – The query doesn't use anything specified in the dropdown menus of the user\-interface \(UI\) to run\. It queries `example-table-name` within `example-database-name` within `example-data-catalog-name`\.
+ `select * from example-database-name.example-table-name` – The query uses the data catalog that you've specified in the **Data catalog** dropdown menu to run\. It queries `example-table-name` within `example-database-name` within the data catalog that you've specified\.
+ `select * from example-table-name` – The query requires you to select fields for both the **Data catalog** and **Database name** dropdown menus\. It queries `example-table-name` within the data catalog within the database and data catalog that you've specified\.

The link between Data Wrangler and the data source is a *connection*\. You use the connection to import data from your data source\.

There are the following types of connections:
+ Direct
+ Cataloged

Data Wrangler always has access to the most recent data in a direct connection\. If the data in the data source has been updated, you can use the connection to import the data\. For example, if someone adds a file to one of your Amazon S3 buckets, you can import the file\.

A cataloged connection is the result of a data transfer\. The data in the cataloged connection doesn't necessarily have the most recent data\. For example, you might set up a data transfer between Salesforce and Amazon S3\. If there's an update to the Salesforce data, you must transfer the data again\. You can automate the process of transferring data\. For more information about data transfers, see [Import Data From Software as a Service \(SaaS\) Platforms](#data-wrangler-import-saas)\.

## Import data from Amazon S3<a name="data-wrangler-import-s3"></a>

You can use Amazon Simple Storage Service \(Amazon S3\) to store and retrieve any amount of data, at any time, from anywhere on the web\. You can accomplish these tasks using the AWS Management Console, which is a simple and intuitive web interface, and the Amazon S3 API\. If you've stored your dataset locally, we recommend that you add it to an S3 bucket for import into Data Wrangler\. To learn how, see [Uploading an object to a bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/PuttingAnObjectInABucket.html) in the Amazon Simple Storage Service User Guide\. 

Data Wrangler uses [S3 Select](http://aws.amazon.com/s3/features/#s3-select) to allow you to preview your Amazon S3 files in Data Wrangler\. You incur standard charges for each file preview\. To learn more about pricing, see the **Requests & data retrievals** tab on [Amazon S3 pricing](http://aws.amazon.com/s3/pricing/)\. 

**Important**  
If you plan to export a data flow and launch a Data Wrangler job, ingest data into a SageMaker feature store, or create a SageMaker pipeline, be aware that these integrations require Amazon S3 input data to be located in the same AWS region\.

**Important**  
If you're importing a CSV file, make sure it meets the following requirements:  
A record in your dataset can't be longer than one line\.
A backslash, `\`, is the only valid escape character\.
Your dataset must use one of the following delimiters:  
Comma – `,`
Colon – `:`
Semicolon – `;`
Pipe – `|`
Tab – `[TAB]`
To save space, you can import compressed CSV files\.

Data Wrangler gives you the ability to either import the entire dataset or sample a portion of it\. For Amazon S3, it provides the following sampling options:
+ None – Import the entire dataset\.
+ First K – Sample the first K rows of the dataset, where K is an integer that you specify\.
+ Randomized – Takes a random sample of a size that you specify\.
+ Stratified – Takes a stratified random sample\. A stratified sample preserves the ratio of values in a column\.

After you've imported your data, you can also use the sampling transformer to take one or more samples from your entire dataset\. For more information about the sampling transformer, see [Sampling](data-wrangler-transform.md#data-wrangler-transform-sampling)\.

You can import either a single file or multiple files as a dataset\. You can use the multifile import operation when you have a dataset that is partitioned into separate files\. It takes all of the files from an Amazon S3 directory and imports them as a single dataset\. For information on the types of files that you can import and how to import them, see the following sections\.

------
#### [ Single File Import ]

You can import single files in the following formats:
+ Comma Separated Values \(CSV\)
+ Parquet
+ Javascript Object Notation \(JSON\)
+ Optimized Row Columnar \(ORC\)

For files formatted in JSON, Data Wrangler supports both JSON lines \(\.jsonl\) and JSON documents \(\.json\)\. When you preview your data, it automatically shows the JSON in tabular format\. For nested JSON documents that are larger than 5 MB, Data Wrangler shows the schema for the structure and the arrays as values in the dataset\. Use the **Flatten structured** and **Explode array** operators to display the nested values in tabular format\. For more information, see [Unnest JSON Data](data-wrangler-transform.md#data-wrangler-transform-flatten-column) and [Explode Array](data-wrangler-transform.md#data-wrangler-transform-explode-array)\.

When you choose a dataset, you can rename it, specify the file type, and identify the first row as a header\.

You can import a dataset that you've partitioned into multiple files in an Amazon S3 bucket in a single import step\.

**To import a dataset into Data Wrangler from a single file that you've stored in Amazon S3:**

1. If you are not currently on the **Import** tab, choose **Import**\.

1. Under **Available**, choose **Amazon S3** to see the **Import S3 Data Source** view\. 

1. From the table of available S3 buckets, select a bucket and navigate to the dataset you want to import\. 

1. Select the file that you want to import\. If your dataset does not have a \.csv or \.parquet extension, select the data type from the **File Type** dropdown list\.

1. If your CSV file has a header, select the checkbox next to **Add header to table**\.

1. Use the **Preview** table to preview your dataset\. This table shows up to 100 rows\. 

1. In the **Details** pane, verify or change the **Name** and **File Type** for your dataset\. If you add a **Name** that contains spaces, these spaces are replaced with underscores when your dataset is imported\. 

1. Specify the sampling configuration that you'd like to use\. 

1. Choose **Import dataset**\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/import-s3.png)

------
#### [ Multifile Import ]

The following are the requirements for importing multiple files:
+ The files must be in the same folder of your Amazon S3 bucket\.
+ The files must either share the same header or have no header\.

Each file must be in one of the following formats:
+ CSV
+ Parquet
+ Optimized Row Columnar \(ORC\)
+ JSON

Use the following procedure to import multiple files\.

**To import a dataset into Data Wrangler from multiple files that you've stored in an Amazon S3 directory**

1. If you are not currently on the **Import** tab, choose **Import**\.

1. Under **Available**, choose **Amazon S3** to see the **Import S3 Data Source** view\. 

1. From the table of available S3 buckets, select the bucket containing the folder that you want to import\.

1. Select the folder containing the files that you want to import\. Each file must be in one of the supported formats\. Your files must be the same data type\.

1. If your folder contains CSV files with headers, select the checkbox next to **First row is header**\.

1. If your files are nested within other folders, select the checkbox next to **Include nested directories**\.

1. \(Optional\) Choose **Add filename column** add a column to the dataset that shows the filename for each observation\.

1. \(Optional\) By default, Data Wrangler doesn't show you a preview of a folder\. You can activate previewing by choosing the blue **Preview off** button\. A preview shows the first 10 rows of the first 10 files in the folder\. The following images show you how to activate a preview for a dataset created from nested directories\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/multifile-nested-preview-off-cropped.png)  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/multifile-nested-preview-on-cropped.png)

1. In the **Details** pane, verify or change the **Name** and **File Type** for your dataset\. If you add a **Name** that contains spaces, these spaces are replaced with underscores when your dataset is imported\. 

1. Specify the sampling configuration that you'd like to use\. 

1. Choose **Import dataset**\.

------

You can use parameters to quickly change the data that you're importing into a Data Wrangler flow\. For more information, see [Reusing Data Flows for Different Datasets](data-wrangler-parameterize.md)\.

## Import data from Athena<a name="data-wrangler-import-athena"></a>

Use Amazon Athena to import your data from Amazon Simple Storage Service \(Amazon S3\) into Data Wrangler\. In Athena, you write standard SQL queries to select the data that you're importing from Amazon S3\. For more information, see [What is Amazon Athena?](https://docs.aws.amazon.com/athena/latest/ug/what-is.html)

You can use the AWS Management Console to set up Amazon Athena\. You must create at least one database in Athena before you start running queries\. For more information about getting started with Athena, see [Getting started](https://docs.aws.amazon.com/athena/latest/ug/getting-started.html)\.

Athena is directly integrated with Data Wrangler\. You can write Athena queries without having to leave the Data Wrangler UI\.

In addition to writing simple Athena queries in Data Wrangler, you can also use:
+ Athena workgroups for query result management\. For more information about workgroups, see [Managing query results](#data-wrangler-import-manage-results)\.
+ Lifecycle configurations for setting data retention periods\. For more information about data retention, see [Setting data retention periods](#data-wrangler-import-athena-retention)\.

### Query Athena within Data Wrangler<a name="data-wrangler-import-athena-query"></a>

**Note**  
Data Wrangler does not support federated queries\.

If you use AWS Lake Formation with Athena, make sure your Lake Formation IAM permissions do not override IAM permissions for the database `sagemaker_data_wrangler`\.

Data Wrangler gives you the ability to either import the entire dataset or sample a portion of it\. For Athena, it provides the following sampling options:
+ None – Import the entire dataset\.
+ First K – Sample the first K rows of the dataset, where K is an integer that you specify\.
+ Randomized – Takes a random sample of a size that you specify\.
+ Stratified – Takes a stratified random sample\. A stratified sample preserves the ratio of values in a column\.

The following procedure shows how to import a dataset from Athena into Data Wrangler\.

**To import a dataset into Data Wrangler from Athena**

1. Sign into [Amazon SageMaker Console](https://console.aws.amazon.com/sagemaker)\.

1. Choose **Studio**\.

1. Choose **Launch app**\.

1. From the dropdown list, select **Studio**\.

1. Choose the Home icon\.

1. Choose **Data**\.

1. Choose **Data Wrangler**\.

1. Choose **Import data**\.

1. Under **Available**, choose **Amazon Athena**\.

1. For **Data Catalog**, choose a data catalog\.

1. Use the **Database** dropdown list to select the database that you want to query\. When you select a database, you can preview all tables in your database using the **Tables** listed under **Details**\.

1. \(Optional\) Choose **Advanced configuration**\.

   1. Choose a **Workgroup**\.

   1. If your workgroup hasn't enforced the Amazon S3 output location or if you don't use a workgroup, specify a value for **Amazon S3 location of query results**\.

   1. \(Optional\) For **Data retention period**, select the checkbox to set a data retention period and specify the number of days to store the data before it's deleted\.

   1. \(Optional\) By default, Data Wrangler saves the connection\. You can choose to deselect the checkbox and not save the connection\.

1. For **Sampling**, choose a sampling method\. Choose **None** to turn off sampling\.

1. Enter your query in the query editor and use the **Run** button to run the query\. After a successful query, you can preview your result under the editor\.
**Note**  
Salesforce data uses the `timestamptz` type\. If you're querying the timestamp column that you've imported to Athena from Salesforce, cast the data in the column to the `timestamp` type\. The following query casts the timestamp column to the correct type\.  

   ```
   # cast column timestamptz_col as timestamp type, and name it as timestamp_col
   select cast(timestamptz_col as timestamp) as timestamp_col from table
   ```

1. To import the results of your query, select **Import**\.

After you complete the preceding procedure, the dataset that you've queried and imported appears in the Data Wrangler flow\.

By default, Data Wrangler saves the connection settings as a new connection\. When you import your data, the query that you've already specified appears as a new connection\. The saved connections store information about the Athena workgroups and Amazon S3 buckets that you're using\. When you're connecting to the data source again, you can choose the saved connection\.

### Managing query results<a name="data-wrangler-import-manage-results"></a>

Data Wrangler supports using Athena workgroups to manage the query results within an AWS account\. You can specify an Amazon S3 output location for each workgroup\. You can also specify whether the output of the query can go to different Amazon S3 locations\. For more information, see [Using Workgroups to Control Query Access and Costs](https://docs.aws.amazon.com/athena/latest/ug/manage-queries-control-costs-with-workgroups.html)\.

Your workgroup might be configured to enforce the Amazon S3 query output location\. You can't change the output location of the query results for those workgroups\.

If you don't use a workgroup or specify an output location for your queries, Data Wrangler uses the default Amazon S3 bucket in the same AWS Region in which your Studio instance is located to store Athena query results\. It creates temporary tables in this database to move the query output to this Amazon S3 bucket\. It deletes these tables after data has been imported; however the database, `sagemaker_data_wrangler`, persists\. To learn more, see [Imported Data Storage](#data-wrangler-import-storage)\.

To use Athena workgroups, set up the IAM policy that gives access to workgroups\. If you're using a `SageMaker-Execution-Role`, we recommend adding the policy to the role\. For more information about IAM policies for workgroups, see [IAM policies for accessing workgroups](https://docs.aws.amazon.com/athena/latest/ug/workgroups-iam-policy.html)\. For example workgroup policies, see [Workgroup example policies](https://docs.aws.amazon.com/athena/latest/ug/example-policies-workgroup.html)\.

### Setting data retention periods<a name="data-wrangler-import-athena-retention"></a>

Data Wrangler automatically sets a data retention period for the query results\. The results are deleted after the length of the retention period\. For example, the default retention period is five days\. The results of the query are deleted after five days\. This configuration is designed to help you clean up data that you're no longer using\. Cleaning up your data prevents unauthorized users from gaining access\. It also helps control the costs of storing your data on Amazon S3\.

If you don't set a retention period, the Amazon S3 lifecycle configuration determines the duration that the objects are stored\. The data retention policy that you've specified for the lifecycle configuration removes any query results that are older than the Lifecycle configuration that you've specified\. For more information, see [Setting lifecycle configuration on a bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/how-to-set-lifecycle-configuration-intro.html)\.

Data Wrangler uses S3 lifecycle configurations to manage data retention and expiration\. You must give your Amazon SageMaker Studio IAM execution role permissions to manage bucket lifecycle configurations\. Use the following procedure to give permissions\.

To give permissions to manage the lifecycle configuration do the following\.

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. Choose **Roles**\.

1. In the search bar, specify the Amazon SageMaker execution role that Amazon SageMaker Studio is using\.

1. Choose the role\.

1. Choose **Add permissions**\.

1. Choose **Create inline policy**\.

1. For **Service**, specify **S3** and choose it\.

1. Under the **Read** section, choose **GetLifecycleConfiguration**\.

1. Under the **Write** section, choose **PutLifecycleConfiguration**\.

1. For **Resources**, choose **Specific**\.

1. For **Actions**, select the arrow icon next to **Permissions management**\.

1. Choose **PutResourcePolicy**\.

1. For **Resources**, choose **Specific**\.

1. Choose the checkbox next to **Any in this account**\.

1. Choose **Review policy**\.

1. For **Name**, specify a name\.

1. Choose **Create policy**\.

## Import data from Amazon Redshift<a name="data-wrangler-import-redshift"></a>

Amazon Redshift is a fully managed, petabyte\-scale data warehouse service in the cloud\. The first step to create a data warehouse is to launch a set of nodes, called an Amazon Redshift cluster\. After you provision your cluster, you can upload your dataset and then perform data analysis queries\. 

You can connect to and query one or more Amazon Redshift clusters in Data Wrangler\. To use this import option, you must create at least one cluster in Amazon Redshift\. To learn how, see [Getting started with Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/gsg/getting-started.html)\.

You can output the results of your Amazon Redshift query in one of the following locations:
+ The default Amazon S3 bucket
+ An Amazon S3 output location that you specify

You can either import the entire dataset or sample a portion of it\. For Amazon Redshift, it provides the following sampling options:
+ None – Import the entire dataset\.
+ First K – Sample the first K rows of the dataset, where K is an integer that you specify\.
+ Randomized – Takes a random sample of a size that you specify\.
+ Stratified – Takes a stratified random sample\. A stratified sample preserves the ratio of values in a column\.

The default Amazon S3 bucket is in the same AWS Region in which your Studio instance is located to store Amazon Redshift query results\. For more information, see [Imported Data Storage](#data-wrangler-import-storage)\.

For either the default Amazon S3 bucket or the bucket that you specify, you have the following encryption options:
+ The default AWS service\-side encryption with an Amazon S3 managed key \(SSE\-S3\)
+  An AWS Key Management Service \(AWS KMS\) key that you specify

An AWS KMS key is an encryption key that you create and manage\. For more information on KMS keys, see [AWS Key Management Service](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html)\.

You can specify an AWS KMS key using either the key ARN or the ARN of your AWS account\.

If you use the IAM managed policy, `AmazonSageMakerFullAccess`, to grant a role permission to use Data Wrangler in Studio, your **Database User** name must have the prefix `sagemaker_access`\.

Use the following procedures to learn how to add a new cluster\. 

**Note**  
Data Wrangler uses the Amazon Redshift Data API with temporary credentials\. To learn more about this API, refer to [Using the Amazon Redshift Data API](https://docs.aws.amazon.com/redshift/latest/mgmt/data-api.html) in the Amazon Redshift Management Guide\. 

**To connect to a Amazon Redshift cluster**

1. Sign into [Amazon SageMaker Console](https://console.aws.amazon.com/sagemaker)\.

1. Choose **Studio**\.

1. Choose **Launch app**\.

1. From the dropdown list, select **Studio**\.

1. Choose the Home icon\.

1. Choose **Data**\.

1. Choose **Data Wrangler**\.

1. Choose **Import data**\.

1. Under **Available**, choose **Amazon Athena**\.

1. Choose **Amazon Redshift**\.

1. Choose **Temporary credentials \(IAM\)** for **Type**\.

1. Enter a **Connection Name**\. This is a name used by Data Wrangler to identify this connection\. 

1. Enter the **Cluster Identifier** to specify to which cluster you want to connect\. Note: Enter only the cluster identifier and not the full endpoint of the Amazon Redshift cluster\.

1. Enter the **Database Name** of the database to which you want to connect\.

1. Enter a **Database User** to identify the user you want to use to connect to the database\. 

1. For **UNLOAD IAM Role**, enter the IAM role ARN of the role that the Amazon Redshift cluster should assume to move and write data to Amazon S3\. For more information about this role, see [Authorizing Amazon Redshift to access other AWS services on your behalf](https://docs.aws.amazon.com/redshift/latest/mgmt/authorizing-redshift-service.html) in the Amazon Redshift Management Guide\. 

1. Choose **Connect**\.

1. \(Optional\) For **Amazon S3 output location**, specify the S3 URI to store the query results\.

1. \(Optional\) For **KMS key ID**, specify the ARN of the AWS KMS key or alias\. The following image shows you where you can find either key in the AWS Management Console\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/kms-alias-redacted.png)

The following image shows all the fields from the preceding procedure\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/redshift-connection.png)

After your connection is successfully established, it appears as a data source under **Data Import**\. Select this data source to query your database and import data\.

**To query and import data from Amazon Redshift**

1. Select the connection that you want to query from **Data Sources**\.

1. Select a **Schema**\. To learn more about Amazon Redshift Schemas, see [Schemas](https://docs.aws.amazon.com/redshift/latest/dg/r_Schemas_and_tables.html) in the Amazon Redshift Database Developer Guide\.

1. \(Optional\) Under **Advanced configuration**, specify the **Sampling** method that you'd like to use\.

1. Enter your query in the query editor and choose **Run** to run the query\. After a successful query, you can preview your result under the editor\.

1. Select **Import dataset** to import the dataset that has been queried\. 

1. Enter a **Dataset name**\. If you add a **Dataset name** that contains spaces, these spaces are replaced with underscores when your dataset is imported\. 

1. Choose **Add**\.

To edit a dataset, do the following\.

1. Navigate to your Data Wrangler flow\.

1. Choose the \+ next to **Source \- Sampled**\.

1. Change the data that you're importing\.

1. Choose **Apply**

## Import data from Amazon EMR<a name="data-wrangler-emr"></a>

You can use Amazon EMR as a data source for your Amazon SageMaker Data Wrangler flow\. Amazon EMR is a managed cluster platform that you can use process and analyze large amounts of data\. For more information about Amazon EMR, see [What is Amazon EMR?](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-what-is-emr.html)\. To import a dataset from EMR, you connect to it and query it\. 

**Important**  
You must meet the following prerequisites to connect to an Amazon EMR cluster:  
You have an Amazon VPC in the Region that you're using to launch Amazon SageMaker Studio and Amazon EMR\.
You must do one of the following:  
Launch Amazon EMR and Amazon SageMaker Studio in the same private subnet\.
Launch Amazon EMR and Amazon SageMaker Studio in different private subnets\.
Amazon SageMaker Studio must be in VPC\-only mode\.  
For more information about creating a VPC, see [Create a VPC](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html#Create-VPC)\.  
For more information about creating a VPC, see [Connect SageMaker Studio Notebooks in a VPC to External Resources](https://docs.aws.amazon.com/vpc/latest/userguide/studio-notebooks-and-internet-access.html)\.
The Amazon EMR clusters that you're running must be in the same Amazon VPC\.
The Amazon EMR clusters and the Amazon VPC must be in the same AWS account\.
Your Amazon EMR clusters are running Presto\. They must allow inbound traffic from Studio security groups on port 8889\.
Amazon SageMaker Studio must run Jupyter Lab Version 3\. For information about updating the Jupyter Lab Version, see [View and update the JupyterLab version of an application from the console](studio-jl.md#studio-jl-view)\.
Amazon SageMaker Studio has an IAM role that controls users access\. The default IAM role that you're using to run Amazon SageMaker Studio doesn't have policies that you can give you access Amazon EMR clusters\. You must attach the policy granting permissions to the IAM role\. For more information, see [Required Permissions](studio-notebooks-emr-required-permissions.md)\.
The IAM role must also have the following policy attached `secretsmanager:PutResourcePolicy`\.
If you're using a Studio domain that you've already created, make sure that its `AppNetworkAccessType` is in VPC\-only mode\. For information about updating a domain to use VPC\-only mode, see [Shut down and Update SageMaker Studio](studio-tasks-update-studio.md)\.
You must have Presto installed on your cluster\.
The Amazon EMR release must be version 5\.5\.0 or later\.  
Amazon EMR supports auto termination\. Auto termination stops idle clusters from running and prevents you from incurring costs\. The following are the releases that support auto termination:  
For 6\.x releases, version 6\.1\.0 or later\.
For 5\.x releases, version 5\.30\.0 or later\.

An Amazon VPC is a virtual network that is logically isolated from other networks on the AWS cloud\. Amazon SageMaker Studio and your Amazon EMR cluster only exist within the Amazon VPC\.

Use the following procedure to launch Amazon SageMaker Studio in an Amazon VPC\.

To launch Studio within a VPC, do the following\.

1. Navigate to the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Launch SageMaker Studio**\.

1. Choose **Standard setup**\.

1. For **Default execution role**, choose the IAM role to set up Studio\.

1. Choose the VPC where you've launched the Amazon EMR clusters\.

1. For **Subnet**, choose a private subnet\.

1. For **Security group\(s\)**, specify the security groups that you're using to control between your VPC

1. Choose **VPC Only**\.

1. \(Optional\) AWS uses a default encryption key\. You can specify an AWS Key Management Service key to encrypt your data\.

1. Choose **Next**\.

1. Under **Studio settings**, choose the configurations that are best suited to you\.

1. Choose **Next** to skip the SageMaker Canvas settings\.

1. Choose **Next** to skip the RStudio settings\.

If you don't have an Amazon EMR cluster ready, you can use the following procedure to create one\. For more information about Amazon EMR, see [What is Amazon EMR?](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-what-is-emr.html)

To create a cluster, do the following\.

1. Navigate to the AWS Management Console\.

1. In the search bar, specify **Amazon EMR**\.

1. Choose **Create cluster**\.

1. For **Cluster name**, specify the name of your cluster\.

1. For **Release**, select the release version of the cluster\.
**Note**  
Amazon EMR supports auto termination for the following releases:  
For 6\.x releases, releases 6\.1\.0 or later
For 5\.x releases, releases 5\.30\.0 or later
Auto termination stops idle clusters from running and prevent you from incurring costs\.

1. Choose the application that you're running on the cluster\.

1. Under **Networking**, for **Hardware configuration**, specify the hardware configuration settings\.
**Important**  
For **Networking**, choose the VPC that is running Amazon SageMaker Studio and choose a private subnet\.

1. Under **Security and access**, specify the security settings\.

1. Choose **Create**\.

For a tutorial about creating an Amazon EMR cluster, see [Getting started with Amazon EMR](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-gs.html)\. For information about best practices for configuring a cluster, see [Considerations and best practices](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-plan-ha-considerations.html)\.

**Important**  
To use AWS Glue as metastore for Presto tables, select **Use** for **Presto table metadata** to store the results of your Amazon EMR queries in a AWS Glue data catalog when launching EMR clusters\. Storing the query results in a AWS Glue data catalog can save you from incurring charges\.  
To be able to query large datasets on Amazon EMR clusters, add the following properties to Presto configuration file on your EMR clusters:  

```
[{"classification":"presto-config","properties":{
"http-server.max-request-header-size":"5MB",
"http-server.max-response-header-size":"5MB"}}]
```
You can also modify the configuration settings when you launch the Amazon EMR cluster\.  
The configuration file for your Amazon EMR cluster is located under the following path: `/etc/presto/conf/config.properties`\.

**Note**  
For security best practices, Data Wrangler can only connect to VPCs on private subnets\. You won't be able to connect to the master node unless you use AWS Systems Manager for your EMR instances\. For more information, see [Securing access to EMR clusters using AWS Systems Manager](http://aws.amazon.com/blogs/big-data/securing-access-to-emr-clusters-using-aws-systems-manager/)\.

You can currently use the following methods to access and Amazon EMR cluster:
+ No authentication
+ Lightweight Directory Access Protocol \(LDAP\)

The following sections show how you can configure LDAP for a Presto cluster\.

Presto LDAP requires access to the Presto coordinator need to be through HTTPS\. Once the LDAP server has been setup to be able to authenticate Presto from EMR cluster via port 636, enable SSL for Presto coordinator, add or modify the following configurations for Presto:

```
- Classification: presto-config
     ConfigurationProperties:
        http-server.authentication.type: 'PASSWORD'
        http-server.https.enabled: 'true'
        http-server.https.port: '8889'
        http-server.http.port: '8899'
        node-scheduler.include-coordinator: 'true'
        http-server.https.keystore.path: '/path/to/keystore/path/for/presto'
        http-server.https.keystore.key: 'kestore-key-password'
        discovery.uri: 'http://master-node-dns-name:8899'
- Classification: presto-password-authenticator
     ConfigurationProperties:
        password-authenticator.name: 'ldap'
        ldap.url: !Sub 'ldaps://ldap-server-dns-name:636'
        ldap.user-bind-pattern: "uid=${USER},dc=example,dc=org"
        internal-communication.authentication.ldap.user: "ldap-user-name"
        internal-communication.authentication.ldap.password: "ldap-password"
```

For information about setting up LDAP in Presto, see the following resources:
+ [LDAP Authentication](https://prestodb.io/docs/current/security/ldap.html)
+ [Using LDAP Authentication for Presto on Amazon EMR](https://docs.aws.amazon.com/emr/latest/ReleaseGuide/emr-presto-ldap.html)

**Note**  
As a security best practice, we recommend enabling SSL for Presto\. For more information, see [Secure Internal Communication](https://prestodb.io/docs/current/security/internal-communication.html)\.

Use the following procedure to import data from a cluster\.

To import data from a cluster, do the following\.

1. Open a Data Wrangler flow\.

1. Choose **Create Connection**\.

1. Choose **Amazon EMR**\.

1. Do one of the following\.
   + \(Optional\) For **Secrets ARN**, specify the Amazon Resource Number \(ARN\) of the database within the cluster\. Secrets provide additional security\. For more information about secrets, see [What is AWS Secrets Manager?](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) For information about creating a secret for your cluster, see [Creating a AWS Secrets Manager secret for your cluster](#data-wrangler-emr-secrets-manager)\.
   + From the dropdown table, choose a cluster\.

1. Choose **Next**\.

1. For **Select an endpoint for *example\-cluster\-name* cluster**, choose a query engine\.

1. \(Optional\) Select **Save connection**\.

1. Choose **Next, select login** and choose one of the following:
   + No authentication
   + LDAP

1. For **Login into *example\-cluster\-name* cluster**, specify the **Username** and **Password** for the cluster\.

1. Choose **Connect**\.

1. In the query editor specify a SQL query\.

1. Choose **Run**\.

1. Choose **Import**\.

### Creating a AWS Secrets Manager secret for your cluster<a name="data-wrangler-emr-secrets-manager"></a>

A Secrets Manager secret stores the JDBC URL of the Amazon EMR cluster as a secret\. Using a secret is more secure than directly entering in your credentials\.

Use the following procedure to store the JDBC URL as a secret\.

To store the JDBC URL as a secret, do the following\.

1. Navigate to the AWS Management Console\.

1. In the search bar, specify Secrets Manager\.

1. Choose **AWS Secrets Manager**\.

1. Choose **Store a new secret**\.

1. For **Secret type**, choose **Other type of secret**\.

1. For **Key/value pairs**, specify `jdbcURL` as the key and a valid JDBC URL as the value\.

   The following list shows the valid JBDC URL formats for the different possible configurations\.
   + Presto, no authentication – jdbc:presto://*emr\-cluster\-master\-public\-dns*:8889/;
   + For Presto with SSL enabled, the JDBC URL format depends on whether you use a Java Keystore File for the TLS configuration\. The Java Keystore File helps verify the identity of the master node of the Amazon EMR cluster\. To use a Java Keystore File, generate it on an EMR cluster and upload it to Data Wrangler\. To upload a file, choose the upward arrow on the left\-hand navigation of the Data Wrangler UI\. For information about creating a Java Keystore File for Presto, see [Java Keystore File for TLS](https://prestodb.io/docs/current/security/tls.html#server-java-keystore)\. For information about running commands on an Amazon EMR cluster, see [Securing access to EMR clusters using AWS Systems Manager](http://aws.amazon.com/blogs/big-data/securing-access-to-emr-clusters-using-aws-systems-manager/)\.
     + Wihtout a Java Keystore File – `jdbc:presto://emr-cluster-master-public-dns:8889/;SSL=1;AuthenticationType=LDAP Authentication;UID=user-name;PWD=password;AllowSelfSignedServerCert=1;AllowHostNameCNMismatch=1;`
     + With a Java Keystore File – `jdbc:presto://emr-cluster-master-public-dns:8889/;SSL=1;AuthenticationType=LDAP Authentication;SSLTrustStorePath=/home/sagemaker-user/data/Java-keystore-file-name;SSLTrustStorePwd=Java-keystore-file-passsword;UID=user-name;PWD=password;`

Throughout the process of importing data from an Amazon EMR cluster, you might run into issues\. For information about troubleshooting them, see [Troubleshooting issues with Amazon EMR](data-wrangler-trouble-shooting.md#data-wrangler-trouble-shooting-emr)\.

## Import data from Databricks \(JDBC\)<a name="data-wrangler-databricks"></a>

You can use Databricks as a data source for your Amazon SageMaker Data Wrangler flow\. To import a dataset from Databricks, use the JDBC \(Java Database Connectivity\) import functionality to access to your Databricks database\. After you access the database, specify a SQL query to get the data and import it\.

We assume that you have a running Databricks cluster and that you've configured your JDBC driver to it\. For more information, see the following Databricks documentation pages:
+ [JDBC driver](https://docs.databricks.com/integrations/bi/jdbc-odbc-bi.html#jdbc-driver)
+ [JDBC configuration and connection parameters](https://docs.databricks.com/integrations/bi/jdbc-odbc-bi.html#jdbc-configuration-and-connection-parameters)
+ [Authentication parameters](https://docs.databricks.com/integrations/bi/jdbc-odbc-bi.html#authentication-parameters)

Data Wrangler stores your JDBC URL in AWS Secrets Manager\. You must give your Amazon SageMaker Studio IAM execution role permissions to use Secrets Manager\. Use the following procedure to give permissions\.

To give permissions to Secrets Manager, do the following\.

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. Choose **Roles**\.

1. In the search bar, specify the Amazon SageMaker execution role that Amazon SageMaker Studio is using\.

1. Choose the role\.

1. Choose **Add permissions**\.

1. Choose **Create inline policy**\.

1. For **Service**, specify **Secrets Manager** and choose it\.

1. For **Actions**, select the arrow icon next to **Permissions management**\.

1. Choose **PutResourcePolicy**\.

1. For **Resources**, choose **Specific**\.

1. Choose the checkbox next to **Any in this account**\.

1. Choose **Review policy**\.

1. For **Name**, specify a name\.

1. Choose **Create policy**\.

You can use partitions to import your data more quickly\. Partitions give Data Wrangler the ability to process the data in parallel\. By default, Data Wrangler uses 2 partitions\. For most use cases, 2 partitions give you near\-optimal data processing speeds\.

If you choose to specify more than 2 partitions, you can also specify a column to partition the data\. The type of the values in the column must be numeric or date\.

We recommend using partitions only if you understand the structure of the data and how it's processed\.

You can either import the entire dataset or sample a portion of it\. For a Databricks database, it provides the following sampling options:
+ None – Import the entire dataset\.
+ First K – Sample the first K rows of the dataset, where K is an integer that you specify\.
+ Randomized – Takes a random sample of a size that you specify\.
+ Stratified – Takes a stratified random sample\. A stratified sample preserves the ratio of values in a column\.

Use the following procedure to import your data from a Databricks database\.

To import data from Databricks, do the following\.

1. Sign into [Amazon SageMaker Console](https://console.aws.amazon.com/sagemaker)\.

1. Choose **Studio**\.

1. Choose **Launch app**\.

1. From the dropdown list, select **Studio**\.

1. From the **Import data** tab of your Data Wrangler flow, choose **Databricks**\.

1. Specify the following fields:
   + **Dataset name** – A name that you want to use for the dataset in your Data Wrangler flow\.
   + **Driver** – **com\.simba\.spark\.jdbc\.Driver**\.
   + **JDBC URL** – The URL of the Databricks database\. The URL formatting can vary between Databricks instances\. For information about finding the URL and the specifying the parameters within it, see [JDBC configuration and connection parameters](https://docs.databricks.com/integrations/bi/jdbc-odbc-bi.html#jdbc-configuration-and-connection-parameters)\. The following is an example of how a URL can be formatted: jdbc:spark://aws\-sagemaker\-datawrangler\.cloud\.databricks\.com:443/default;transportMode=http;ssl=1;httpPath=sql/protocolv1/o/3122619508517275/0909\-200301\-cut318;AuthMech=3;UID=*token*;PWD=*personal\-access\-token*\.
**Note**  
You can specify a secret ARN that contains the JDBC URL instead of specifying the JDBC URL itself\. The secret must contain a key\-value pair with the following format: `jdbcURL:JDBC-URL`\. For more information, see [What is Secrets Manager?](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html)\.

1. Specify a SQL SELECT statement\.
**Note**  
Data Wrangler doesn't support Common Table Expressions \(CTE\) or temporary tables within a query\.

1. For **Sampling**, choose a sampling method\.

1. Choose **Run**\. 

1. \(Optional\) For the **PREVIEW**, choose the gear to open the **Partition settings**\.   


   1. Specify the number of partitions\. You can partition by column if you specify the number of partitions:
     + **Enter number of partitions** – Specify a value greater than 2\.
     + \(Optional\) **Partition by column** – Specify the following fields\. You can only partition by a column if you've specified a value for **Enter number of partitions**\.
       + **Select column** – Select the column that you're using for the data partition\. The data type of the column must be numeric or date\.
       + **Upper bound** – From the values in the column that you've specified, the upper bound is the value that you're using in the partition\. The value that you specify doesn't change the data that you're importing\. It only affects the speed of the import\. For the best performance, specify an upper bound that's close to the column's maximum\.
       + **Lower bound** – From the values in the column that you've specified, the lower bound is the value that you're using in the partition\. The value that you specify doesn't change the data that you're importing\. It only affects the speed of the import\. For the best performance, specify a lower bound that's close to the column's minimum\.

1. Choose **Import**\.

## Import data from Snowflake<a name="data-wrangler-snowflake"></a>

You can use Snowflake as a data source in SageMaker Data Wrangler to prepare data in Snowflake for machine learning\.

With Snowflake as a data source in Data Wrangler, you can quickly connect to Snowflake without writing a single line of code\. Additionally, you can join your data in Snowflake with data stored in Amazon S3 and data queried through Amazon Athena and Amazon Redshift to prepare data for machine learning\. 

Once connected, you can interactively query data stored in Snowflake, transform data with more than 300 preconfigured data transformations, understand data and identify potential errors and extreme values with a set of robust preconfigured visualization templates, quickly identify inconsistencies in your data preparation workflow, and diagnose issues before models are deployed into production\. Finally, you can export your data preparation workflow to Amazon S3 for use with other SageMaker features such as Amazon SageMaker Autopilot, Amazon SageMaker Feature Store and Amazon SageMaker Model Building Pipelines\.

You can encrypt the output of your queries using an AWS Key Management Service key that you've created\. For more information about AWS KMS, see [AWS Key Management Service](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html)\.

### Administrator Guide<a name="data-wrangler-snowflake-admin"></a>

**Important**  
To learn more about granular access control and best practices, see [Security Access Control](https://docs.snowflake.com/en/user-guide/security-access-control.html)\. 

This section is for Snowflake administrators who are setting up access to Snowflake from within SageMaker Data Wrangler\.

**Important**  
Your administrator is responsible for managing and monitoring the access control within Snowflake\. This includes what data a user can access, what storage integration a user can use, and what queries a user can run\. Data Wrangler does not add a layer of access control with respect to Snowflake\. 

**Important**  
Note that granting monitor privileges can permit users to see details within an object, such as queries or usage within a warehouse\. 

#### Configure Snowflake with Data Wrangler<a name="data-wrangler-snowflake-admin-config"></a>

To import data from Snowflake, Snowflake admins must configure access from Data Wrangler using Amazon S3\.

This feature is currently not available in the opt\-in Regions\.

To configure access, follow these steps\.

1. Configure access permissions for the S3 bucket\.

   **AWS Access Control Requirements**

   Snowflake requires the following permissions on an S3 bucket and directory to be able to access files in the directory\. 
   + `s3:GetObject`
   + `s3:GetObjectVersion`
   + `s3:ListBucket`
   + `s3:ListObjects`
   + `s3:GetBucketLocation`

   **Create an IAM policy**

   The following steps describe how to configure access permissions for Snowflake in your AWS Management Console so you can use an S3 bucket to load and unload data: 
   + Log in to the AWS Management Console\.
   + From the home dashboard, choose **IAM**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/iam.png)
   + Choose **Policies**\.
   + Choose **Create Policy**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/create_policy.png)
   + Select the **JSON** tab\.
   + Add a policy document that allows Snowflake to access the S3 bucket and directory\. 

     The following policy \(in JSON format\) provides Snowflake with the required permissions to load and unload data using a single bucket and directory path\. Make sure to replace `bucket` and `prefix` with your actual bucket name and directory path prefix\.

     ```
     # Example policy for S3 write access
     # This needs to be updated
     {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": [
             "s3:PutObject",
             "s3:GetObject",
             "s3:GetObjectVersion",
             "s3:DeleteObject",
             "s3:DeleteObjectVersion"
         ],
         "Resource": "arn:aws:s3:::bucket/prefix/*"
       },
       {
         "Effect": "Allow",
         "Action": [
             "s3:ListBucket"
         ],
         "Resource": "arn:aws:s3:::bucket/",
         "Condition": {
             "StringLike": {
                 "s3:prefix": ["prefix/*"]
             }
         }
       }
      ]
     }
     ```
   + Choose **Next: Tags**\.
   + Choose **Next: Review**\.

     Enter the policy name \(such as `snowflake_access`\) and an optional description\. Choose **Create policy**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/create_policy_review.png)

1. [Create the IAM Role in AWS](https://docs.snowflake.com/en/user-guide/data-load-s3-config-storage-integration.html#step-2-create-the-iam-role-in-aws)\.

1. [Create a Cloud Storage Integration in Snowflake](https://docs.snowflake.com/en/user-guide/data-load-s3-config-storage-integration.html#step-3-create-a-cloud-storage-integration-in-snowflake)\.

1. [Retrieve the AWS IAM User for your Snowflake Account](https://docs.snowflake.com/en/user-guide/data-load-s3-config-storage-integration.html#step-4-retrieve-the-aws-iam-user-for-your-snowflake-account)\.

1. [Grant the IAM User Permissions to Access Bucket](https://docs.snowflake.com/en/user-guide/data-load-s3-config-storage-integration.html#step-5-grant-the-iam-user-permissions-to-access-bucket-objects)\.

1. Grant the data scientist's Snowflake role usage permission to the storage integration\.
   + In the Snowflake console, run `GRANT USAGE ON INTEGRATION integration_name TO snowflake_role;`
     + `integration_name` is the name of your storage integration\.
     + `snowflake_role` is the name of the default [Snowflake role](https://docs.snowflake.com/en/user-guide/security-access-control-overview.html#roles) given to the data scientist user\.

#### Provide information to the data scientist<a name="data-wrangler-snowflake-admin-ds-info"></a>

Provide the data scientist with the information that they need to access Snowflake from Amazon SageMaker Data Wrangler\.

1. To allow your data scientist to access Snowflake from SageMaker Data Wrangler, provide them with one of the following:
   + A Snowflake account name, user name, and password\.
   + A secret created with [AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) and the ARN of the secret\. Use the following procedure below to create the secret for Snowflake if you choose this option\.
**Important**  
If your data scientists use the **Snowflake Credentials \(User name and Password\)** option to connect to Snowflake, you can use [Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) to store the credentials in a secret\. Secrets Manager rotates secrets as part of a best practice security plan\. The secret created in Secrets Manager is only accessible with the Studio role configured when you set up a Studio user profile\. This requires you to add this permission, `secretsmanager:PutResourcePolicy`, to the policy that is attached to your Studio role\.  
We strongly recommend that you scope the role policy to use different roles for different groups of Studio users\. You can add additional resource\-based permissions for the Secrets Manager secrets\. See [Manage Secret Policy](https://docs.aws.amazon.com/secretsmanager/latest/userguide/manage_secret-policy.html) for condition keys you can use\.  
For information about creating a secret, see [Create a secret](https://docs.aws.amazon.com/secretsmanager/latest/userguide/create_secret.html)\. You're charged for the secrets that you create\.

1. Provide the data scientist with the name of the storage integration you created in Step 3: [Create a Cloud Storage Integration in Snowflake](                                      https://docs.snowflake.com/en/user-guide/data-load-s3-config-storage-integration.html#step-3-create-a-cloud-storage-integration-in-snowflake)\. This is the name of the new integration and is called `integration_name` in the `CREATE INTEGRATION` SQL command you ran, which is shown in the following snippet: 

   ```
     CREATE STORAGE INTEGRATION integration_name
     TYPE = EXTERNAL_STAGE
     STORAGE_PROVIDER = S3
     ENABLED = TRUE
     STORAGE_AWS_ROLE_ARN = 'iam_role'
     [ STORAGE_AWS_OBJECT_ACL = 'bucket-owner-full-control' ]
     STORAGE_ALLOWED_LOCATIONS = ('s3://bucket/path/', 's3://bucket/path/')
     [ STORAGE_BLOCKED_LOCATIONS = ('s3://bucket/path/', 's3://bucket/path/') ]
   ```

### Data Scientist Guide<a name="data-wrangler-snowflake-ds"></a>

This section outlines how to access your Snowflake data warehouse from within SageMaker Data Wrangler and how to use Data Wrangler features\.

**Important**  
Note: Your administrator needs to follow the Administer Guide set up in the preceding section before you can use Data Wrangler within Snowflake\. 

Use the following procedure to open Amazon SageMaker Studio and see which version you're running\.

To open Studio and check its version, see the following procedure\.

1. Use the steps in [Prerequisites](data-wrangler-getting-started.md#data-wrangler-getting-started-prerequisite) to access Data Wrangler through Amazon SageMaker Studio\.

1. Next to the user you want to use to launch Studio, select **Launch app**\.

1. Choose **Studio**\.

1. After Studio loads, select **File**, then **New**, and then **Terminal**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/terminal.png)

1. Once you have launched Studio, select **File**, then **New**, and then **Terminal**\.

1. Enter `cat /opt/conda/share/jupyter/lab/staging/yarn.lock | grep -A 1 "@amzn/sagemaker-ui-data-prep-plugin@"` to print the version of your Studio instance\. You must have Studio version 1\.3\.0 to use Snowflake\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/cat-command.png)

Use the following procedure to check that you're running version 1\.3\.0 or greater\.

To check the version of Studio, do the following\.

1. If you do not have this version, then update your Studio version\. To do this, close your Studio window and navigate to the [SageMaker Studio Console](https://console.aws.amazon.com/sagemaker)\.

1. Next, select the user you are using to access Studio and then select **Delete app**\. After the deletion is complete, launch Studio again by selecting **Open Studio**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/user-details.png)

1. Follow Step 3 again to verify that your Studio version is 1\.3\.0\.

Use the following procedure to connect to Snowflake\.

1. Create a new data flow from within Data Wrangler

   Once you have accessed Data Wrangler from within Studio and have version 1\.3\.0, select the **\+** sign on the **New data flow** card under **ML tasks and components**\. This creates a new directory in Studio with a \.flow file inside, which contains your data flow\. The \.flow file automatically opens in Studio\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/studio-ml.png)

   Alternatively, you can also create a new flow by selecting **File**, then **New**, and then choosing **Flow**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/studio-flow.png)

   When you create a new \.flow file in Studio, you may see a message at the top of the Data Wrangler interface that says: 

   **Connecting to engine**

   **Establishing connection to engine\.\.\.**

1. Connect to Snowflake\.

   There are two ways to connect to Snowflake from within Data Wrangler\. You only need to choose one of the two ways\. 

   1. Specify your Snowflake credentials \(account name, user name, and password\) in Data Wrangler\. 

   1. Provide an Amazon Resource Name \(ARN\) of a secret\. 
**Important**  
If you do not have your Snowflake credentials or ARN, reach out to your administrator\. Your administrator can tell you which of the preceding methods to use to connect to Snowflake\.

   Start on the **Import** data screen and first select **Add data source** from the dropdown menu, and then select **Snowflake**\. 

Choose an authentication method\. For this step, as previously mentioned, you can use your Snowflake credentials or ARN name\. One of the two is** provided by your administrator\. [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)**

Next, we explain both authentication methods and provide screenshots for each\. 

1. **Snowflake Credentials Option**\.

   Select the **Basic** \(user name and password\) option from the **Authentication method** dropdown list\. Then, enter your credentials in the following fields: 
   + **Storage integration**: Provide the name of the storage integration\. Your administrator provides this name\. 
   + **Snowflake account name**: The full name of your Snowflake account\.
   + **User name**: Snowflake account user name\.
   + **Password**: Snowflake account password\.
   + **Connection name**: Choose a connection name for your choice\.
   + \(Optional\) **KMS key ID**: Choose the AWS KMS key to encrypt the output of the Snowflake query\. For more information about AWS Key Management Service, see [https://docs.aws.amazon.com/kms/latest/developerguide/overview.html](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html)\. If you don't specify a AWS KMS key, Data Wrangler uses the default SSE\-KMS encryption method\.

   Select **Connect**\. 

   The following screenshot shows how to complete these fields\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-connection.png)

1. **ARN Option**

   Select the ARN option from the **Authentication method** dropdown list\. Then, provide your ARN name under **Secrets Manager ARN** and your **Storage integration**, which is provided by your administrator\. If you've created a KMS key, you can specify its ID for **KMS key ID**\. For more information about AWS Key Management Service, see [https://docs.aws.amazon.com/kms/latest/developerguide/overview.html](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html)\. Create a **Connection name** and select **Connect**, as shown in the following screenshot\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-connection-complete.png)

1. The workflow at this point is to connect your Snowflake account to Data Wrangler, then run some queries on your data and then finally use Data Wrangler for performing data transformations\.

   The following steps explain the import and querying step from within Data Wrangler\.

   After creating your Snowflake connection, you are taken to the **Import data from Snowflake** screen, as shown in the following screenshot\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/import-snowflake.png)

   From here, select your warehouse\. You can also optionally select your database and schema, in which case the written query should specify them\. If **Database** and **Schema** are provided in the dropdown list, the written query does not need to specify the database and schema names\.

   Your schemas and tables from your Snowflake account are listed in the left panel\. You can select and unravel these entities\. When you select a specific table, select the eye icon on the right side of each table name to preview the table\.
**Important**  
If you're importing a dataset with columns of type `TIMESTAMP_TZ` or `TIMESTAMP_LTZ`, add `::string` to the column names of your query\. For more information, see [How To: Unload TIMESTAMP\_TZ and TIMESTAMP\_LTZ data to a Parquet file](https://community.snowflake.com/s/article/How-To-Unload-Timestamp-data-in-a-Parquet-file)\.

   The following screenshot shows the panel with your data warehouses, databases, and schemas, along with the eye icon with which you can preview your table\. Once you select the **Preview Table** icon, the schema preview of that table is generated\. You must select a warehouse before you can preview a table\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/studio-panel-snowflake.png)

   After selecting a data warehouse, database and schema, you can now write queries and run them\. The output of your query shows under **Query results**, as shown in the following screenshot\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-queries.png)

   Once you have settled on the output of your query, you can then import the output of your query into a Data Wrangler flow to perform data transformations\. 

   To do this, select **Import**, then specify a name and select **Go**, as shown in the following screenshot\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-import.png)

   From here, transition to the **Data flow** screen to prepare your data transformation, as shown in the following screenshot\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-transition.png)

### Private Connectivity between Data Wrangler and Snowflake via AWS PrivateLink<a name="data-wrangler-security-snowflake-vpc"></a>

This section explains how to use AWS PrivateLink to establish a private connection between Data Wrangler and Snowflake\. The steps are explained in the following sections\. 

#### Create a VPC<a name="data-wrangler-snowflake-snowflake-vpc-setup"></a>

If you do not have a VPC set up, then follow the [Create a new VPC](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/gsg_create_vpc.html#create_vpc) instructions to create one\.

Once you have a chosen VPC you would like to use for establishing a private connection, provide the following credentials to your Snowflake Administrator to enable AWS PrivateLink:
+ VPC ID
+ AWS Account ID
+ Your corresponding account URL you use to access Snowflake

**Important**  
As described in Snowflake's documentation, enabling your Snowflake account can take up to two business days\. 

#### Set up Snowflake AWS PrivateLink Integration<a name="data-wrangler-snowflake-snowflake-vpc-privatelink-setup"></a>

After AWS PrivateLink is activated, retrieve the AWS PrivateLink configuration for your Region by running the following command in a Snowflake worksheet\. Log into your Snowflake console and enter the following under **Worksheets**: `select SYSTEM$GET_PRIVATELINK_CONFIG();` 

1. Retrieve the values for the following: `privatelink-account-name`, `privatelink_ocsp-url`, `privatelink-account-url`, and `privatelink_ocsp-url` from the resulting JSON object\. Examples of each value are shown in the following snippet\. Store these values for later use\.

   ```
   privatelink-account-name: xxxxxxxx.region.privatelink
   privatelink-vpce-id: com.amazonaws.vpce.region.vpce-svc-xxxxxxxxxxxxxxxxx
   privatelink-account-url: xxxxxxxx.region.privatelink.snowflakecomputing.com
   privatelink_ocsp-url: ocsp.xxxxxxxx.region.privatelink.snowflakecomputing.com
   ```

1. Switch to your AWS Console and navigate to the VPC menu\.

1. From the left side panel, choose the **Endpoints** link to navigate to the **VPC Endpoints** setup\.

   Once there, choose **Create Endpoint**\. 

1. Select the radio button for **Find service by name**, as shown in the following screenshot\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-radio.png)

1. In the **Service Name** field, paste in the value for `privatelink-vpce-id` that you retrieved in the preceding step and choose **Verify**\. 

   If the connection is successful, a green alert saying **Service name found** appears on your screen and the **VPC** and **Subnet** options automatically expand, as shown in the following screenshot\. Depending on your targeted Region, your resulting screen may show another AWS Region name\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-service-name-found.png)

1. Select the same VPC ID that you sent to Snowflake from the **VPC** dropdown list\.

1. If you have not yet created a subnet, then perform the following set of instructions on creating a subnet\. 

1. Select **Subnets** from the **VPC** dropdown list\. Then select **Create subnet** and follow the prompts to create a subset in your VPC\. Ensure you select the VPC ID you sent Snowflake\. 

1. Under **Security Group Configuration**, select **Create New Security Group** to open the default **Security Group** screen in a new tab\. In this new tab, select t**Create Security Group**\. 

1. Provide a name for the new security group \(such as `datawrangler-doc-snowflake-privatelink-connection`\) and a description\. Be sure to select the VPC ID you have used in previous steps\. 

1. Add two rules to allow traffic from within your VPC to this VPC endpoint\. 

   Navigate to your VPC under **Your VPCs** in a separate tab, and retrieve your CIDR block for your VPC\. Then choose **Add Rule** in the **Inbound Rules** section\. Select `HTTPS` for the type, leave the **Source** as **Custom** in the form, and paste in the value retrieved from the preceding `describe-vpcs` call \(such as `10.0.0.0/16`\)\. 

1. Choose **Create Security Group**\. Retrieve the **Security Group ID** from the newly created security group \(such as `sg-xxxxxxxxxxxxxxxxx`\)\.

1. In the **VPC Endpoint** configuration screen, remove the default security group\. Paste in the security group ID in the search field and select the checkbox\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-security-group.png)

1. Select **Create Endpoint**\. 

1. If the endpoint creation is successful, you see a page that has a link to your VPC endpoint configuration, specified by the VPC ID\. Select the link to view the configuration in full\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-success-endpoint.png)

   Retrieve the topmost record in the DNS names list\. This can be differentiated from other DNS names because it only includes the Region name \(such as `us-west-2`\), and no Availability Zone letter notation \(such as `us-west-2a`\)\. Store this information for later use\.

#### Configure DNS for Snowflake Endpoints in your VPC<a name="data-wrangler-snowflake-vpc-privatelink-dns"></a>

This section explains how to configure DNS for Snowflake endpoints in your VPC\. This allows your VPC to resolve requests to the Snowflake AWS PrivateLink endpoint\. 

1. Navigate to the [Route 53 menu](https://console.aws.amazon.com/route53) within your AWS console\.

1. Select the **Hosted Zones** option \(if necessary, expand the left\-hand menu to find this option\)\.

1. Choose **Create Hosted Zone**\.

   1. In the **Domain name** field, reference the value that was stored for `privatelink-account-url` in the preceding steps\. In this field, your Snowflake account ID is removed from the DNS name and only uses the value starting with the Region identifier\. A **Resource Record Set** is also created later for the subdomain, such as, `region.privatelink.snowflakecomputing.com`\.

   1. Select the radio button for **Private Hosted Zone** in the **Type** section\. Your Region code may not be `us-west-2`\. Reference the DNS name returned to you by Snowflake\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-create-hosted-zone.png)

   1. In the **VPCs to associate with the hosted zone** section, select the Region in which your VPC is located and the VPC ID used in previous steps\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-vpc-hosted-zone.png)

   1. Choose **Create hosted zone**\.

1. Next, create two records, one for `privatelink-account-url` and one for `privatelink_ocsp-url`\.
   + In the **Hosted Zone** menu, choose **Create Record Set**\.

     1. Under **Record name**, enter your Snowflake Account ID only \(the first 8 characters in `privatelink-account-url`\)\.

     1. Under **Record type**, select **CNAME**\.

     1. Under **Value**, enter the DNS name for the regional VPC endpoint you retrieved in the last step of the *Set up the Snowflake AWS PrivateLink Integration* section\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-quick-create-record.png)

     1. Choose **Create records**\.

     1. Repeat the preceding steps for the OCSP record we notated as `privatelink-ocsp-url`, starting with `ocsp` through the 8\-character Snowflake ID for the record name \(such as `ocsp.xxxxxxxx`\)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-quick-create-ocsp.png)

#### Configure Route 53 Resolver Inbound Endpoint for your VPC<a name="data-wrangler-snowflake-vpc-privatelink-route53"></a>

This section explains how to configure Route 53 resolvers inbound endpoints for your VPC\.

1. Navigate to the [Route 53 menu](https://console.aws.amazon.com/route53) within your AWS console\.
   + In the left hand panel in the **Security** section, select the **Security Groups** option\.

1. Choose **Create Security Group**\. 
   + Provide a name for your security group \(such as `datawranger-doc-route53-resolver-sg`\) and a description\.
   + Select the VPC ID used in previous steps\.
   + Create rules that allow for DNS over UDP and TCP from within the VPC CIDR block\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-inbound-rules.png)
   + Choose **Create Security Group**\. Note the **Security Group ID** because adds a rule to allow traffic to the VPC endpoint security group\.

1. Navigate to the [Route 53 menu](https://console.aws.amazon.com/route53) within your AWS console\.
   + In the **Resolver** section, select the **Inbound Endpoint** option\.

1. Choose **Create Inbound Endpoint**\. 
   + Provide an endpoint name\.
   + From the **VPC in the Region** dropdown list, select the VPC ID you have used in all previous steps\. 
   + In the **Security group for this endpoint** dropdown list, select the security group ID from Step 2 in this section\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-inbound-endpoint.png)
   + In the **IP Address** section, select an Availability Zones, select a subnet, and leave the radio selector for **Use an IP address that is selected automatically** selected for each IP address\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-ip-address-1.png)
   + Choose **Submit**\.

1. Select the **Inbound endpoint** after it has been created\.

1. Once the inbound endpoint is created, note the two IP addresses for the resolvers\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-ip-addresses-2.png)

#### SageMaker VPC Endpoints<a name="data-wrangler-snowflake-sagemaker-vpc-endpoints"></a>

 This section explains how to create VPC endpoints for the following: Amazon SageMaker Studio, SageMaker Notebooks, the SageMaker API, SageMaker Runtime, and Amazon SageMaker Feature Store Runtime\.

##### <a name="data-wrangler-snowflake-sagemaker-vpc-endpoints-create-security-group"></a>

**Create a security group that is applied to all endpoints\.**

1. Navigate to the [EC2 menu](https://console.aws.amazon.com/ec2) in the AWS Console\.

1. In the **Network & Security** section, select the **Security groups** option\.

1. Choose **Create security group**\.

1. Provide a security group name and description \(such as `datawrangler-doc-sagemaker-vpce-sg`\)\. A rule is added later to allow traffic over HTTPS from SageMaker to this group\. 

##### <a name="data-wrangler-snowflake-sagemaker-vpc-endpoints-creating-endpoints"></a>

**Creating the endpoints**

1. Navigate to the [VPC menu](https://console.aws.amazon.com/vpc) in the AWS console\.

1. Select the **Endpoints** option\.

1. Choose **Create Endpoint**\.

1. Search for the service by entering its name in the **Search** field\.

1. From the **VPC** dropdown list, select the VPC in which your Snowflake AWS PrivateLink connection exists\.

1. In the **Subnets** section, select the subnets which have access to the Snowflake PrivateLink connection\.

1. Leave the **Enable DNS Name** checkbox selected\.

1. In the **Security Groups** section, select the security group you created in the preceding section\.

1. Choose **Create Endpoint**\.

#### <a name="data-wrangler-snowflake-sagemaker-vpc-endpoints-studio-configuration"></a>

**Configure Studio and Data Wrangler**

This section explains how to configure Studio and Data Wrangler\.

1. Configure the security group\.

   1. Navigate to the Amazon EC2 menu in the AWS Console\.

   1. Select the **Security Groups** option in the **Network & Security** section\.

   1. Choose **Create Security Group**\. 

   1. Provide a name and description for your security group \(such as `datawrangler-doc-sagemaker-studio`\)\. 

   1. Create the following inbound rules\.
      + The HTTPS connection to the security group you provisioned for the Snowflake PrivateLink connection you created in the *Set up the Snowflake PrivateLink Integration* step\.
      + The HTTP connection to the security group you provisioned for the Snowflake PrivateLink connection you created in the *Set up the Snowflake PrivateLink Integration step*\.
      + The UDP and TCP for DNS \(port 53\) to Route 53 Resolver Inbound Endpoint security group you create in step 2 of *Configure Route 53 Resolver Inbound Endpoint for your VPC*\.

   1. Choose **Create Security Group** button in the lower right hand corner\.

1. Configure Studio\.
   + Navigate to the SageMaker menu in the AWS console\.
   + From the left hand console, Select the **SageMaker Studio** option\.
   + If you do not have any domains configured, the **Get Started** menu is present\.
   + Select the **Standard Setup** option from the **Get Started** menu\.
   + Under **Authentication method**, select **AWS Identity and Access Management \(IAM\)**\.
   + From the **Permissions** menu, you can create a new role or use a pre\-existing role, depending on your use case\.
     + If you choose **Create a new role**, you are presented the option to provide an S3 bucket name, and a policy is generated for you\.
     + If you already have a role created with permissions for the S3 buckets to which you require access, select the role from the dropdown list\. This role should have the `AmazonSageMakerFullAccess` policy attached to it\.
   + Select the **Network and Storage** dropdown list to configure the VPC, security, and subnets SageMaker uses\.
     + Under **VPC**, select the VPC in which your Snowflake PrivateLink connection exists\.
     + Under **Subnet\(s\)**, select the subnets which have access to the Snowflake PrivateLink connection\.
     + Under **Network Access for Studio**, select **VPC Only**\.
     + Under **Security Group\(s\)**, select the security group you created in step 1\.
   + Choose **Submit**\.

1. Edit the SageMaker security group\.
   + Create the following inbound rules:
     + Port 2049 to the inbound and outbound NFS Security Groups created automatically by SageMaker in step 2 \(the security group names contain the Studio domain ID\)\.
     + Access to all TCP ports to itself \(required for SageMaker for VPC Only\)\.

1. Edit the VPC Endpoint Security Groups:
   + Navigate to the Amazon EC2 menu in the AWS console\.
   + Locate the security group you created in a preceding step\.
   + Add an inbound rule allowing for HTTPS traffic from the security group created in step 1\.

1. Create a user profile\.
   + From the **SageMaker Studio Control Panel **, choose **Add User**\.
   + Provide a user name\. 
   + For the **Execution Role**, choose to create a new role or to use a pre\-existing role\.
     + If you choose **Create a new role**, you are presented the option to provide an Amazon S3 bucket name, and a policy is generated for you\.
     + If you already have a role created with permissions to the Amazon S3 buckets to which you require access, select the role from the dropdown list\. This role should have the `AmazonSageMakerFullAccess` policy attached to it\.
   + Choose **Submit**\. 

1. Create a data flow \(follow the data scientist guide outlined in a preceding section\)\. 
   + When adding a Snowflake connection, enter the value of `privatelink-account-name` \(from the *Set up Snowflake PrivateLink Integration* step\) into the **Snowflake account name \(alphanumeric\)** field, instead of the plain Snowflake account name\. Everything else is left unchanged\.

## Import Data From Software as a Service \(SaaS\) Platforms<a name="data-wrangler-import-saas"></a>

You can use Data Wrangler to import data from more than forty software as a service \(SaaS\) platforms\. To import your data from your SaaS platform, you or your administrator must use Amazon AppFlow to transfer the data from the platform to Amazon S3 or Amazon Redshift\. For more information about Amazon AppFlow, see [What is Amazon AppFlow?](https://docs.aws.amazon.com/appflow/latest/userguide/what-is-appflow.html) If you don't need to use Amazon Redshift, we recommend transferring the data to Amazon S3 for a simpler process\.

Data Wrangler supports transferring data from the following SaaS platforms:
+ [Amplitude](https://docs.aws.amazon.com/appflow/latest/userguide/amplitude.html)
+ [CircleCI](https://docs.aws.amazon.com/appflow/latest/userguide/connectors-circleci.html)
+ [DocuSign Monitor](https://docs.aws.amazon.com/appflow/latest/userguide/connectors-docusign-monitor.html)
+ [Domo](https://docs.aws.amazon.com/appflow/latest/userguide/connectors-domo.html)
+ [Datadog](https://docs.aws.amazon.com/appflow/latest/userguide/datadog.html)
+ [Dynatrace](https://docs.aws.amazon.com/appflow/latest/userguide/dynatrace.html)
+ [Facebook Ads](https://docs.aws.amazon.com/appflow/latest/userguide/connectors-facebook-ads.html)
+ [Facebook Page Insights](https://docs.aws.amazon.com/appflow/latest/userguide/connectors-facebook-page-insights.html)
+ [Google Ads](https://docs.aws.amazon.com/appflow/latest/userguide/connectors-google-ads.html)
+ [Google Analytics 4](https://docs.aws.amazon.com/appflow/latest/userguide/connectors-google-analytics-4.html)
+ [Google Search Console](https://docs.aws.amazon.com/appflow/latest/userguide/connectors-google-search-console.html)
+ [GitHub](https://docs.aws.amazon.com/appflow/latest/userguide/connectors-github.html)
+ [GitLab](https://docs.aws.amazon.com/appflow/latest/userguide/connectors-gitlab.html)
+ [Infor Nexus](https://docs.aws.amazon.com/appflow/latest/userguide/infor-nexus.html)
+ [Instagram Ads](https://docs.aws.amazon.com/appflow/latest/userguide/connectors-instagram-ads.html)
+ [Jira Cloud](https://docs.aws.amazon.com/appflow/latest/userguide/connectors-jira-cloud.html)
+ [LinkedIn Ads](https://docs.aws.amazon.com/appflow/latest/userguide/connectors-linkedin-ads.html)
+ [Mailchimp](https://docs.aws.amazon.com/appflow/latest/userguide/connectors-mailchimp.html)
+ [Marketo](https://docs.aws.amazon.com/appflow/latest/userguide/marketo.html)
+ [Microsoft Teams](https://docs.aws.amazon.com/appflow/latest/userguide/connectors-microsoft-teams.html)
+ [Mixpanel](https://docs.aws.amazon.com/appflow/latest/userguide/connectors-mixpanel.html)
+ [Okta](https://docs.aws.amazon.com/appflow/latest/userguide/connectors-okta.html)
+ [Salesforce](https://docs.aws.amazon.com/appflow/latest/userguide/salesforce.html)
+ [Salesforce Marketing Cloud](https://docs.aws.amazon.com/appflow/latest/userguide/connectors-salesforce-marketing-cloud.html)
+ [Salesforce Pardot](https://docs.aws.amazon.com/appflow/latest/userguide/pardot.html)
+ [SAP OData](https://docs.aws.amazon.com/appflow/latest/userguide/sapodata.html)
+ [SendGrid](https://docs.aws.amazon.com/appflow/latest/userguide/connectors-sendgrid.html)
+ [ServiceNow](https://docs.aws.amazon.com/appflow/latest/userguide/servicenow.html)
+ [Singular](https://docs.aws.amazon.com/appflow/latest/userguide/singular.html)
+ [Slack](https://docs.aws.amazon.com/appflow/latest/userguide/slack.html)
+ [Stripe](https://docs.aws.amazon.com/appflow/latest/userguide/connectors-stripe.html)
+ [Trend Micro](https://docs.aws.amazon.com/appflow/latest/userguide/trend-micro.html)
+ [Typeform](https://docs.aws.amazon.com/appflow/latest/userguide/connectors-typeform.html)
+ [Veeva](https://docs.aws.amazon.com/appflow/latest/userguide/veeva.html)
+ [Zendesk](https://docs.aws.amazon.com/appflow/latest/userguide/slack.html)
+ [Zendesk Chat](https://docs.aws.amazon.com/appflow/latest/userguide/connectors-zendesk-chat.html)
+ [Zendesk Sell](https://docs.aws.amazon.com/appflow/latest/userguide/connectors-zendesk-sell.html)
+ [Zendesk Sunshine](https://docs.aws.amazon.com/appflow/latest/userguide/connectors-zendesk-sunshine.html)
+ [Zoom Meetings](https://docs.aws.amazon.com/appflow/latest/userguide/connectors-zoom-meetings.html)

The preceding list has links to more information about setting up your data source\. You or your administrator can refer to the preceding links after you've read the following information\.

When you navigate to the **Import** tab of your Data Wrangler flow, you see data sources under the following sections:
+ **Available**
+ **Set up data sources**

You can connect to data sources under **Available** without needing additional configuration\. You can choose the data source and import your data\.

Data sources under **Set up data sources**, require you or your administrator to use Amazon AppFlow to transfer the data from the SaaS platform to Amazon S3 or Amazon Redshift\. For information about performing a transfer, see [Using Amazon AppFlow to transfer your data](#data-wrangler-import-saas-transfer)\.

After you perform the data transfer, the SaaS platform appears as a data source under **Available**\. You can choose it and import the data that you've transferred into Data Wrangler\. The data that you've transferred appears as tables that you can query\.

### Using Amazon AppFlow to transfer your data<a name="data-wrangler-import-saas-transfer"></a>

Amazon AppFlow is a platform that you can use to transfer data from your SaaS platform to Amazon S3 or Amazon Redshift without having to write any code\. To perform a data transfer, you use the AWS Management Console\.

**Important**  
You must make sure you've set up the permissions to perform a data transfer\. For more information, see [Amazon AppFlow Permissions](data-wrangler-security.md#data-wrangler-appflow-permissions)\.

After you've added permissions, you can transfer the data\. Within Amazon AppFlow, you create a *flow* to transfer the data\. A flow is a series of configurations\. You can use it to specify whether you're running the data transfer on a schedule or whether you're partitioning the data into separate files\. After you've configured the flow, you run it to transfer the data\.

For information about creating a flow, see [Creating flows in Amazon AppFlow](https://docs.aws.amazon.com/appflow/latest/userguide/create-flow.html)\. For information about running a flow, see [Activate an Amazon AppFlow flow](https://docs.aws.amazon.com/appflow/latest/userguide/run-flow.html)\.

After the data has been transferred, use the following procedure to access the data in Data Wrangler\.
**Important**  
Before you try to access your data, make sure your IAM role has the following policy:  

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "glue:SearchTables",
            "Resource": [
                "arn:aws:glue:*:*:table/*/*",
                "arn:aws:glue:*:*:database/*",
                "arn:aws:glue:*:*:catalog"
            ]
        }
    ]
}
```
By default, the IAM role that you use to access Data Wrangler is the `SageMakerExecutionRole`\. For more information about adding policies, see [Adding IAM identity permissions \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html#add-policies-console)\.

To connect to a data source, do the following\.

1. Sign into [Amazon SageMaker Console](https://console.aws.amazon.com/sagemaker)\.

1. Choose **Studio**\.

1. Choose **Launch app**\.

1. From the dropdown list, select **Studio**\.

1. Choose the Home icon\.

1. Choose **Data**\.

1. Choose **Data Wrangler**\.

1. Choose **Import data**\.

1. Under **Available**, choose the data source\.

1. For the **Name** field, specify the name of the connection\.

1. \(Optional\) Choose **Advanced configuration**\.

   1. Choose a **Workgroup**\.

   1. If your workgroup hasn't enforced the Amazon S3 output location or if you don't use a workgroup, specify a value for **Amazon S3 location of query results**\.

   1. \(Optional\) For **Data retention period**, select the checkbox to set a data retention period and specify the number of days to store the data before it's deleted\.

   1. \(Optional\) By default, Data Wrangler saves the connection\. You can choose to deselect the checkbox and not save the connection\.

1. Choose **Connect**\.

1. Specify a query\.
**Note**  
To help you specify a query, you can choose a table on the left\-hand navigation panel\. Data Wrangler shows the table name and a preview of the table\. Choose the icon next to the table name to copy the name\. You can use the table name in the query\.

1. Choose **Run**\.

1. Choose **Import query**\.

1. For **Dataset name**, specify the name of the dataset\.

1. Choose **Add**\.

When you navigate to the **Import data** screen, you can see the connection that you've created\. You can use the connection to import more data\.

## Imported Data Storage<a name="data-wrangler-import-storage"></a>

**Important**  
 We strongly recommend that you follow the best practices around protecting your Amazon S3 bucket by following [ Security best practices](https://docs.aws.amazon.com/AmazonS3/latest/userguide/security-best-practices.html)\. 

When you query data from Amazon Athena or Amazon Redshift, the queried dataset is automatically stored in Amazon S3\. Data is stored in the default SageMaker S3 bucket for the AWS Region in which you are using Studio\.

Default S3 buckets have the following naming convention: `sagemaker-region-account number`\. For example, if your account number is 111122223333 and you are using Studio in `us-east-1`, your imported datasets are stored in `sagemaker-us-east-1-`111122223333\. 

 Data Wrangler flows depend on this Amazon S3 dataset location, so you should not modify this dataset in Amazon S3 while you are using a dependent flow\. If you do modify this S3 location, and you want to continue using your data flow, you must remove all objects in `trained_parameters` in your \.flow file\. To do this, download the \.flow file from Studio and for each instance of `trained_parameters`, delete all entries\. When you are done, `trained_parameters` should be an empty JSON object:

```
"trained_parameters": {}
```

When you export and use your data flow to process your data, the \.flow file you export refers to this dataset in Amazon S3\. Use the following sections to learn more\. 

### Amazon Redshift Import Storage<a name="data-wrangler-import-storage-redshift"></a>

Data Wrangler stores the datasets that result from your query in a Parquet file in your default SageMaker S3 bucket\. 

This file is stored under the following prefix \(directory\): redshift/*uuid*/data/, where *uuid* is a unique identifier that gets created for each query\. 

For example, if your default bucket is `sagemaker-us-east-1-111122223333`, a single dataset queried from Amazon Redshift is located in s3://sagemaker\-us\-east\-1\-111122223333/redshift/*uuid*/data/\.

### Amazon Athena Import Storage<a name="data-wrangler-import-storage-athena"></a>

When you query an Athena database and import a dataset, Data Wrangler stores the dataset, as well as a subset of that dataset, or *preview files*, in Amazon S3\. 

The dataset you import by selecting **Import dataset** is stored in Parquet format in Amazon S3\. 

Preview files are written in CSV format when you select **Run** on the Athena import screen, and contain up to 100 rows from your queried dataset\. 

The dataset you query is located under the prefix \(directory\): athena/*uuid*/data/, where *uuid* is a unique identifier that gets created for each query\.

For example, if your default bucket is `sagemaker-us-east-1-111122223333`, a single dataset queried from Athena is located in `s3://sagemaker-us-east-1-111122223333`/athena/*uuid*/data/*example\_dataset\.parquet*\.

The subset of the dataset that is stored to preview dataframes in Data Wrangler is stored under the prefix: athena/\.