# Import<a name="data-wrangler-import"></a>

You can use Amazon SageMaker Data Wrangler to import data from the following *data sources*: Amazon Simple Storage Service \(Amazon S3\), Amazon Athena, Amazon Redshift, and Snowflake\.

**Topics**
+ [Import data from Amazon S3](#data-wrangler-import-s3)
+ [Import data from Athena](#data-wrangler-import-athena)
+ [Import data from Amazon Redshift](#data-wrangler-import-redshift)
+ [Import data from Snowflake](#data-wrangler-snowflake)
+ [Imported Data Storage](#data-wrangler-import-storage)

Some data sources allow you to add multiple *data connections*:
+ You can connect to multiple Amazon Redshift clusters\. Each cluster becomes a data source\. 
+ You can query any Athena database in your account to import data from that database\.

When you import a dataset from a data source, it appears in your data flow\. Data Wrangler automatically infers the data type of each column in your dataset\. To modify these types, select the **Data types** step and select **Edit data types**\.

When you import data from Athena or Amazon Redshift, the imported data is automatically stored in the default SageMaker S3 bucket for the AWS Region in which you are using Studio\. Additionally, Athena stores data you preview in Data Wrangler in this bucket\. To learn more, see [Imported Data Storage](#data-wrangler-import-storage)\.

**Important**  
 The default Amazon S3 bucket may not have the least permissive security settings like bucket policy and server\-side encryption \(SSE\)\. We strongly recommend that you [ Add a Bucket Policy To Restrict Access to Datasets Imported to Data Wrangler](https://docs.aws.amazon.com/sagemaker/latest/dg/data-wrangler-security.html#data-wrangler-security-bucket-policy)\. 

**Important**  
 In addition, if you use the managed policy for SageMaker, we strongly recommend that you scope it down to the most restricted policy that allows you to perform your use case\. For more information, see [Grant an IAM Role Permission to Use Data Wrangler](https://docs.aws.amazon.com/sagemaker/latest/dg/data-wrangler-security.html#data-wrangler-security-iam-policy)\. 

## Import data from Amazon S3<a name="data-wrangler-import-s3"></a>

Amazon Simple Storage Service \(Amazon S3\) can be used to store and retrieve any amount of data at any time, from anywhere on the web\. You can accomplish these tasks using the AWS Management Console, which is a simple and intuitive web interface, and the Amazon S3 API\. If your dataset is stored locally, we recommend that you add it to an S3 bucket for import into Data Wrangler\. To learn how, see [Uploading an object to a bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/PuttingAnObjectInABucket.html) in the Amazon Simple Storage Service Developer Guide\. 

Data Wrangler uses [S3 Select](http://aws.amazon.com/s3/features/#s3-select) to allow you to preview your Amazon S3 files in Data Wrangler\. You incur standard charges for each file preview\. To learn more about pricing, see the **Requests & data retrievals** tab on [Amazon S3 pricing](http://aws.amazon.com/s3/pricing/)\. 

**Important**  
If you plan to export a data flow and launch a Data Wrangler job, ingest data into a SageMaker feature store, or create a SageMaker pipeline, be aware that these integrations require Amazon S3 input data to be located in the same AWS region\.

You can browse all buckets in your AWS account and import CSV and Parquet files using the Amazon S3 import in Data Wrangler\. 

When you choose a dataset for import, you can rename it, specify the file type, and identify the first row as a header\. 

**Important**  
If you plan to import a CSV file, be aware that a single record must not exceed multiple lines, otherwise an error is thrown\.

**To import a dataset into Data Wrangler from Amazon S3:**

1. If you are not currently on the **Import** tab, choose **Import**\.

1. Under **Data Preparation**, choose **Amazon S3** to see the **Import S3 Data Source** view\. 

1. From the table of available S3 buckets, select a bucket and navigate to the dataset you want to import\. 

1. Select the file that you want to import\. You can import CSV and Parquet files\. If your dataset does not have a \.csv or \.parquet extension, select the data type from the **File Type** dropdown list\.

1. If your CSV file has a header, select the check box next to **Add header to table**\.

1. Use the **Preview** table to preview your dataset\. This table shows up to 100 rows\. 

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

Data Wrangler uses the default S3 bucket in the same AWS Region in which your Studio instance is located to store Athena query results\. When you import from Athena, Data Wrangler creates a new database in your Athena account named `sagemaker_data_wrangler` if one does not already exist\. It creates temporary tables in this database to move the query output to this S3 bucket\. It deletes these tables after data has been imported; however the database, `sagemaker_data_wrangler`, persists\. To learn more, see [Imported Data Storage](#data-wrangler-import-storage)\.

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

1. Enter the **Cluster Identifier** to specify to which cluster you want to connect\. Note: Enter only the cluster identifier and not the full endpoint of the Amazon Redshift cluster\.

1. Enter the **Database Name** of the database to which you want to connect to\.

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

## Import data from Snowflake<a name="data-wrangler-snowflake"></a>

You can use Snowflake as a data source in SageMaker Data Wrangler to prepare data in Snowflake for machine learning\.

With Snowflake as a data source in Data Wrangler, you can quickly and easily connect to Snowflake without writing a single line of code\. Additionally, you can join your data in Snowflake with data stored in Amazon S3 and data queried through Amazon Athena and Amazon Redshift to prepare data for machine learning\. 

Once connected, you can interactively query data stored in Snowflake, easily transform data with 300\+ pre\-configured data transformations, understand data and identify potential errors and extreme values with a set of robust pre\-configured visualization templates, and quickly identify inconsistencies in your data preparation workflow and diagnose issues before models are deployed into production\. Finally, you can export your data preparation workflow to Amazon S3 for use with other SageMaker features such as SageMaker Autopilot, Amazon Feature Store and SageMaker Pipelines\. 

### Administrator Guide<a name="data-wrangler-snowflake-admin"></a>

**Important**  
 To learn more about granular access control and best practices, see [Security Access Control](https://docs.snowflake.com/en/user-guide/security-access-control.html)\. 

This section is for Snowflake administrators who are setting up access to Snowflake from within SageMaker Data Wrangler\.

**Important**  
 Your administrator is responsible for managing and monitoring the access control within Snowflake\. This includes what data a user can access, what storage integration a user can use, and what queries a user can run\. Data Wrangler does not add a layer of access control with respect to Snowflake\. 

**Important**  
Note that granting monitor privileges can enable users the ability to see details within an object e\.g\., queries, usage within a warehouse\. 

#### Configure Snowflake with Data Wrangler<a name="data-wrangler-snowflake-admin-config"></a>

To import data from Snowflake, Snowflake admins will need to configure access from Data Wrangler via Amazon S3\.

This feature is currently not available in the opt\-in regions\.

To do this, follow these steps\.

1. Configure access permissions for the S3 Bucket\.

   **AWS Access Control Requirements**

   Snowflake requires the following permissions on an S3 bucket and directory to be able to access files in the directory\. 
   + `s3:GetObject`
   + `s3:GetObjectVersion`
   + `s3:ListBucket`

   **Create an IAM policy**

   The following describes how to configure access permissions for Snowflake in your AWS Management Console so you can use an Amazon S3 bucket to load and unload data: 
   + Log in to the AWS Management Console\.
   + From the home dashboard, choose Identity and Access Management \(IAM\):  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/iam.png)
   + Choose **Policies** from the left navigation panel\.
   + Choose **Create Policy**:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/create_policy.png)
   + Select the **JSON** tab\.
   + Add a policy document that allows Snowflake to access the S3 bucket and directory\. 

     The following policy \(in JSON format\) provides Snowflake with the required permissions to load and unload data using a single bucket and directory path\. Note: Make sure to replace `bucket` and `prefix` with your actual bucket name and directory path prefix\.

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
   + Choose the button ****Next: Tags****\.
   + Choose the button ****Next: Review****\.

     Enter the policy name \(e\.g\., `snowflake_access` and an optional description\. Choose **Create policy**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/create_policy_review.png)

1. [Create the IAM Role in AWS](https://docs.snowflake.com/en/user-guide/data-load-s3-config-storage-integration.html#step-2-create-the-iam-role-in-aws)\.

1. [Create a Cloud Storage Integration in Snowflake](https://docs.snowflake.com/en/user-guide/data-load-s3-config-storage-integration.html#step-3-create-a-cloud-storage-integration-in-snowflake)\.

1. [Retrieve the AWS IAM User for your Snowflake Account](https://docs.snowflake.com/en/user-guide/data-load-s3-config-storage-integration.html#step-4-retrieve-the-aws-iam-user-for-your-snowflake-account)\.

1. [Grant the IAM User Permissions to Access Bucket](https://docs.snowflake.com/en/user-guide/data-load-s3-config-storage-integration.html#step-5-grant-the-iam-user-permissions-to-access-bucket-objects)\.

1. Grant the data scientist's Snowflake role usage permission to the storage integration\.
   + In the Snowflake console, run `GRANT USAGE ON INTEGRATION integration_name TO snowflake_role;`
     + `integration_name` is the name of your storage integration\.
     + `snowflake_role` is the name of the default [Snowflake role](https://docs.snowflake.com/en/user-guide/security-access-control-overview.html#roles) given to the data scientist user\.

#### What information needs to be provided to the Data Scientist<a name="data-wrangler-snowflake-admin-ds-info"></a>

1. To allow your data scientist to access Snowflake from SageMaker Data Wrangler, provide them with one of following:
   + A Snowflake account name, user name and password\.
   + Create a secret with [ AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) and provide the ARN of the secret\. Use the following procedure below to create the secret for Snowflake if you choose this option\.
**Important**  
 If your data scientists use the Snowflake Credentials \(User name and Password\) option to connect to Snowflake, note that [Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) is used to store the credentials in a secret and rotates secrets as part of a best practice security plan\. The secret created in Secrets Manager is only accessible with the Studio role configured when you set up Studio User profile\. This will require you to add this permission, `secretsmanager:PutResourcePolicy` to the policy that is attached to your Studio role\.  
It is strongly recommended that you scope the role policy to use different roles for different groups of Studio users\. You can add additional resource\-based permissions for the Secrets Manager secrets\. See [Manage Secret Policy](https://docs.aws.amazon.com/secretsmanager/latest/userguide/manage_secret-policy.html) for condition keys you can use\. 
     + **How to create an Amazon Secret for Snowflake**\.
       + Sign in to the [AWS Secrets Manager console](https://console.aws.amazon.com/secretsmanager)\. 
       + Choose **Store a new secret**\. 
       + In the **Select secret type** section, select **Other type of secrets**  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/store-new-secret.png)
       + Specify the details of your custom secret as Key and Value pairs\. The name of the keys are case sensitive and so the `username` key must be `username`, the password key must be `password`, and the account ID key must be `accountid`\. If you enter any of these incorrectly, Data Wrangler will raise an error\. Quotes for `username`, `password`, and `accountid` are not required if using secret key/value\. Alternatively, you can select the Plaintext tab and enter the secret value in JSON as shown in the following example: 

         ```
         {
                 "username": "snowflake username",
                 "password": "snowflake password",
                 "accountid": "snowflake accountid"
         }
         ```
       + Choose next, and on the following screen, prefix the name of your secret with `AmazonSageMaker-`\. Additionally, add a tag with key SageMaker \(without quotes\) and value: `true` \(without quotes\)\. The rest of the fields are optional\. You can scroll to the bottom of the page and click next\. The rest of the screens are optional\. Choose next until the secret has been stored\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/store-new-secret-params.png)
       + Select on the secret name and save the ARN of the secret\. Next, choose the final **Store** button\.
       + Select the secret you just created\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/secrets.png)
       + You will see your ARN on the screen\. Provide the ARN to the data scientist if they are using the ARN to connect to Snowflake\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/arn-test.png)

1. You will need to provide the data scientist the name of the storage integration you created in Step 3: [Create a Cloud Storage Integration in Snowflake](                                      https://docs.snowflake.com/en/user-guide/data-load-s3-config-storage-integration.html#step-3-create-a-cloud-storage-integration-in-snowflake)\. This is the name of the new integration and is called `integration_name` in the `CREATE INTEGRATION` SQL command you ran which is shown below: 

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
Note: Your administrator needs to follow the Administer Guide set up above before you can use Data Wrangler within Snowflake\. 

1. Access Data Wrangler

   To start, access Data Wrangler through Amazon SageMaker Studio by following the [Prerequisites](https://docs.aws.amazon.com/sagemaker/latest/dg/data-wrangler-getting-started.html#data-wrangler-getting-started-prerequisite) steps\.

    Once you have completed the Prerequisite steps you can now access Data Wrangler in Studio\.
   + Next to the user you want to use to launch Studio, select **Open Studio**\. 
   + Once you have launched Studio, select **File** then **New** then **Terminal**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/terminal.png)
   + Enter `cat /opt/conda/share/jupyter/lab/staging/yarn.lock | grep -A 1 "@amzn/sagemaker-ui-data-prep-plugin@"` to print the version of your Studio instance\. You must have Studio version 1\.3\.0 to enable Snowflake\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/cat-command.png)
     + If you do not have this version then update your Studio version\. To do this, close your Studio window and navigate to the [SageMaker Studio Console](https://console.aws.amazon.com/sagemaker)\.
     + Next, you will select the user you are using to access Studio and then select **Delete app**\. After the deletion is complete, launch Studio again by selecting **Open Studio**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/user-details.png)
     + Follow Step 3 above again to verify your Studio version is 1\.3\.0\.

1. Create a new data flow from within Data Wrangler

   Once you have accessed Data Wrangler from within Studio and have version 1\.3\.0, select the **\+** sign on the **New data flow** card under **ML tasks and components**\. This creates a new directory in Studio with a \.flow file inside, which contains your data flow\. The \.flow file automatically opens in Studio\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/studio-ml.png)

   Alternatively, you can also create a new flow by selecting **File**, then **New**, and choosing **Flow** in the top navigation bar\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/studio-flow.png)

   When you create a new \.flow file in Studio, you may see a message at the top of the Data Wrangler interface that says: 

   **Connecting to engine**

   **Establishing connection to engine\.\.\.**  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/connect-engine.png)

1. Connect to Snowflake

   There are two ways to connect to Snowflake from within Data Wrangler\. You will only need to choose one of the two ways\. 

   1. Specify your Snowflake credentials \(Account name, user name and password\) in Data Wrangler\. 

   1. Provide an Amazon Resource Name \(ARN\) of a secret\. 
**Important**  
If you do not have your Snowflake credentials or ARN, reach out to your administrator\. Your administrator can tell you which of the preceding wo methods to use to connect to Snowflake\.

   Start on the **Import** data screen and first select **Add data source** from the top right corner dropdown menu and then select Snowflake\. The following screenshot illustrates where to find the **Snowflake** option\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-in-ui.png)

Choose an Authentication method\. For this step as previously mentioned, you can use your Snowflake credentials or ARN name\. One of the two will be** provided by your administrator\. **

Next we explain both Authentications methods and provide screenshots for each\. 

1. **Snowflake Credentials Option**\.

   Select the Basic \(user name and password\) option from the Authentication method dropdown\. Then, enter your credentials to the following fields: 
   + **Storage integration: **Provide the name of the storage integration\. This is provided from your administrator\. 
   + **Snowflake account name:** The full name of your Snowflake account\.
   + **User name: **Snowflake account user name\.
   + **Password:** Snowflake account password\.
   + **Connection Name: **Choose a connection name of your choice\.

   Select **Connect**\. 

   Below is a screenshot of this screen and an example on how these fields should be completed\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-connection.png)

1. **ARN Option**

   Select the ARN option under the **Authentication** method dropdown\. Then, provide your ARN name under Secrets Manager ARN and your storage integration which is provided by your administrator\. Lastly, create a connection name and select **Connect**\. 

   Below is a screenshot of this screen\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-connection-complete.png)

1. The workflow at this point is to connect your Snowflake account to Data Wrangler, then run some queries on your data and then finally use Data Wrangler for performing data transformations etc\.

   The next few steps explain the import and querying step from within Data Wrangler\.

   After creating your Snowflake connection you will be taken to the **Import data from Snowflake** screen\. Below is a screenshot of this\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/import-snowflake.png)

   From here, select your warehouse\. You can also optionally select your database and schema, in which case the written query should specify them\. If **Database** and **Schema** are provided in the dropdown list, the written query does not need to specify the database and schema names\.

   Your schemas and tables from your Snowflake account are listed in the left panel\. You can select and unravel these entities\. When you select a specific table, select the eye icon on the right side of each table name to preview the table\.

   The following screenshot shows the panel with your data warehouses, databases, and schemas, along with the eye icon with which you can preview your table\. Once you select the **Preview Table** icon, the schema preview of that table is generated\. You must select a warehouse before you can preview a table\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/studio-panel-snowflake.png)

   After selecting a data warehouse, database and schema, you can now write queries and select run them\. The output of your query will show under Query results\. Below is a screenshot of this\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-queries.png)

   Once you have settled on the output of your query, you can then import the output of your query into a Data Wrangler flow to perform data transformations\. 

   To do this, select **Import**, then specify a name and select **Ok**, as shown in the following screenshot\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-import.png)

   From here, transition to the **Data flow** screen to prepare your data transformation, as shown in the following screenshot\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-transition.png)

### Private Connectivity between Data Wrangler and Snowflake via AWS PrivateLink<a name="data-wrangler-security-snowflake-vpc"></a>

This sections explains how to use AWS PrivateLink to establish a private connection between Data Wrangler and Snowflake\. The steps are explained below\. 

#### Create a VPC<a name="data-wrangler-snowflake-snowflake-vpc-setup"></a>

 If you do not have a VPC set up, then follow the [Create a new VPC](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/gsg_create_vpc.html#create_vpc) instructions to create one\.

 Once you have a chosen VPC you would like to use for establishing a private connection, provide the following credentials to your Snowflake Administrator to enable AWS PrivateLink:
+ VPC ID\.
+ AWS Account ID\.
+ Your corresponding account URL you use to access Snowflake\.

**Important**  
 As per Snowflake's documentation, enabling for your Snowflake account can take up to two business days\. 

#### Set up Snowflake AWS PrivateLink Integration<a name="data-wrangler-snowflake-snowflake-vpc-privatelink-setup"></a>

 After AWS PrivateLink is enabled, retrieve the AWS PrivateLink configuration for your region by executing the following command in a Snowflake worksheet\. Log into your Snowflakes console, under worksheets enter the following: `select SYSTEM$GET_PRIVATELINK_CONFIG();` 

1. Retrieve the values for the following: `privatelink-account-name`, `privatelink_ocsp-url`, `privatelink-account-url`, and `privatelink_ocsp-url` from the resulting JSON object\. Examples of each value are below\. Store these values for later use\.

   ```
   privatelink-account-name: xxxxxxxx.region.privatelink
   privatelink-vpce-id: com.amazonaws.vpce.region.vpce-svc-xxxxxxxxxxxxxxxxx
   privatelink-account-url: xxxxxxxx.region.privatelink.snowflakecomputing.com
   privatelink_ocsp-url: ocsp.xxxxxxxx.region.privatelink.snowflakecomputing.com
   ```

1. Switch to your AWS Console and navigate to the VPC menu\.

1. From the left side panel, click the link for “Endpoints” to navigate to VPC Endpoints set up\.

   Once there, click the **Create Endpoint** button in the top left\. 

1. Select the radio button for "Find service by name"\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-radio.png)

1. In the Service Name field, paste in the value for `privatelink-vpce-id` that you retrieved in the step above and select the **Verify** button\. 

   If successful a green alert with "Service name found" will appear on your screen and the VPC and Subnet options will automatically expand\. Below is a screenshot showing this\. Note: Depending on your targeted region, your resulting screen may show another AWS region name\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-service-name-found.png)

