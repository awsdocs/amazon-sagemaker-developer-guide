# Export<a name="data-wrangler-data-export"></a>

In your Data Wrangler flow, you can export some or all of the transformations that you've made to your data processing pipelines\.

A *Data Wrangler flow* is the series of data preparation steps that you've performed on your data\. In your data preparation, you perform one or more transformations to your data\. Each transformation is done using a transform step\. The flow has a series of nodes that represent the import of your data and the transformations that you've performed\. For an example of nodes, see the following image\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/data-wrangler-destination-nodes-photo-0.png)

The preceding image shows a Data Wrangler flow with two nodes\. The **Source \- sampled** node shows the data source from which you've imported your data\. The **Data types** node indicates that Data Wrangler has performed a transformation to convert the dataset into a usable format\. 

Each transformation that you add to the Data Wrangler flow appears as an additional node\. For information on the transforms that you can add, see [Transform Data](data-wrangler-transform.md)\. The following image shows a Data Wrangler flow that has a **Rename\-column** node to change the name of a column in a dataset\.

You can export your data transformations to the following:
+ Amazon S3
+ SageMaker Pipelines
+ Amazon SageMaker Feature Store
+ Python Code

**Important**  
We recommend that you use the IAM `AmazonSageMakerFullAccess` managed policy to grant AWS permission to use Data Wrangler\. If you don't use the managed policy, you can use an IAM policy that gives Data Wrangler access to an Amazon S3 bucket\. For more information on the policy, see [Security and Permissions](data-wrangler-security.md)\.

When you export your data flow, you're charged for the AWS resources that you use\. You can use cost allocation tags to organize and manage the costs of those resources\. You create these tags for your user\-profile and Data Wrangler automatically applies them to the resources used to export the data flow\. For more information, see [Using Cost Allocation Tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html)\.

## Export to Amazon S3<a name="data-wrangler-data-export-s3"></a>

Data Wrangler gives you the ability to export your data to a location within an Amazon S3 bucket\. You can specify the location using one of the following methods:
+ Destination node – Where Data Wrangler stores the data after it has processed it\.
+ Export to – Exports the data resulting from a transformation to Amazon S3\.
+ Export data – For small datasets, can quickly export the data that you've transformed\.

Use the following sections to learn more about each of these methods\.

------
#### [ Destination Node ]

If you want to output a series of data processing steps that you've performed to Amazon S3, you create a destination node\. A destination node tells Data Wrangler where to store the data after you've processed it\. After you create a destination node, you create a processing job to output the data\. A processing job is an Amazon SageMaker processing job\. When you're using a destination node, it runs the computational resources needed to output the data that you've transformed to Amazon S3\. 

You can use a destination node to export some of the transformations or all of the transformations that you've made in your Data Wrangler flow\.

You can use multiple destination nodes to export different transformations or sets of transformations\. The following example shows two destination nodes in a single Data Wrangler flow\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/data-wrangler-destination-nodes-photo-4.png)

You can use the following procedure to create destination nodes and export them to an Amazon S3 bucket\.

To export your data flow, you create destination nodes and a Data Wrangler job to export the data\. Creating a Data Wrangler job starts a SageMaker processing job to export your flow\. You can choose the destination nodes that you want to export after you've created them\.

Use the following procedure to create destination nodes\.

1. Choose the **\+** next to the nodes that represent the transformations that you want to export\.

1. Choose **Add destination**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/destination-nodes/destination-nodes-add-destination-0.png)

1. Choose **Amazon S3**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/destination-nodes/destination-nodes-add-destination-S3-selected.png)

1. Specify the fields shown in the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/destination-nodes/destination-nodes-add-destination-configuration.png)

1. Choose **Add destination**\.

Use the following procedure to create a Data Wrangler job\.

Create a job from the **Data flow** page and choose the destination nodes that you want to export\.

1. Choose **Create job**\. The following image shows the pane that appears after you select **Create job**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/destination-nodes/destination-nodes-create-job.png)

