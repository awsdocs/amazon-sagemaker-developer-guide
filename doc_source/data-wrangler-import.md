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

You can use Amazon Simple Storage Service \(Amazon S3\) to store and retrieve any amount of data at any time, from anywhere on the web\. You can accomplish these tasks using the AWS Management Console, which is a simple and intuitive web interface, and the Amazon S3 API\. If you've stored your dataset locally, we recommend that you add it to an S3 bucket for import into Data Wrangler\. To learn how, see [Uploading an object to a bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/PuttingAnObjectInABucket.html) in the Amazon Simple Storage Service User Guide\. 

Data Wrangler uses [S3 Select](http://aws.amazon.com/s3/features/#s3-select) to allow you to preview your Amazon S3 files in Data Wrangler\. You incur standard charges for each file preview\. To learn more about pricing, see the **Requests & data retrievals** tab on [Amazon S3 pricing](http://aws.amazon.com/s3/pricing/)\. 

**Important**  
If you plan to export a data flow and launch a Data Wrangler job, ingest data into a SageMaker feature store, or create a SageMaker pipeline, be aware that these integrations require Amazon S3 input data to be located in the same AWS region\.

In Data Wrangler, you can use the Amazon S3 import functionality to browse all the S3 buckets in your AWS account and import files with the following formats:

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

You can import either a single file or multiple files as a dataset\. You can use the multifile import operation when you have a dataset that is partitioned into separate files\. It takes all of the files from an Amazon S3 directory and imports them as a single dataset\. For information on the types of files that you can import and how to import them, see the following\.

------
#### [ Single File Import ]

You can import single files in the following formats:
+ Comma Separated Values \(CSV\)
+ Parquet
+ Javascript Object Notation \(JSON\)
+ Optimized Row Columnar \(ORC\)

For files formatted in JSON, Data Wrangler supports both JSON lines \(\.jsonl\) and JSON documents \(\.json\)\. When you preview your data, it automatically shows the JSON in tabular format\. For nested JSON documents that are larger than 5 MB, Data Wrangler shows the schema for the structure and the arrays as values in the dataset\. Use the **Flatten structured** and **Explode array** operators to display the nested values in tabular format\. For more information, see [Unnest JSON data](data-wrangler-transform.md#data-wrangler-transform-flatten-column) and [Explode array](data-wrangler-transform.md#data-wrangler-transform-explode-array)\.

When you choose a dataset, you can rename it, specify the file type, and identify the first row as a header\.

You can import a dataset that you've partitioned into multiple files in an Amazon S3 bucket in a single import step\.

**To import a dataset into Data Wrangler from a single file that you've stored in Amazon S3:**

1. If you are not currently on the **Import** tab, choose **Import**\.

1. Under **Data Preparation**, choose **Amazon S3** to see the **Import S3 Data Source** view\. 

1. From the table of available S3 buckets, select a bucket and navigate to the dataset you want to import\. 

1. Select the file that you want to import\. If your dataset does not have a \.csv or \.parquet extension, select the data type from the **File Type** dropdown list\.

1. If your CSV file has a header, select the check box next to **Add header to table**\.

1. Use the **Preview** table to preview your dataset\. This table shows up to 100 rows\. 

1. In the **Details** pane, verify or change the **Name** and **File Type** for your dataset\. If you add a **Name** that contains spaces, these spaces are replaced with underscores when your dataset is imported\. 

1. **Enable sampling** is selected by default\. If you do not uncheck this box, Data Wrangler will sample and import up to 100 MB of data\. To disable sampling, uncheck this check box\. 

1. Choose **Import dataset**\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/import-s3.png)

------
#### [ Multifile Import ]

The following are the requirements for importing multiple files:
+ The files must be in the same folder of your Amazon S3 bucket\.
+ The files must either share the same header, or have no header\.

Each file must be in one of the following formats:
+ CSV
+ Parquet
+ Optimized Row Columnar \(ORC\)
+ JSON

Use the following procedure to import multiple files\.

**To import a dataset into Data Wrangler from multiple files that you've stored in an Amazon S3 directory:**

1. If you are not currently on the **Import** tab, choose **Import**\.

1. Under **Data Preparation**, choose **Amazon S3** to see the **Import S3 Data Source** view\. 

1. From the table of available S3 buckets, select the bucket containing the folder that you want to import\.

1. Select the folder containing the files that you want to import\. Each file must be in one of the supported formats\. Your files must be the same data type\.

1. If your folder contains CSV files with headers, select the check box next to **First row is header**\.

1. If you're files are nested within other folders, select the check box next to **Include nested directories**\.

1. \(Optional\) Choose **Add filename column** add a column to the dataset that shows the filename for each observation\.

1. \(Optional\) By default, Data Wrangler doesn't show you a preview of a folder\. You can activate previewing by choosing the blue **Preview off** button\. A preview shows the first 10 rows of the first 10 files in the folder\. The following images show you how to activate a preview for a dataset created from nested directories\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/multifile-nested-preview-off-cropped.png)  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/multifile-nested-preview-on-cropped.png)

1. In the **Details** pane, verify or change the **Name** and **File Type** for your dataset\. If you add a **Name** that contains spaces, these spaces are replaced with underscores when your dataset is imported\. 

1. **Enable sampling** is selected by default\. If you do not uncheck the box, Data Wrangler will sample and import up to 100 MB of data\. To disable sampling, uncheck the check box\. 

1. Choose **Import dataset**\.

------

## Import data from Athena<a name="data-wrangler-import-athena"></a>

Amazon Athena is an interactive query service that makes it easy to analyze data directly in Amazon S3 using standard SQL\. With a few actions in the AWS Management Console, you can point Athena at your data stored in Amazon S3 and begin using standard SQL to run ad\-hoc queries and get results in seconds\. To learn more, see [What is Amazon Athena?](https://docs.aws.amazon.com/athena/latest/ug/what-is.html) in the Amazon Athena User Guide\. 

You can query Athena databases and import the results in Data Wrangler\. To use this import option, you must create at least one database in Athena\. To learn how, see [Getting Started](https://docs.aws.amazon.com/athena/latest/ug/getting-started.html) in the Amazon Athena User Guide\. 

Note the following about the Athena import option in Data Wrangler:
+ Data Wrangler supports using Athena workgroups to manage the query results within an AWS account\. For more information, see [Using Workgroups to Control Query Access and Costs](https://docs.aws.amazon.com/athena/latest/ug/manage-queries-control-costs-with-workgroups.html)\.
+ Data Wrangler does not support federated queries\.

Data Wrangler uses the default Amazon S3 bucket in the same AWS Region in which your Studio instance is located to store Athena query results\. It creates temporary tables in this database to move the query output to this Amazon S3 bucket\. It deletes these tables after data has been imported; however the database, `sagemaker_data_wrangler`, persists\. To learn more, see [Imported Data Storage](#data-wrangler-import-storage)\.

If you use AWS Lake Formation with Athena, make sure your Lake Formation IAM permissions do not override IAM permissions for the database `sagemaker_data_wrangler`\.



**To import a dataset into Data Wrangler from Athena:**

1. On the **Import data** screen, choose **Amazon Athena**\.

1. For **Data Catalog**, choose a data catalog\.

1. Use the **Database** dropdown list to select the database that you want to query\. When you select a database, you can preview all tables in your database using the **Table**s listed under **Details**\.

1. Choose **Advanced configuration**\.

   **Enable sampling** is selected by default\. When sampling is activated, Data Wrangler samples and imports approximately 50% of the queried data\. Unselect this check box to disable sampling\.

1. Specify a value for **Workgroup** if you're using one\.

1. Enter your query in the query editor and use the **Run** button to run the query\. After a successful query, you can preview your result under the editor\.

1. To import the results of your query, select **Import**\.

After you complete the preceding procedure, the dataset that you've queried and imported appears in the Data Wrangler flow\.

## Import data from Amazon Redshift<a name="data-wrangler-import-redshift"></a>

Amazon Redshift is a fully managed, petabyte\-scale data warehouse service in the cloud\. The first step to create a data warehouse is to launch a set of nodes, called an Amazon Redshift cluster\. After you provision your cluster, you can upload your dataset and then perform data analysis queries\. 

You can connect to and query one or more Amazon Redshift clusters in Data Wrangler\. To use this import option, you must create at least one cluster in Amazon Redshift\. To learn how, see [Getting started with Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/gsg/getting-started.html)\.

You can output the results of your Amazon Redshift query in one of the following locations:
+ The default Amazon S3 bucket
+ An Amazon S3 output location that you specify

The default Amazon S3 bucket is in the same AWS Region in which your Studio instance is located to store Amazon Redshift query results\. For more information, see [Imported Data Storage](#data-wrangler-import-storage)\.

For either the default Amazon S3 bucket or the bucket that you specify, you have the following encryption options:
+ The default AWS service\-side encryption with an Amazon S3 managed key \(SSE\-S3\)
+  An AWS Key Management Service \(KMS\) key that you specify

An AWS KMS key is an encryption key that you create and manage\. For more information on KMS keys, see [AWS Key Management Service](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html)\.

You can specify an AWS KMS key using either the key ARN or the ARN of your AWS account\.

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

1. Enter the **Database Name** of the database to which you want to connect\.

1. Enter a **Database User** to identify the user you want to use to connect to the database\. 

1. For **UNLOAD IAM Role**, enter the IAM role ARN of the role that the Amazon Redshift cluster should assume to move and write data to Amazon S3\. For more information about this role, see [Authorizing Amazon Redshift to access other AWS services on your behalf](https://docs.aws.amazon.com/redshift/latest/mgmt/authorizing-redshift-service.html) in the Amazon Redshift Cluster Management Guide\. 

1. Choose **Connect**\.

1. \(Optional\) For **Amazon S3 output location**, specify the S3 URI to store the query results\.

1. \(Optional\) For **KMS key ID**, specify the ARN of the AWS KMS key or alias\. The following image shows you where you can find either key in the AWS Management Console\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/kms-alias-redacted.png)

The following image shows all the fields from the preceding procedure\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/redshift-connection.png)

After your connection is successfully established, it appears as a data source under **Data Import**\. Select this data source to query your database and import data\.

**To query and import data from Redshift:**

1. Select the connection that you want to query from **Data Sources**\.

1. Select a **Schema**\. To learn more about Amazon Redshift Schemas, see [Schemas](https://docs.aws.amazon.com/redshift/latest/dg/r_Schemas_and_tables.html) in the Amazon Redshift Database Developer Guide\.

1. Under **Advanced configuration**, **Enable sampling** is selected by default\. If you do not uncheck this box, Data Wrangler samples and imports approximately 50% of the queried data\. Unselect this check box to disable sampling\. 

1. Enter your query in the query editor and use the **Run** button to run the query\. After a successful query, you can preview your result under the editor\.

1. Select **Import dataset** to import the dataset that has been queried\. 

1. Enter a **Dataset name**\. If you add a **Dataset name** that contains spaces, these spaces are replaced with underscores when your dataset is imported\. 

1. Select **Add**\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/import-redshift.png)

## Import data from Snowflake<a name="data-wrangler-snowflake"></a>

You can use Snowflake as a data source in SageMaker Data Wrangler to prepare data in Snowflake for machine learning\.

With Snowflake as a data source in Data Wrangler, you can quickly and easily connect to Snowflake without writing a single line of code\. Additionally, you can join your data in Snowflake with data stored in Amazon S3 and data queried through Amazon Athena and Amazon Redshift to prepare data for machine learning\. 

Once connected, you can interactively query data stored in Snowflake, transform data with 300\+ pre\-configured data transformations, understand data and identify potential errors and extreme values with a set of robust pre\-configured visualization templates, quickly identify inconsistencies in your data preparation workflow, and diagnose issues before models are deployed into production\. Finally, you can export your data preparation workflow to Amazon S3 for use with other SageMaker features such as Amazon SageMaker Autopilot, Amazon SageMaker Feature Store and Amazon SageMaker Model Building Pipelines\.

### Administrator Guide<a name="data-wrangler-snowflake-admin"></a>

**Important**  
To learn more about granular access control and best practices, see [Security Access Control](https://docs.snowflake.com/en/user-guide/security-access-control.html)\. 

This section is for Snowflake administrators who are setting up access to Snowflake from within SageMaker Data Wrangler\.

**Important**  
Your administrator is responsible for managing and monitoring the access control within Snowflake\. This includes what data a user can access, what storage integration a user can use, and what queries a user can run\. Data Wrangler does not add a layer of access control with respect to Snowflake\. 

**Important**  
Note that granting monitor privileges can enable users the ability to see details within an object, such as queries or usage within a warehouse\. 

#### Configure Snowflake with Data Wrangler<a name="data-wrangler-snowflake-admin-config"></a>

To import data from Snowflake, Snowflake admins must configure access from Data Wrangler via Amazon S3\.

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
   + From the home dashboard, choose **IAM**:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/iam.png)
   + Choose **Policies**\.\.
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

#### What information needs to be provided to the data scientist<a name="data-wrangler-snowflake-admin-ds-info"></a>

1. To allow your data scientist to access Snowflake from SageMaker Data Wrangler, provide them with one of the following:
   + A Snowflake account name, user name, and password\.
   + A secret created with [ AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) and the ARN of the secret\. Use the following procedure below to create the secret for Snowflake if you choose this option\.
**Important**  
If your data scientists use the **Snowflake Credentials \(User name and Password\)** option to connect to Snowflake, note that [Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) is used to store the credentials in a secret and rotates secrets as part of a best practice security plan\. The secret created in Secrets Manager is only accessible with the Studio role configured when you set up a Studio user profile\. This will require you to add this permission, `secretsmanager:PutResourcePolicy` to the policy that is attached to your Studio role\.  
It is strongly recommended that you scope the role policy to use different roles for different groups of Studio users\. You can add additional resource\-based permissions for the Secrets Manager secrets\. See [Manage Secret Policy](https://docs.aws.amazon.com/secretsmanager/latest/userguide/manage_secret-policy.html) for condition keys you can use\. 
     + **How to create an Amazon Secret for Snowflake**\.
       + Sign in to the [Secrets Manager console](https://console.aws.amazon.com/secretsmanager)\. 
       + Choose **Store a new secret**\. 
       + In the **Select secret type** section, select **Other type of secrets**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/store-new-secret.png)
       + Specify the details of your custom secret as key\-value pairs\. The name of the keys are case sensitive: the `username` key must be `username`, the password key must be `password`, and the account ID key must be `accountid`\. If you enter any of these incorrectly, Data Wrangler raises an error\. Quotes for `username`, `password`, and `accountid` are not required if you are using a secret key\-value\. Alternatively, you can select the **Plaintext** tab and enter the secret value in JSON as shown in the following example: 

         ```
         {
                 "username": "snowflake username",
                 "password": "snowflake password",
                 "accountid": "snowflake accountid"
         }
         ```
       + Choose **Next**, and on the following screen, prefix the name of your secret with `AmazonSageMaker-`\. Additionally, add a tag with the key SageMaker \(without quotes\) and the value: `true` \(without quotes\)\. The rest of the fields are optional\. You can scroll to the bottom of the page and choose **Next**\. The rest of the screens are optional\. Choose **Next** until the secret has been stored\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/store-new-secret-params.png)
       + Select the secret name and save the ARN of the secret\. Choose **Store**\.
       + Select the secret you just created\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/secrets.png)
       + You see your ARN on the screen\. Provide the ARN to the data scientist if they are using the ARN to connect to Snowflake\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/arn-test.png)

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

1. Enter `cat /opt/conda/share/jupyter/lab/staging/yarn.lock | grep -A 1 "@amzn/sagemaker-ui-data-prep-plugin@"` to print the version of your Studio instance\. You must have Studio version 1\.3\.0 to enable Snowflake\.   
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
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/connect-engine.png)

1. Connect to Snowflake

   There are two ways to connect to Snowflake from within Data Wrangler\. You only need to choose one of the two ways\. 

   1. Specify your Snowflake credentials \(account name, user name, and password\) in Data Wrangler\. 

   1. Provide an Amazon Resource Name \(ARN\) of a secret\. 
**Important**  
If you do not have your Snowflake credentials or ARN, reach out to your administrator\. Your administrator can tell you which of the preceding wo methods to use to connect to Snowflake\.

   Start on the **Import** data screen and first select **Add data source** from the dropdown menu, and then select **Snowflake**\. The following screenshot illustrates where to find the **Snowflake** option\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-in-ui.png)

Choose an authentication method\. For this step, as previously mentioned, you can use your Snowflake credentials or ARN name\. One of the two is** provided by your administrator\. **

Next, we explain both authentication methods and provide screenshots for each\. 

1. **Snowflake Credentials Option**\.

   Select the **Basic** \(user name and password\) option from the **Authentication method** dropdown list\. Then, enter your credentials in the following fields: 
   + **Storage integration**: Provide the name of the storage integration\. This is provided by your administrator\. 
   + **Snowflake account name**: The full name of your Snowflake account\.
   + **User name**: Snowflake account user name\.
   + **Password**: Snowflake account password\.
   + **Connection name**: Choose a connection name for your choice\.

   Select **Connect**\. 

   The following screenshot shows how to complete these fields\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/snowflake-connection.png)