1. Select the same VPC ID that you sent to Snowflake from the VPC dropdown menu\.

1. If you have not yet created a subnet, then following the next set of instructions on how to do this\. 

1. Select the Subnets from the VPC dropdown menu\. Then select Create subnet and follow the prompts to create a subset in your VPC\. Ensure you select the VPC ID you sent Snowflake\. 

1. Scroll down to the Security Group Configuration, select Create New Security Group\. This will open the default Security Group screen in a new tab\. In this new tab, select the Create Security Group button in the top right corner\. 

1. Provide a new security group a name \(e\.g\., datawrangler\-doc\-snowflake\-privatelink\-connection\) and a description\. Be sure to select the VPC ID you have used in previous steps\. 

1. You now will need to add two rules to allow traffic from within your VPC to this VPC endpoint\. 

   Navigate to your VPC under Your VPCs in a separate tab, and retrieve your CIDR block for your VPC\. Then select the Add Rule button in the Inbound Rules section in Select `HTTPS` for the type, leave the Source as Custom in the form, paste in the value retrieved from the above describe\-vpcs call \(e\.g\., `10.0.0.0/16`\)\. 

1. Select the Create Security Group button in the bottom right\. Retrieve the Security Group ID from the newly created security group \(e\.g\., `sg-xxxxxxxxxxxxxxxxx`\)\.