1. For **Job name**, specify the name of the export job\.

1. Choose the destination nodes that you want to export\.

1. \(Optional\) Specify a AWS KMS key ARN\. A AWS KMS key is a cryptographic key that you can use to protect your data\. For more information about AWS KMS keys, see [AWS Key Management Service](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html)\.

1. \(Optional\) Under **Trained parameters**\. choose **Refit** if you've done the following:
   + Sampled your dataset
   + Applied a transform that uses your data to create a new column in the dataset

   For more information about refitting the transformations you've made to an entire dataset, see [Refit Transforms to The Entire Dataset and Export Them](#data-wrangler-data-export-fit-transform)\.

1. Choose **Configure job**\. The following image shows the **Configure job** page\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/destination-nodes/destination-nodes-configure-job.png)

1. \(Optional\) Configure the Data Wrangler job\.

1. Choose **Run**\.

------
#### [ Export to ]

As an alternative to using a destination node, you can use the **Export to** option to export your Data Wrangler flow to Amazon S3 using a Jupyter Notebook\. You can choose any data node in your Data Wrangler flow and export it\. Exporting the data node exports the transformation that the node represents and the transformations that precede it\.

Use the following procedure to generate a Jupyter Notebook and run it to export your Data Wrangler flow to Amazon S3\.

1. Choose the **\+** next to the node that you want to export\.

1. Choose **Export to**\.

1. Choose **Amazon S3 \(via Jupyter Notebook\)**\.

1. Run the Jupyter Notebook\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/data-wrangler-destination-nodes-photo-export-to.png)

When you run the notebook, it exports your data flow \(\.flow file\) in the same AWS Region as the Data Wrangler flow\.

The notebook provides options that you can use to configure the processing job and the data that it outputs\.

**Important**  
We provide you with job configurations to configure the output of your data\. For the partitioning and driver memory options, we strongly recommend that you don't specify a configuration unless you already have knowledge about them\.

Under **Job Configurations**, you can configure the following:
+ `output_content_type` – The content type of the output file\. Uses `CSV` as the default format, but you can specify `Parquet`\.
+ `delimiter` – The character used to separate values in the dataset when writing to a CSV file\.
+ `compression` – If set, compresses the output file\. Uses gzip as the default compression format\.
+ `num_partitions` – The number of partitions or files that Data Wrangler writes as the output\.
+ `partition_by` – The names of the columns that you use to partition the output\.

To change the output file format from CSV to Parquet, change the value from `"CSV"` to `"Parquet"`\. For the rest of the preceding fields, uncomment the lines containing the fields that you want to specify\.

Under **\(Optional\) Configure Spark Cluster Driver Memory** you can configure Spark properties for the job, such as the Spark driver memory, in the `config` dictionary\.

The following shows the `config` dictionary\.

```
config = json.dumps({
    "Classification": "spark-defaults",
    "Properties": {
        "spark.driver.memory": f"{driver_memory_in_mb}m",
    }
})
```

To apply the configuration to the processing job, uncomment the following lines:

```
# data_sources.append(ProcessingInput(
#     source=config_s3_uri,
#     destination="/opt/ml/processing/input/conf",
#     input_name="spark-config",
#     s3_data_type="S3Prefix",
#     s3_input_mode="File",
#     s3_data_distribution_type="FullyReplicated"
# ))
```

------
#### [ Export data ]

If you have a transformation on a small dataset that you want to export quickly, you can use the **Export data** method\. When you start choose **Export data**, Data Wrangler works synchronously to export the data that you've transformed to Amazon S3\. You can't use Data Wrangler until either it finishes exporting your data or you cancel the operation\.

For information on using the **Export data** method in your Data Wrangler flow, see the following procedure\.

To use the **Export data** method\.

1. Choose a node in your Data Wrangler flow by double\-clicking on it\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/export-s3.png)

1. Configure how you want to export the data\.

1. Choose **Export data**\.

------

