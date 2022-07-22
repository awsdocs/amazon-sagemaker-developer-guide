# Connect to an external data source<a name="canvas-connecting-external"></a>

Add a connection to an external data source to import data from it\. The credentials that you specify depend on the data source that you're connecting to\. For more information, see the following sections\.

## Connect to an Amazon Redshift database<a name="canvas-connecting-redshift"></a>

You can import data from Amazon Redshift, a data warehouse where your organization keeps its data\. Before you can import data from Amazon Redshift, the AWS IAM role you use must have the `AmazonRedshiftFullAccess` managed policy attached\. For instructions on how to attach this policy, see [Give Users Permissions to Import Amazon Redshift Data](canvas-redshift-permissions.md)\. 

To import data from Amazon Redshift, you do the following:

1. Create a connection to Amazon Redshift database\.

1. Choose the data that you're importing\.

1. Import the data\.

You can use the Amazon Redshift editor to drag datasets onto the import pane and import them into SageMaker Canvas\. For more control over the values returned in the dataset, you can use the following:
+ SQL queries
+ Joins

SQL queries give you the ability to customize how you import the values in the dataset\. For example, you can specify the columns returned in the dataset or the range of values for a column\.

You can use joins to combine multiple datasets from Amazon Redshift into a single dataset\. You can drag your datasets from Amazon Redshift into the panel that gives you the ability to join the datasets\.

You can use the SQL editor to edit the dataset that you've joined and convert the joined dataset into a single node\. You can join another dataset to the node\. You can import the data that you've selected into SageMaker Canvas\.

Use the following procedure to import data from Amazon Redshift\.

You can join datasets before you import them into SageMaker Canvas using SQL or the SageMaker Canvas interface\. You can consolidate the joins you make into a single node before joining them into a another node\.

1. Navigate to the import data screen\.

1. Choose **Add connection**\.

1. Choose **Amazon Redshift**\.

1. Specify your Amazon Redshift credentials\.

1. From the tab that has the name of your connection, drag the \.csv file that you're importing to the **Drag and drop table to import** pane\.

1. Optional: Drag additional tables to the import pane\. You can use the GUI to join the tables\. For more specificity in your joins, choose **Edit in SQL**\.

1. Optional: If you're using SQL to query the data, you can choose **Context** to add context to the connection by specifying values for the following:
   + **Warehouse**
   + **Database**
   + **Schema**

1. Choose **Import**\.

The following image shows an example of fields specified for an Amazon Redshift connection\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-redshift-add-connection.png)

The following image shows the page used to join datasets in Amazon Redshift\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-redshift-join.png)

The following image shows a SQL query being used to edit a join in Amazon Redshift\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-redshift-edit-sql.png)

## Use Snowflake with Amazon SageMaker Canvas<a name="canvas-using-snowflake"></a>

You can import data from your Snowflake account by doing the following:

1. Create a connection to the Snowflake database\.

1. Choose the data that you're importing by dragging and dropping the table from the left navigation menu into the editor\.

1. Import the data\.

You can use the Snowflake editor to drag datasets onto the import pane and import them into SageMaker Canvas\. For more control over the values returned in the dataset, you can use the following:
+ SQL queries
+ Joins

SQL queries give you the ability to customize how you import the values in the dataset\. For example, you can specify the columns returned in the dataset or the range of values for a column\.

You can use joins to combine multiple datasets from Snowflake into a single dataset\. You can drag your datasets from Snowflake into the panel that gives you the ability to join the datasets\.

You can combine the datasets that you've joined into a single node and join the nodes to a different Snowflake dataset\. You can import the data that you've selected into SageMaker Canvas\.

Use the following procedure to import data from Snowflake to Amazon SageMaker Canvas\.

You can join datasets before you import them into SageMaker Canvas using SQL or the SageMaker Canvas interface\. You can edit the joins in SQL and convert the SQL into a single node\. You can join other nodes to the node that you've converted\.

1. Navigate to the import data screen\.

1. Choose **Add connection**\.

1. Choose **Snowflake**\.

1. Specify your Snowflake credentials\.

1. From the tab that has the name of your connection, drag the \.csv file that you're importing to the **Drag and drop table to import** pane\.

1. Optional: Drag additional tables to the import pane\. You can use the user interface to join the tables\. For more specificity in your joins, choose **Edit in SQL**\.

1. Optional: If you're using SQL to query the data, you can choose **Context** to add context to the connection by specifying values for the following:
   + **Warehouse**
   + **Database**
   + **Schema**

   Adding context to a connection makes it easier to specify future queries\.

1. Choose **Import**\.

The following image shows an example of fields specified for a Snowflake connection\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-snowflake-connection.png)

The following image shows the page used to add context to a connection\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-connection-context.png)

The following image shows the page used to join datasets in Snowflake\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-snowflake-join.png)

The following image shows a SQL query being used to edit a join in Snowflake\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-snowflake-edit-sql.png)