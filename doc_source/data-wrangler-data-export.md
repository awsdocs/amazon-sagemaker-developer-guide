# Export<a name="data-wrangler-data-export"></a>

When you create a Data Wrangler data flow, you can choose from four export options to easily integrate that data flow into your data processing pipeline\. Data Wrangler offers export options to SageMaker **Data Wrangler Job**, **Pipeline**, **Python code** and **Feature Store**\. 

The following options create a Jupyter Notebook to run your data flow and integrate with the respective SageMaker feature\.
+ **Data Wrangler Job**
+ **Pipeline**
+ **Feature Store**

For these options, you choose one or more steps in your data flow to export\. When you select a step, all downstream steps are also exported\. For example, if you have defined seven consecutive steps in your data flow, and you choose to export the 7th step, code is exported to run steps 1 through 7\. The Jupyter Notebooks automatically open when you select one of these export options, and you can run the notebook directly in Studio using a **Python 3 \(Data science\)** kernel\.

If you select **Python code**, a Python file is created containing all steps in your data flow\. 

When you choose an export option that creates a Jupyter Notebook and you run the notebook, it exports your data flow \(\.flow file\) to the default SageMaker S3 bucket for the AWS Region in which the data flow was created, under the prefix *data\_wrangler\_flows*\. Default S3 buckets have the following naming convention: sagemaker\-*region*\-*account number*\. For example, if your account number is 111122223333 and you are using Studio in us\-east\-1, your imported datasets are stored in `sagemaker-us-east-1-111122223333`\. In this example, your \.flow files created in us\-east\-1 are stored in s3://sagemaker\-*region*\-*account number*/data\_wrangler\_flows/\. 

**Important**  
If you do not use the IAM managed policy, `AmazonSageMakerFullAccess`, to grant AWS roles permission to use Data Wrangler, make sure you grant these roles permission to access this bucket\. See [Security and Permissions](data-wrangler-security.md) for an example IAM policy you can use to grant these permissions\.

Use the following procedure to export a data flow\. Use the sections on this page to learn more about each export option\. 

**To export your Data Wrangler flow:**

1. Save your Data Wrangler flow\. Select **File** and then select **Save Data Wrangler Flow**\.

1. Navigate to the **Export** tab\.

1. Select the last step in your Data Wrangler flow\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/export-select-step.png)

1. Choose **Export step**\.

1. Select the export option you want\. 

## Export to a Data Wrangler Job<a name="data-wrangler-data-export-processing"></a>

If you export a Data Wrangler job Jupyter Notebook, we recommend that you select **Python 3 \(Data Science\)** for the **Kernel** and run it directly in Studio to run your data flow and process your data\. Follow the instructions in the notebook to launch your Data Wrangler job\.

Data Wrangler jobs use processing jobs to process your data\. You can run these processing jobs using `ml.m5.4xl`, `ml.m5.12xl`, and `ml.m5.24xl` instances and support one instance count\. By default, the notebook that is exported from Data Wrangler sets the following `instance_count` and `instance_type`:

```
instance_count = 1
instance_type = "ml.m5.xlarge"
```