When you export your data flow to an Amazon S3 bucket, Data Wrangler stores a copy of the flow file in the S3 bucket\. It stores the flow file under the *data\_wrangler\_flows* prefix\. If you use the default Amazon S3 bucket to store your flow files, they use the following naming convention: `sagemaker-region-account number`\. For example, if your account number is 111122223333 and you are using Studio in us\-east\-1, your imported datasets are stored in `sagemaker-us-east-1-111122223333`\. In this example, your \.flow files created in us\-east\-1 are stored in `s3://sagemaker-region-account number/data_wrangler_flows/`\. 

## Export to SageMaker Pipelines<a name="data-wrangler-data-export-pipelines"></a>

When you want to build and deploy large\-scale machine learning \(ML\) workflows, you can use SageMaker Pipelines to create end\-to\-end workflows that manage and deploy SageMaker jobs\. SageMaker Pipelines gives you the ability to build workflows that manage your SageMaker data preparation, model training, and model deployment jobs\. You can use the first\-party algorithms that SageMaker offers by using SageMaker Pipelines\. For more information on SageMaker Pipelines, see [SageMaker Pipelines](https://docs.aws.amazon.com/sagemaker/latest/dg/pipelines.html)\.

When you export one or more steps from your data flow to SageMaker Pipelines, Data Wrangler creates a Jupyter Notebook that you can use to define, instantiate, run, and manage a pipeline\.

### Use a Jupyter Notebook to Create a Pipeline<a name="data-wrangler-pipelines-notebook"></a>

Use the following procedure to create a Jupyter Notebook to export your Data Wrangler flow to SageMaker Pipelines\.

Use the following procedure to generate a Jupyter Notebook and run it to export your Data Wrangler flow to SageMaker Pipelines\.

1. Choose the **\+** next to the node that you want to export\.

1. Choose **Export to**\.

1. Choose **SageMaker Pipelines \(via Jupyter Notebook\)**\.

1. Run the Jupyter notebook\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/data-wrangler-destination-nodes-photo-export-to.png)

The Jupyter Notebook that Data Wrangler produces can be used to define a pipeline\. The pipeline includes the data processing steps that are defined by your Data Wrangler flow\. 

You can add additional steps to your pipeline by adding steps to the `steps` list in the following code in the notebook:

```
pipeline = Pipeline(
    name=pipeline_name,
    parameters=[instance_type, instance_count],
    steps=[step_process], #Add more steps to this list to run in your Pipeline
)
```

For more information on defining pipelines, see [Define SageMaker Pipeline](https://docs.aws.amazon.com/sagemaker/latest/dg/define-pipeline.html)\.

## Export to Python Code<a name="data-wrangler-data-export-python-code"></a>

To export all steps in your data flow to a Python file that you can manually integrate into any data processing workflow, use the following procedure\.

Use the following procedure to generate a Jupyter Notebook and run it to export your Data Wrangler flow to Python Code\.

1. Choose the **\+** next to the node that you want to export\.

1. Choose **Export to**\.

1. Choose **Python Code**\.

1. Run the Jupyter Notebook\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/data-wrangler-destination-nodes-photo-export-to.png)

You might need to configure the Python script to make it run in your pipeline\. For example, if you're running a Spark environment, make sure that you are running the script from an environment that has permission to access AWS resources\.

## Export to Amazon SageMaker Feature Store<a name="data-wrangler-data-export-feature-store"></a>

You can use Data Wrangler to export features you've created to Amazon SageMaker Feature Store\. A feature is a column in your dataset\. Feature Store is a centralized store for features and their associated metadata\. You can use Feature Store to create, share, and manage curated data for machine learning \(ML\) development\. Centralized stores make your data more discoverable and reusable\. For more information about Feature Store, see [Amazon SageMaker Feature Store](https://docs.aws.amazon.com/sagemaker/latest/dg/feature-store.html)\.

A core concept in Feature Store is a feature group\. A feature group is a collection of features, their records \(observations\), and associated metadata\. It's similar to a table in a database\.

