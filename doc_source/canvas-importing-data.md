# Importing data in Amazon SageMaker Canvas<a name="canvas-importing-data"></a>

You can import data from different data sources into Amazon SageMaker Canvas\. The data sources include Amazon S3, your local machine, and external data sources\. Currently, you can only import \.csv files\. You can use the dataset that you import to build a model and make predictions on other datasets\.

You can import data from the following external data sources:
+ An Amazon S3 bucket from an external account\.
+ An Amazon Redshift database\.
+ Snowflake

You can import data for the following data types:
+ Categorical
+ Numeric
+ Text
+ Datetime

To import data from an external data source, create a connection\. For more information, see [Connect to an external data source](canvas-connecting-external.md)\.

To import data from multiple files on your local machine or Amazon S3 locations, you import the data from each data source and join them into a single dataset\. For information about joining datasets, see [Join data that you've imported into SageMaker Canvas](canvas-joining-data.md)\.