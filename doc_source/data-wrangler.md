# Prepare ML Data with Amazon SageMaker Data Wrangler<a name="data-wrangler"></a>

Amazon SageMaker Data Wrangler \(Data Wrangler\) is a feature of SageMaker Studio that provides an end\-to\-end solution to import, prepare, transform, featurize, and analyze data\. You can integrate a Data Wrangler data flow into your machine learning \(ML\) workflows to simplify and streamline data pre\-processing and feature engineering using little to no coding\. You can also add your own Python scripts and transformations to customize workflows\.

The following are the core functionalities that Data Wrangler provides to help you analyze and prepare data for machine learning applications\. 
+ **Import** – Connect to and import data from Amazon Simple Storage Service \(Amazon S3\), Amazon Athena \(Athena\), and Amazon Redshift\.
+ **Data Flow** – Create a data flow to define a series of ML data prep steps\. You can use a flow to combine datasets from different data sources, identify the number and types of transformations you want to apply to datasets, and define a data prep workflow that can be easily integrated into an ML pipeline\. 
+ **Transform** – Clean and transform your dataset using standard *transforms* like string, vector, and numeric data formatting tools\. Featurize your data using transforms like text and date/time embedding and categorical encoding\.
+ **Analyze** – Analyze features in your dataset at any point in your flow\. Data Wrangler includes built\-in data visualization tools like scatter plots and histograms, as well as data analysis tools like target leakage analysis and quick modeling to understand feature correlation\. 
+ **Export** – Data Wrangler offers export options to other SageMaker services, including Data Wrangler jobs, Feature Store, and pipelines, making it easy to integrate your data prep flow into your ML workflow\. You can also export your Data Wrangler flow to Python code\. 

To start using Data Wrangler, see [Get Started with Data Wrangler](data-wrangler-getting-started.md)\.

**Topics**
+ [Get Started with Data Wrangler](data-wrangler-getting-started.md)
+ [Import](data-wrangler-import.md)
+ [Create and Use a Data Wrangler Flow](data-wrangler-data-flow.md)
+ [Transform Data](data-wrangler-transform.md)
+ [Analyze and Visualize](data-wrangler-analyses.md)
+ [Export](data-wrangler-data-export.md)
+ [Security and Permissions](data-wrangler-security.md)
+ [Release Notes](data-wrangler-release-notes.md)
+ [Troubleshoot](data-wrangler-trouble-shooting.md)
+ [Update Data Wrangler](data-wrangler-update.md)
+ [Shut Down Data Wrangler](data-wrangler-shut-down.md)