1. **ARN Option**

   Select the ARN option under the **Authentication method** dropdown list\. Then, provide your ARN name under **Secrets Manager ARN** and your **Storage integration**, which is provided by your administrator\. Finally, create a **Connection name** and select **Connect**, as shown in the following screenshot\.   
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

   After selecting a data warehouse, database and schema, you can now write queries and select run them\. The output of your query shows under **Query results**, as shown in the following screenshot\.  
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
+ VPC ID\.
+ AWS Account ID\.
+ Your corresponding account URL you use to access Snowflake\.

**Important**  
As per Snowflake's documentation, enabling for your Snowflake account can take up to two business days\. 

#### Set up Snowflake AWS PrivateLink Integration<a name="data-wrangler-snowflake-snowflake-vpc-privatelink-setup"></a>

After AWS PrivateLink is enabled, retrieve the AWS PrivateLink configuration for your region by executing the following command in a Snowflake worksheet\. Log into your Snowflakes console, under worksheets enter the following: `select SYSTEM$GET_PRIVATELINK_CONFIG();` 

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

1. Provide a new security group a name \(such as `datawrangler-doc-snowflake-privatelink-connection`\) and a description\. Be sure to select the VPC ID you have used in previous steps\. 

1. Add two rules to allow traffic from within your VPC to this VPC endpoint\. 

   Navigate to your VPC under **Your VPCs** in a separate tab, and retrieve your CIDR block for your VPC\. Then choose **Add Rule** in the **Inbound Rules** section\. Select `HTTPS` for the type, leave the **Source** as **Custom** in the form, and paste in the value retrieved from the preceding `describe-vpcs` call \(such as `10.0.0.0/16`\)\. 

1. Choose **Create Security Group**\. Retrieve the **Security Group ID** from the newly created security group \(such as `sg-xxxxxxxxxxxxxxxxx`\)\.

1. In the **VPC Endpoint** configuration screen, remove the default security group\. Paste in the security group ID in the search field and select the check box\.  
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
   + Provide your security group a name \(such as `datawranger-doc-route53-resolver-sg`\) and description\.
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

For example, if your default bucket is `sagemaker-us-east-1-111122223333`, a single dataset queried from Athena is located in `s3://sagemaker-us-east-1-111122223333`/redshift/*uuid*/data/*example\_dataset\.parquet*\. 

The subset of the dataset that is stored to preview dataframes in Data Wrangler is stored under the prefix: athena/\.