You can use Data Wrangler to do one of the following:
+ Update an existing feature group with new records\. A record is an observation in the dataset\.
+ Create a new feature group from a node in your Data Wrangler flow\. Data Wrangler adds the observations from your datasets as records in your feature group\.

If you're updating an existing feature group, your dataset's schema must match the schema of the feature group\. All the records in the feature group are replaced with the observations in your dataset\.

You can use either a Jupyter Notebook or a destination node to update your feature group with the observations in the dataset\.

------
#### [ Destination Node ]

If you want to output a series of data processing steps that you've performed to a feature group, you can create a destination node\. When you create and run a destination node, Data Wrangler updates a feature group with your data\. You can also create a new feature group from the destination node UI\. After you create a destination node, you create a processing job to output the data\. A processing job is an Amazon SageMaker processing job\. When you're using a destination node, it runs the computational resources needed to output the data that you've transformed to the feature group\. 

You can use a destination node to export some of the transformations or all of the transformations that you've made in your Data Wrangler flow\.

Use the following procedure to create a destination node to update a feature group with the observations from your dataset\.

To update a feature group using a destination node, do the following\.

1. Choose the **\+** symbol next to the node containing the dataset that you'd like to export\.

1. Under **Add destination**, choose **SageMaker Feature Store**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/feature-store-destination-node-selection.png)

1. Double click on the feature group\. Data Wrangler checks whether the schema of the feature group matches the schema of the data that you're using to update the feature group\.

1. \(Optional\) Select **Export to offline store only** for feature groups that have both an online store and an offline store\. This option only updates the offline store with observations from your dataset\.

1. After Data Wrangler validates the schema of your dataset, choose **Add**\.

Use the following procedure to create a new feature group with data from your dataset\.

You can have the following options for how you want to store your feature group:
+ Online – Low latency, high availability cache for a feature group that enables real\-time lookup of records\. The online store allows quick access to the latest value for a record in a feature group\.
+ Offline – Stores data for your feature group in an Amazon S3 bucket\. You can store your data offline when you don't need low latency \(sub\-second\) reads\. You can use an offline store for features used in data exploration, model training, and batch inference\.
+ Both online and offline – Stores your data in both an online store and an offline store\.

To create a feature group using a destination node, do the following\.

1. Choose the **\+** symbol next to the node containing the dataset that you'd like to export\.

1. Under **Add destination**, choose **SageMaker Feature Store**\.

1. Choose **Create Feature Group**\.

1. In the following dialog box, if your dataset doesn't have an event time column, select **Create "EventTime" column**\.

1. Choose **Next**\.

1. Choose **Copy JSON Schema**\. When you create a feature group, you paste the schema into the feature definitions\.

1. Choose **Create**\.

1. For **Feature group name**, specify a name for your feature group\.

1. For **Description \(optional\)**, specify a description to make your feature group more discoverable\.

1. To create a feature group for an online store, do the following\.

   1. Select **Enable storage online**\.

   1. For **Online store encryption key**, specify an AWS managed encryption key or an encryption key of your own\.

1. To create a feature group for an offline store, do the following\.

   1. Select **Enable storage offline**\.

   1. Specify values for the following fields:
      + **S3 bucket name** – The name of the bucket storing the feature group\.
      + **\(Optional\) Dataset directory name** – The Amazon S3 prefix that you're using to store the feature group\.
      + **IAM Role ARN** – The IAM role that has access to feature store\.
      + **Offline store encryption key** – By default, Feature Store uses an AWS managed key, but you can use the field to specify a key of your own\.

1. Choose **Continue**\.

1. Choose **JSON**\.

1. Remove the placeholder brackets in the window\.

1. Paste the JSON text from Step 6\.

1. Choose **Continue**\.

1. For **RECORD IDENTIFIER FEATURE NAME**, choose the column in your dataset that has unique identifiers for each record in your dataset\.

1. For **EVENT TIME FEATURE NAME**, choose the column with the timestamp values\.

1. Choose **Continue**\.