You can monitor your Data Wrangler job status in the [SageMaker console](https://console.aws.amazon.com/sagemaker/) in the **Processing** tab\. The processing job is named `data-wrangler-flow-processing-flow_id`\. The `flow_id` is defined in the notebook using the day and time the notebook is ran\. 

Additionally, you can monitor your Data Wrangler job using Amazon CloudWatch\. For additional information, see [Monitor Amazon SageMaker Processing Jobs with CloudWatch Logs and Metrics](https://docs.aws.amazon.com/sagemaker/latest/dg/processing-job.html#processing-job-cloudwatch)\. 

## Export to SageMaker Pipelines<a name="data-wrangler-data-export-pipelines"></a>

When you want to build and deploy large\-scale machine learning workflows, you can use SageMaker ML Pipelines to create end\-to\-end workflows that manage and deploy SageMaker jobs\. ML Pipelines allows you to build workflows that manage your SageMaker data preparation, model training, and model deployment jobs\. Because of this, you can take advantage of the first\-party algorithms that SageMaker offers\. To learn more about SageMaker Pipelines, see [SageMaker Pipelines](https://docs.aws.amazon.com/sagemaker/latest/dg/pipelines.html)\.

When you export one or more steps from your data flow to SageMaker Pipelines, Data Wrangler creates a Jupyter Notebook that you can use to define, instantiate, run, and manage a pipeline\.

### Use A Jupyter Notebook to Create a Pipeline<a name="data-wrangler-pipelines-notebook"></a>

The Jupyter Notebook that Data Wrangler produces can be used to define a pipeline\. The pipeline includes a data processing step that is defined by your data flow\. 

You can add additional steps to your pipeline by adding steps to the `steps` list in the following code in the notebook:

```
pipeline = Pipeline(
    name=pipeline_name,
    parameters=[instance_type, instance_count],
    steps=[step_process], #Add more steps to this list to run in your Pipeline
)
```

To learn more about defining pipelines, see [Define SageMaker Pipeline](https://docs.aws.amazon.com/sagemaker/latest/dg/define-pipeline.html)\.

## Export to Python Code<a name="data-wrangler-data-export-python-code"></a>

To export all steps in your data flow to a Python file that you can manually integrate into any data processing workflow, choose the **Export to Code** option\. 

You may need to configure the Python script to make it runnable\. For example, you may need to modify your Spark environment, and ensure you are running the script from an environment that has permission to access AWS resources\. 

## Export to the SageMaker Feature Store<a name="data-wrangler-data-export-feature-store"></a>

The SageMaker Feature Store can be used to create, share, and manage curated data for machine learning \(ML\) development\. You can configure an online and offline feature store to be a centralized store for features and associated metadata so features can be easily discovered and reused\. To learn more about the Data Wrangler feature store, see [Amazon SageMaker Feature Store](https://docs.aws.amazon.com/sagemaker/latest/dg/feature-store.html)\.

### Use A Jupyter Notebook to Add Features to a Feature Store<a name="data-wrangler-feature-store-notebook"></a>

The Jupyter Notebook that SageMaker produces can be used to process your dataset using a SageMaker Data Wrangler job, and then ingest the data into an online and offline feature store\.

**Important**  
The IAM role you use to run this notebook must have the following AWS managed policies attached: `AmazonSageMakerFullAccess` and `AmazonSageMakerFeatureStoreAccess`\.

You only need to enable one online or offline feature store when you create a feature group\. Optionally, you can enable both\. To disable online store creation, set `EnableOnlineStore` to `False`:

```
# Online Store Configuration
online_store_config = {
    "EnableOnlineStore": False
}
```

The notebook uses the column names and types of the dataframe you export to create a feature group schema, which is used to create a feature group\. A feature group is a group of features defined in the feature store to describe a record\. The feature group defines the schema and features contained in the feature group\. A feature group definition is composed of a list of features, a record identifier feature name, an event time feature name, and configurations for its online store and offline store\. 

Each feature in a feature group can have one of the following types: *String*, *Fractional*, or *Integral*\. If a column in your exported dataframe is not one of these types, it defaults to String\. 

The following is an example of a feature group schema\.

```
column_schema = [
    {
        "name": "Height",
        "type": "long"
    },
    {
        "name": "Input",
        "type": "string"
    },
    {
        "name": "Output",
        "type": "string"
    },
    {
        "name": "Sum",
        "type": "string"
    },
    {
        "name": "Time",
        "type": "string"
    }
]
```

Additionally, you must specify a record identifier name and event time feature name:
+ The record identifier name is the name of the feature whose value uniquely identifies a record defined in the feature store\. Only the latest record per identifier value is stored in the online store\. The record identifier feature name must be one of feature definitions' names\.
+ The event time feature name is the name of the feature that stores the `EventTime` of a record in a feature group\. An `EventTime` is a point in time when a new event occurs that corresponds to the creation or update of a record in a feature\. All records in the feature group must have a corresponding `EventTime`\.

The notebook uses these configurations to create a feature group, process your data at scale, and then ingest the processed data into your online and offline feature stores\. To learn more, see [Data Sources and Ingestion](https://docs.aws.amazon.com/sagemaker/latest/dg/feature-store-ingest-data.html)\.