1. In the VPC Endpoint configuration screen, remove the default security group\. Paste in the security group ID in the search field and select the check box\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-security-group.png)

1. Scroll down to the bottom of the page and select the Create Endpoint button\. 

1. On success you will be navigated to page that has a link to your VPC Endpoint configuration, specified by the VPC ID\. Select the link to view the configuration in full\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-success-endpoint.png)

   Retrieve the topmost record in the DNS names list\. This can be differentiated from other DNS names because it will only include the region name \(e\.g\., us\-west\-2\), and no availability zone letter notation \(e\.g\.,us\-west\-2a\)\. Store this information for later use\.

#### Configure DNS for Snowflake Endpoints in your VPC<a name="data-wrangler-snowflake-vpc-privatelink-dns"></a>

 This section explains how to configure DNS for Snowflake Endpoints in your VPC\. This will allow your VPC to resolve requests to the Snowflake AWS PrivateLink endpoint\. 

1. Navigate to the [Route 53 menu](https://console.aws.amazon.com/route53) within your AWS console\.

1. From the left hand menu \(this might need to be expanded\) select the Hosted Zones option\.

1. Select the Create Hosted Zone button in the top right\.
   + In the domain name form field, reference the value that was stored for `privatelink-account-url` in the steps above\. In this field, your Snowflake account ID will be removed from the DNS name and will only use the value starting with the region identifier\. A Resource Record Set will also be created later for the subdomain\. E\.g\., `region.privatelink.snowflakecomputing.com`\.
   + Select the radio button for **Private Hosted Zone** in the Type Section\. Your region code may not be us\-west\-2\. Reference the DNS name returned to you by Snowflake\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-create-hosted-zone.png)
   + In the VPCs to associate with the hosted zone section, select the region in which your VPC is located and the VPC ID used in previous steps\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-vpc-hosted-zone.png)
   + Select the Created hosted zone button in the bottom right