1. \(Optional\) Add tags to make your feature group more discoverable\.

1. Choose **Continue**\.

1. Choose **Create feature group**\.

1. Navigate back to your Data Wrangler flow and choose the refresh icon next to the **Feature Group** search bar\.

**Note**  
If you've already created a destination node for a feature group within a flow, you can't create another destination node for the same feature group\. If you want to create another destination node for the same feature group, you must create another flow file\.

Use the following procedure to create a Data Wrangler job\.

Create a job from the **Data flow** page and choose the destination nodes that you want to export\.

1. Choose **Create job**\. The following image shows the pane that appears after you select **Create job**\.

1. For **Job name**, specify the name of the export job\.

1. Choose the destination nodes that you want to export\.

1. \(Optional\) For **Output KMS Key**, specify an ARN, ID, or alias of an AWS KMS key\. A KMS key is a cryptographic key\. You can use the key to encrypt the output data from the job\. For more information about AWS KMS keys, see [AWS Key Management Service](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html)\.

1. The following image shows the **Configure job** page with the **Job configuration** tab open\.  
![\[\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/destination-nodes/destination-nodes-configure-job.png)

   \(Optional\) Under **Trained parameters**\. choose **Refit** if you've done the following:
   + Sampled your dataset
   + Applied a transform that uses your data to create a new column in the dataset

   For more information about refitting the transformations you've made to an entire dataset, see [Refit Transforms to The Entire Dataset and Export Them](#data-wrangler-data-export-fit-transform)\.

1. Choose **Configure job**\. The following image shows the **Configure job** page\.

1. \(Optional\) Configure the Data Wrangler job\. The following are examples of optional configurations\.

   1. For **Flow file S3 location**, specify the Amazon S3 location where you'd like to save the flow file\.

   1. For **Flow file KMS key**, specify the AWS KMS key, ARN, or alias that you're using to encrypt the flow file\.

1. Choose **Run**\.

------
#### [ Jupyter Notebook ]

Use the following procedure to a Jupyter Notebook to export to Amazon SageMaker Feature Store\.

Use the following procedure to generate a Jupyter Notebook and run it to export your Data Wrangler flow to Feature Store\.

1. Choose the **\+** next to the node that you want to export\.

1. Choose **Export to**\.

1. Choose **Amazon SageMaker Feature Store \(via Jupyter Notebook\)**\.

1. Run the Jupyter Notebook\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/data-wrangler-destination-nodes-photo-export-to.png)

Running a Jupyter Notebook runs a Data Wrangler job\. Running a Data Wrangler job starts a SageMaker processing job\. The processing job ingests the flow into an online and offline feature store\.

**Important**  
The IAM role you use to run this notebook must have the following AWS managed policies attached: `AmazonSageMakerFullAccess` and `AmazonSageMakerFeatureStoreAccess`\.

You only need to enable one online or offline feature store when you create a feature group\. You can also enable both\. To disable online store creation, set `EnableOnlineStore` to `False`:

```
# Online Store Configuration
online_store_config = {
    "EnableOnlineStore": False
}
```

The notebook uses the column names and types of the dataframe you export to create a feature group schema, which is used to create a feature group\. A feature group is a group of features defined in the feature store to describe a record\. The feature group defines the schema and features contained in the feature group\. A feature group definition is composed of a list of features, a record identifier feature name, an event time feature name, and configurations for its online store and offline store\. 

Each feature in a feature group can have one of the following types: *String*, *Fractional*, or *Integral*\. If a column in your exported dataframe is not one of these types, it defaults to `String`\. 

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
+ The *record identifier name* is the name of the feature whose value uniquely identifies a record defined in the feature store\. Only the latest record per identifier value is stored in the online store\. The record identifier feature name must be one of feature definitions' names\.
+ The *event time feature name* is the name of the feature that stores the `EventTime` of a record in a feature group\. An `EventTime` is a point in time when a new event occurs that corresponds to the creation or update of a record in a feature\. All records in the feature group must have a corresponding `EventTime`\.

The notebook uses these configurations to create a feature group, process your data at scale, and then ingest the processed data into your online and offline feature stores\. To learn more, see [Data Sources and Ingestion](https://docs.aws.amazon.com/sagemaker/latest/dg/feature-store-ingest-data.html)\.

------

The notebook uses these configurations to create a feature group, process your data at scale, and then ingest the processed data into your online and offline feature stores\. To learn more, see [Data Sources and Ingestion](https://docs.aws.amazon.com/sagemaker/latest/dg/feature-store-ingest-data.html)\.

## Refit Transforms to The Entire Dataset and Export Them<a name="data-wrangler-data-export-fit-transform"></a>

When you import data, Data Wrangler uses a sample of the data to apply the encodings\. By default, Data Wrangler uses the first 50,000 rows as a sample, but you can import the entire dataset or use a different sampling method\. For more information, see [Import](data-wrangler-import.md)\.

The following transformations use your data to create a column in the dataset:
+ [Encode Categorical](data-wrangler-transform.md#data-wrangler-transform-cat-encode)
+ [Featurize Text](data-wrangler-transform.md#data-wrangler-transform-featurize-text)
+ [Handle Outliers](data-wrangler-transform.md#data-wrangler-transform-handle-outlier)
+ [Handle Missing Values](data-wrangler-transform.md#data-wrangler-transform-handle-missing)

If you used sampling to import your data, the preceding transforms only use the data from the sample to create the column\. The transform might not have used all of the relevant data\. For example, if you use the **Encode Categorical** transform, there might have been a category in the entire dataset that wasn't present in the sample\.

You can either use a destination node or a Jupyter notebook to refit the transformations to the entire dataset, When Data Wrangler exports the transformations in the flow, it creates a SageMaker processing job\. When the processing job finishes, Data Wrangler saves the following files in either the default Amazon S3 location or an S3 location that you specify:
+ The Data Wrangler flow file that specifies the transformations that are refit to the dataset
+ The dataset with the refit transformations applied to it

You can open a Data Wrangler flow file within Data Wrangler and apply the transformations to a different dataset\. For example, if you've applied the transformations to a training dataset, you can open and use the Data Wrangler flow file to apply the transformations to a dataset used for inference\.

For a information about using destination nodes to refit transforms and export see the following pages:
+ [Export to Amazon S3](#data-wrangler-data-export-s3)
+ [Export to Amazon SageMaker Feature Store](#data-wrangler-data-export-feature-store)

Use the following procedure to run a Jupyter notebook to refit the transformations and export the data\.

To run a Jupyter Notebook and to refit the transformations and export your Data Wrangler flow, do the following\.

1. Choose the **\+** next to the node that you want to export\.

1. Choose **Export to**\.

1. Choose the location where you're exporting the data\.

1. For the `refit_trained_params` object, set `refit` to `True`\.

1. For the `output_flow` field, specify the name of the output flow file with the refit transformations\.

1. Run the Jupyter Notebook\.

## Create a Schedule to Automatically Process New Data<a name="data-wrangler-data-export-schedule-job"></a>

If you're processing data periodically, you can create a schedule to run the processing job automatically\. For example, you can create a schedule that runs a processing job automatically when you get new data\. For more information about processing jobs, see [Export to Amazon S3](#data-wrangler-data-export-s3) and [Export to Amazon SageMaker Feature Store](#data-wrangler-data-export-feature-store)\.

When you create a job you must specify an IAM role that has permissions to create the job\. By default, the IAM role that you use to access Data Wrangler is the `SageMakerExecutionRole`\.

The following permissions enable Data Wrangler to access EventBridge and allow EventBridge to run processing jobs:
+ Add the following AWS Managed policy to the Amazon SageMaker Studio execution role that provides Data Wrangler with permissions to use EventBridge:

  ```
  arn:aws:iam::aws:policy/AmazonEventBridgeFullAccess
  ```

  For more information about the policy, see [AWS managed policies for EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-use-identity-based.html#eb-full-access-policy)\.
+ Add the following policy to the IAM role that you specify when you create a job in Data Wrangler:

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Effect": "Allow",
              "Action": "sagemaker:StartPipelineExecution",
              "Resource": "arn:aws:sagemaker:Region:AWS-account-id:pipeline/data-wrangler-*"
          }
      ]
  }
  ```

  If you're using the default IAM role, you add the preceding policy to the Amazon SageMaker Studio execution role\.

  Add the following trust policy to the role to allow EventBridge to assume it\.

  ```
  {
      "Effect": "Allow",
      "Principal": {
          "Service": "events.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
  }
  ```

**Important**  
When you create a schedule, Data Wrangler creates an `eventRule` in EventBridge\. You incur charges for both the event rules that you create and the instances used to run the processing job\.  
For information about EventBridge pricing, see [Amazon EventBridge pricing](http://aws.amazon.com/eventbridge/pricing/)\. For information about processing job pricing, see [Amazon SageMaker Pricing](http://aws.amazon.com/sagemaker/pricing/)\.

You can set a schedule using one of the following methods:
+ [CRON expressions](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-create-rule-schedule.html)
**Note**  
Data Wrangler doesn't support the following expressions:  
LW\#
Abbreviations for days
Abbreviations for months
+ [RATE expressions](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-create-rule-schedule.html#eb-rate-expressions)
+ Recurring – Set an hourly or daily interval to run the job\.
+ Specific time – Set specific days and times to run the job\.

The following sections provide procedures on creating jobs\.

------
#### [ CRON ]

Use the following procedure to create a schedule with a CRON expression\.

To specify a schedule with a CRON expression, do the following\.

1. Open your Data Wrangler flow\.

1. Choose **Create job**\.

1. \(Optional\) For **Output KMS key**, specify an AWS KMS key to configure the output of the job\.

1. Choose **Next, 2\. Configure job**\.

1. Select **Associate Schedules**\.

1. Choose **Create a new schedule**\.

1. For **Schedule Name**, specify the name of the schedule\.

1. For **Run Frequency**, choose **CRON**\.

1. Specify a valid CRON expression\.

1. Choose **Create**\.

1. \(Optional\) Choose **Add another schedule** to run the job on an additional schedule\.
**Note**  
You can associate a maximum of two schedules\. The schedules are independent and don't affect each other unless the times overlap\.

1. Choose one of the following:
   + **Schedule and run now** – Data Wrangler the job runs immediately and subsequently runs on the schedules\.
   + **Schedule only** – Data Wrangler the job only runs on the schedules that you specify\.

1. Choose **Run**

------
#### [ RATE ]

Use the following procedure to create a schedule with a RATE expression\.

To specify a schedule with a RATE expression, do the following\.

1. Open your Data Wrangler flow\.

1. Choose **Create job**\.

1. \(Optional\) For **Output KMS key**, specify an AWS KMS key to configure the output of the job\.

1. Choose **Next, 2\. Configure job**\.

1. Select **Associate Schedules**\.

1. Choose **Create a new schedule**\.

1. For **Schedule Name**, specify the name of the schedule\.

1. For **Run Frequency**, choose **Rate**\.

1. For **Value**, specify an integer\.

1. For **Unit**, select one of the following:
   + **Minutes**
   + **Hours**
   + **Days**

1. Choose **Create**\.

1. \(Optional\) Choose **Add another schedule** to run the job on an additional schedule\.
**Note**  
You can associate a maximum of two schedules\. The schedules are independent and don't affect each other unless the times overlap\.

1. Choose one of the following:
   + **Schedule and run now** – Data Wrangler the job runs immediately and subsequently runs on the schedules\.
   + **Schedule only** – Data Wrangler the job only runs on the schedules that you specify\.

1. Choose **Run**

------
#### [ Recurring ]

Use the following procedure to create a schedule that runs a job on a recurring basis\.

To specify a schedule with a CRON expression, do the following\.

1. Open your Data Wrangler flow\.

1. Choose **Create job**\.

1. \(Optional\) For **Output KMS key**, specify an AWS KMS key to configure the output of the job\.

1. Choose **Next, 2\. Configure job**\.

1. Select **Associate Schedules**\.

1. Choose **Create a new schedule**\.

1. For **Schedule Name**, specify the name of the schedule\.

1. For **Run Frequency**, make sure **Recurring** is selected by default\.

1. For **Every x hours**, specify the hourly frequency that the job runs during the day\. Valid values are integers in the inclusive range of **1** and **23**\.

1. For **On days**, select one of the following options:
   + **Every Day**
   + **Weekends**
   + **Weekdays**
   + **Select Days**

   1. \(Optional\) If you've selected **Select Days**, choose the days of the week to run the job\.
**Note**  
The schedule resets every day\. If you schedule a job to run every five hours, it runs at the following times during the day:  
00:00
05:00
10:00
15:00
20:00

1. Choose **Create**\.

1. \(Optional\) Choose **Add another schedule** to run the job on an additional schedule\.
**Note**  
You can associate a maximum of two schedules\. The schedules are independent and don't affect each other unless the times overlap\.

1. Choose one of the following:
   + **Schedule and run now** – Data Wrangler the job runs immediately and subsequently runs on the schedules\.
   + **Schedule only** – Data Wrangler the job only runs on the schedules that you specify\.

1. Choose **Run**

------
#### [ Specific time ]

Use the following procedure to create a schedule that runs a job at specific times\.

To specify a schedule with a CRON expression, do the following\.

1. Open your Data Wrangler flow\.

1. Choose **Create job**\.

1. \(Optional\) For **Output KMS key**, specify an AWS KMS key to configure the output of the job\.

1. Choose **Next, 2\. Configure job**\.

1. Select **Associate Schedules**\.

1. Choose **Create a new schedule**\.

1. For **Schedule Name**, specify the name of the schedule\.

1. Choose **Create**\.

1. \(Optional\) Choose **Add another schedule** to run the job on an additional schedule\.
**Note**  
You can associate a maximum of two schedules\. The schedules are independent and don't affect each other unless the times overlap\.

1. Choose one of the following:
   + **Schedule and run now** – Data Wrangler the job runs immediately and subsequently runs on the schedules\.
   + **Schedule only** – Data Wrangler the job only runs on the schedules that you specify\.

1. Choose **Run**

------

You can use Amazon SageMaker Studio view the jobs that are scheduled to run\. Your processing jobs run within SageMaker Pipelines\. Each processing job has its own pipeline\. It runs as a processing step within the pipeline\. You can view the schedules that you've created within a pipeline\. For information about viewing a pipeline, see [View a Pipeline](pipelines-studio-list-pipelines.md)\.

Use the following procedure to view the jobs that you've scheduled\.

To view the jobs you've scheduled, do the following\.

1. Open Amazon SageMaker Studio\.

1. Open SageMaker Pipelines

1. View the pipelines for the jobs that you've created\.

   The pipeline running the job uses the job name as a prefix\. For example, if you've created a job named `housing-data-feature-enginnering`, the name of the pipeline is `data-wrangler-housing-data-feature-engineering`\.

1. Choose the pipeline containing your job\.

1. View the status of the pipelines\. Pipelines with a **Status** of **Succeeded** have run the processing job successfully\.

To stop the processing job from running, do the following:

To stop a processing job from running, delete the event rule that specifies the schedule\. Deleting an event rule stops all the jobs associated with the schedule from running\. For information about deleting a rule, see [Disabling or deleting an Amazon EventBridge rule](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-delete-rule.html)\.

You can stop and delete the pipelines associated with the schedules as well\. For information about stopping a pipeline, see [StopPipelineExecution](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_StopPipelineExecution.html)\. For information about deleting a pipeline, see [DeletePipeline](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DeletePipeline.html#API_DeletePipeline_RequestSyntax)\.