1. Next we will create two records, one for privatelink\-account\-url and one for `privatelink_ocsp-url`\.
   + Within the Hosted Zone menu, select the Create Record Set button\.

     1. For the record name, enter your Snowflake Account ID only \(the first 8 characters in `privatelink-account-url`\)\.

     1. For record type, select CNAME\.

     1. For value, enter the DNS name for the regional VPC Endpoint you retrieved in the last step of the Set up the Snowflake AWS PrivateLink Integration section\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-quick-create-record.png)

     1. Select the Create Record button\.

     1. Repeat the above steps for the OCSP record we notated as `privatelink-ocsp-url` earlier, starting with "ocsp” through the 8 character Snowflake ID for the record name \(e\.g\., `ocsp.xxxxxxxx`\)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-quick-create-ocsp.png)

#### Configure Route 53 Resolver Inbound Endpoint for your VPC<a name="data-wrangler-snowflake-vpc-privatelink-route53"></a>

This section explains how to configure Route 53 resolvers inbound endpoints for your VPC\.

1. Navigate to the [Route 53 menu](https://console.aws.amazon.com/route53) within your AWS console\.
   + In the left hand panel in the security section, select the Security Groups option\.

1. Select the Create Security Group button in the top right corner\. 
   + Provide your security group a name \(e\.g\.,: datawranger\-doc\-route53\-resolver\-sg\) and description\.
   + Select the VPC ID used in previous steps\.
   + Create rules that allow for DNS over UDP and TCP from within the VPC CIDR block \.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-inbound-rules.png)
   + Select the **Create Security Group** button\. Note the Security Group ID because it will now add a rule to allow traffic to the VPC Endpoint Security Group\.

1. Navigate to the [Route 53 menu](https://console.aws.amazon.com/route53) within your AWS console\.
   + In the left hand panel in the resolver section, select the Inbound Endpoint option\.

1. Select the **Create Inbound Endpoint** button\. 
   + Provide an endpoint a name\.
   + In the VPC in the Region dropdown, select the VPC ID you have used in all previous steps\. 
   + In the Security Group dropdown, select the security group ID from Step 2 in this section\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-inbound-endpoint.png)
   + In the IP Addresses Section, select two Availability Zones, subnets and leave the radio selector for **Use an IP address that is selected automatically** selected\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-ip-address-1.png)
   + Select the Submit button\.

1. Select the Inbound Endpoint after it was created\.

1. Once the Inbound Endpoint is created, note the two IP addresses for the resolvers\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-ip-addresses-2.png)

#### SageMaker VPC Endpoints<a name="data-wrangler-snowflake-sagemaker-vpc-endpoints"></a>

 This section explains how to create VPC endpoints for the following: ** SageMaker Studio**, ** SageMaker Notebook**, ** SageMaker API**, ** SageMaker Runtime**, **SageMaker FeatureStore Runtime**\.

##### <a name="data-wrangler-snowflake-sagemaker-vpc-endpoints-create-security-group"></a>

**Create a security group that will be applied to all endpoints\.**

1. Navigate to the [EC2 menu](https://console.aws.amazon.com/ec2) in the AWS Console\.

1. From the left hand panel, select the Security Groups option, in the Network & Security section\.

1. Select the Create Security Group button in the upper right hand corner\.

1. Provide your security group a name and description \(e\.g\., `datawrangler-doc-sagemaker-vpce-sg`\)\. **Note that a rule will be added to allow traffic over HTTPS from SageMaker to this group later\. **

##### <a name="data-wrangler-snowflake-sagemaker-vpc-endpoints-creating-endpoints"></a>

**Creating the endpoints\.**

1. Navigate to the [VPC menu](https://console.aws.amazon.com/vpc) in the AWS console\.

1. Select the **Endpoints** option from the left hand panel\.

1. Select the **Create Endpoint** button\.

1. Search for the service in the search bar \(e\.g\., enter “sagemaker” in the search bar\)\.

1. From the **VPC **dropdown, select the VPC in which your Snowflake AWS PrivateLink connection exists\.

1. In the **Subnets** section, select the Subnets which have access to the Snowflake PrivateLink connection\.

1. Leave the **Enable DNS Name** checkbox ticked\.

1. In the **Security Groups** section, select the security group you created in the section above\.

1. Select the **Create Endpoint **button\.

#### <a name="data-wrangler-snowflake-sagemaker-vpc-endpoints-studio-configuration"></a>

**Configure SageMaker Studio and SageMaker Data Wrangler**

This section explains how to configure SageMaker Studio and SageMaker Data Wrangler\.

1. Configure Security Group
   + Navigate to the EC2 menu in the AWS Console\.
   + From the left hand panel, select the Security Groups option, in the Network & Security section\.
   + Select the **Create Security Group** button in the upper right hand corner\. 
   + Provide your security group a name and description \(e\.g\., `datawrangler-doc-sagemaker-studio`\)\. 
   + Create the following inbound rules\.
     + HTTPS to the Security Group you provisioned for the Snowflake PrivateLink connection\.
       + As created in Set up the Snowflake PrivateLink Integration step\.
     + HTTP to the Security Group you provisioned for the Snowflake PrivateLink connection\.
       + As created in Set up the Snowflake PrivateLink Integration step\.
     + UDP and TCP for DNS \(port 53\) to Route 53 Resolver Inbound Endpoint security group\. 
       + As created in Step 2 of **Configure Route 53 Resolver Inbound Endpoint for your VPC\.**\.
   + Select the **Create Security Group** button in the lower right hand corner\.

1. Configure SageMaker Studio
   + Navigate to the SageMaker menu in the AWS console\.
   + From the left hand console, Select the **SageMaker Studio** option\.
   + If you do not have any domains configured, the Get Started menu is present\.
   + Select the **Standard Setup** option from the Get Started menu\.
   + For **Authentication method**, select AWS Identity and Access Management \(IAM\)\.
   + Under the **Permissions** menu you can create a new role or use a pre\-existing role, depending on your use case\.
     + If you chose **Create a new role **you are presented the option to provide an S3 bucket name, and a policy is generated for you\.
     + If you already have a role created with permissions to the S3 buckets you require access to, select the role from the dropdown\. This role should have the **AmazonSageMakerFullAccess** policy attached to it\.
   + Select the **Network and Storage** dropdown to configure the VPC, Security, and Subnets SageMaker will use\.
     + For **VPC** select the VPC in which your Snowflake PrivateLink connection exists\.
     + For **Subnet\(s\) **select the Subnets which have access to the Snowflake PrivateLink connection\.
     + For **Network Access for Studio **select VPC Only\.
     + For **Security Group\(s\)** select the security group you created earlier in Step 1\.
   + Select the **Submit** button in the bottom right hand corner\.

1. Edit the SageMaker Security Group\.
   + Create the following inbound rules:
     + Port 2049 to the inbound and outbound NFS Security Groups created automatically by SageMaker in Step 2 \(the security group names will contain the Studio domain ID\)\.
     + Access to all TCP ports to itself \(required for SageMaker for VPC Only\)\.

1. Edit the VPC Endpoint Security Groups:
   + Navigate to the EC2 menu in the AWS console\.
   + Locate the Security Group you created earlier\.
   + Add an inbound rule allowing for HTTPS traffic from the security group created in Step 1\.

1. Create a user profile\.
   + From the **SageMaker Studio Control Panel **select the **Add User** button in the top right\.
   + Provide a user name\. 
   + For the **Execution Role**, choose to create a new role or to use a pre\-existing role\.
     + If you chose **Create a new role **you are presented the option to provide an Amazon S3 bucket name, and a policy is generated for you\.
     + If you already have a role created with permissions to the Amazon S3 buckets you require access to, select the role from the dropdown\. This role should have the **AmazonSageMakerFullAccess** policy attached to it\.
   + Select the **Submit** button\. 

1. Create a Data Flow \(follow the Data Scientist guide outlined above\)\. 
   + When adding a Snowflake connection enter the value of `privatelink-account-name` \(from Step **Set up Snowflake PrivateLink Integration**\) into the Snowflake account name \(alphanumeric\) field, instead of the plain Snowflake account name\. Everything else is left unchanged\.

## Imported Data Storage<a name="data-wrangler-import-storage"></a>

**Important**  
 We strongly recommend that you follow the best practices around protecting your Amazon S3 bucket by following [ Security best practices](https://docs.aws.amazon.com/AmazonS3/latest/userguide/security-best-practices.html)\. 

When you query data from Amazon Athena or Amazon Redshift, the queried dataset is automatically stored in Amazon S3\. Data is stored in the default SageMaker S3 bucket for the AWS Region in which you are using Studio\.

Default S3 buckets have the following naming convention: sagemaker\-*region*\-*account number*\. For example, if your account number is 111122223333 and you are using Studio in us\-east\-1, your imported datasets are stored in sagemaker\-us\-east\-1\-111122223333\. 

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

For example, if your default bucket is `sagemaker-us-east-1-111122223333`, a single dataset queried from Athena is located in `s3://sagemaker-us-east-1-111122223333`/redshift/*uuid*/data/*example\_dataset\.parquet*\. 

The subset of the dataset that is stored to preview dataframes in Data Wrangler is stored under the prefix: athena/\.