# Fraud Detection with Feature Store<a name="feature-store-fraud-detection-notebook"></a>

## Step 1: Set Up Feature Store<a name="feature-store-setup"></a>

 To start using Feature Store, create a SageMaker session, boto3 session, and a Feature Store session\. Also, set up the S3 bucket you want to use for your features\. This is your offline store\. The following code uses the SageMaker default bucket and adds a custom prefix to it\. 

**Note**  
 The role that you use must have the following managed policies attached to it: `AmazonSageMakerFullAccess` and `AmazonSageMakerFeatureStoreAccess`\. 

```
import boto3
import sagemaker
from sagemaker.session import Session

sagemaker_session = sagemaker.Session()
region = sagemaker_session.boto_region_name
boto_session = boto3.Session(region_name=region)
role = sagemaker.get_execution_role()
default_bucket = sagemaker_session.default_bucket()
prefix = 'sagemaker-featurestore'
offline_feature_store_bucket = 's3://{}/{}'.format(default_bucket, prefix)

sagemaker_client = boto_session.client(service_name='sagemaker', region_name=region)
featurestore_runtime = boto_session.client(service_name='sagemaker-featurestore-runtime', region_name=region)

feature_store_session = Session(
    boto_session=boto_session,
    sagemaker_client=sagemaker_client,
    sagemaker_featurestore_runtime_client=featurestore_runtime
)
```

## Step 2: Load Datasets and Partition Data into Feature Groups<a name="feature-store-load-datasets"></a>

Load your data into data frames for each of your features\. You use these data frames after you set up the feature group\. In the fraud detection example, you can see these steps in the following code\. 

```
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import io

fraud_detection_bucket_name = 'sagemaker-featurestore-fraud-detection'
identity_file_key = 'sampled_identity.csv'
transaction_file_key = 'sampled_transactions.csv'

identity_data_object = s3_client.get_object(Bucket=fraud_detection_bucket_name, Key=identity_file_key)
transaction_data_object = s3_client.get_object(Bucket=fraud_detection_bucket_name, Key=transaction_file_key)

identity_data = pd.read_csv(io.BytesIO(identity_data_object['Body'].read()))
transaction_data = pd.read_csv(io.BytesIO(transaction_data_object['Body'].read()))

identity_data = identity_data.round(5)
transaction_data = transaction_data.round(5)

identity_data = identity_data.fillna(0)
transaction_data = transaction_data.fillna(0)

# Feature transformations for this dataset are applied before ingestion into FeatureStore.
# One hot encode card4, card6
encoded_card_bank = pd.get_dummies(transaction_data['card4'], prefix = 'card_bank')
encoded_card_type = pd.get_dummies(transaction_data['card6'], prefix = 'card_type')

transformed_transaction_data = pd.concat([transaction_data, encoded_card_type, encoded_card_bank], axis=1)
transformed_transaction_data = transformed_transaction_data.rename(columns={"card_bank_american express": "card_bank_american_express"})
```

## Step 3: Set Up Feature Groups<a name="feature-store-set-up-feature-groups-fraud-detection"></a>

When you set up your feature groups, you need to customize the feature names with a unique name and set up each feature group with the `FeatureGroup` class\.  

```
from sagemaker.feature_store.feature_group import FeatureGroup
feature_group_name = "some string for a name"
feature_group = FeatureGroup(name=feature_group_name, sagemaker_session=feature_store_session)
```

 For example, in the fraud detection example, the two feature groups are `identity` and `transaction`\. In the following code you can see how the names are customized with a timestamp, and then each group is set up by passing in the name and the session\. 

```
import time
from time import gmtime, strftime, sleep
from sagemaker.feature_store.feature_group import FeatureGroup

identity_feature_group_name = 'identity-feature-group-' + strftime('%d-%H-%M-%S', gmtime())
transaction_feature_group_name = 'transaction-feature-group-' + strftime('%d-%H-%M-%S', gmtime())

identity_feature_group = FeatureGroup(name=identity_feature_group_name, sagemaker_session=feature_store_session)
transaction_feature_group = FeatureGroup(name=transaction_feature_group_name, sagemaker_session=feature_store_session)
```

## Step 4: Set Up Record Identifier and Event Time Features<a name="feature-store-set-up-record-identifier-event-time"></a>

In this step, you specify a record identifier name and an event time feature name\. This name maps to the column of the corresponding features in your data\. For example, in the fraud detection example, the column of interest is `TransactionID`\. `EventTime` can be appended to your data when no timestamp is available\. In the following code, you can see how these variables are set, and then `EventTime` is appended to both feature’s data\. 

```
record_identifier_name = "TransactionID"
event_time_feature_name = "EventTime"
current_time_sec = int(round(time.time()))
identity_data[event_time_feature_name] = pd.Series([current_time_sec]*len(identity_data), dtype="float64")
transformed_transaction_data[event_time_feature_name] = pd.Series([current_time_sec]*len(transaction_data), dtype="float64")
```

## Step 5: Load Feature Definitions<a name="feature-store-load-feature-definitions"></a>

 You can now load the feature definitions by passing a data frame containing the feature data\. In the following code for the fraud detection example, the identity feature and transaction feature are each loaded by using `load_feature_definitions`, and this function automatically detects the data type of each column of data\. For developers using a schema rather than automatic detection, see the [Export Feature Groups from Data Wrangler](https://docs.aws.amazon.com/sagemaker/latest/dg/data-wrangler-data-export.html#data-wrangler-data-export-feature-store) example for code that shows how to load the schema, map it, and add it as a `FeatureDefinition` that you can use to create the `FeatureGroup`\. This example also covers a boto3 implementation, which you can use instead of the SageMaker Python SDK\. 

```
identity_feature_group.load_feature_definitions(data_frame=identity_data); # output is suppressed
transaction_feature_group.load_feature_definitions(data_frame=transformed_transaction_data); # output is suppressed
```

## Step 6: Create a Feature Group<a name="feature-store-setup-create-feature-group"></a>

In this step, you use the `create` function to create the feature group\. The following code shows all of the available parameters\. The online store is not created by default, so you must set this as `True` if you want to enable it\. The `s3_uri` is the S3 bucket location of your offline store\. 

```
# create a FeatureGroup
feature_group.create(
    description = "Some info about the feature group",
    feature_group_name = feature_group_name,
    record_identifier_name = record_identifier_name,
    event_time_feature_name = event_time_feature_name,
    feature_definitions = feature_definitions,
    role_arn = role,
    s3_uri = offline_feature_store_bucket,
    enable_online_store = True,
    online_store_kms_key_id = None,
    offline_store_kms_key_id = None,
    disable_glue_table_creation = False,
    data_catalog_config = None,
    tags = ["tag1","tag2"])
```

 The following code from the fraud detection example shows a minimal `create` call for each of the two features groups being created\.  

```
identity_feature_group.create(
    s3_uri=offline_feature_store_bucket,
    record_identifier_name=record_identifier_name,
    event_time_feature_name=event_time_feature_name,
    role_arn=role,
    enable_online_store=True
)

transaction_feature_group.create(
    s3_uri=offline_feature_store_bucket,
    record_identifier_name=record_identifier_name,
    event_time_feature_name=event_time_feature_name,
    role_arn=role,
    enable_online_store=True
)
```

When you create a feature group, it takes time to load the data, and you need to wait until the feature group is created before you can use it\. You can check status using the following method\. 

```
status = feature_group.describe().get("FeatureGroupStatus")
```

 While the feature group is being created, you receive `Creating` as a response\. When this step has finished successfully, the response is `Created`\. Other possible statuses are `CreateFailed`, `Deleting`, or `DeleteFailed`\. 

## Step 7: Work with Feature Groups<a name="feature-store-working-with-feature-groups"></a>

Now that you've set up your feature group, you can perform any of the following tasks:

**Topics**
+ [Describe a Feature Group](#feature-store-describe-feature-groups)
+ [List Feature Groups](#feature-store-list-feature-groups)
+ [Put Records in a Feature Group](#feature-store-put-records-feature-group)
+ [Get Records from a Feature Group](#feature-store-get-records-feature-group)
+ [Generate Hive DDL Commands](#feature-store-generate-hive-ddl-commands-feature-group)
+ [Build a Training Dataset](#feature-store-build-training-dataset)
+ [Write and Execute an Athena Query](#feature-store-write-athena-query)
+ [Delete a Feature Group](#feature-store-delete-feature-group)

### Describe a Feature Group<a name="feature-store-describe-feature-groups"></a>

 You can retrieve information about your feature group with the `describe` function\. 

```
feature_group.describe()
```

### List Feature Groups<a name="feature-store-list-feature-groups"></a>

 You can list all of your feature groups with the `list_feature_groups` function\. 

```
sagemaker_client.list_feature_groups()
```

### Put Records in a Feature Group<a name="feature-store-put-records-feature-group"></a>

 You can use the `ingest` function to load your feature data\. You pass in a data frame of feature data, set the number of workers, and choose to wait for it to return or not\. The following example demonstrates using the `ingest` function\. 

```
feature_group.ingest(
    data_frame=feature_data, max_workers=3, wait=True
)
```

 For each feature group you have, run the `ingest` function on the feature data you want to load\. 

### Get Records from a Feature Group<a name="feature-store-get-records-feature-group"></a>

 You can use the `get_record` function to retrieve the data for a specific feature by its record identifier\. The following example uses an example identifier to retrieve the record\. 

```
record_identifier_value = str(2990130)
featurestore_runtime.get_record(FeatureGroupName=transaction_feature_group_name, RecordIdentifierValueAsString=record_identifier_value)
```

 An example response from the fraud detection example: 

```
...
'Record': [{'FeatureName': 'TransactionID', 'ValueAsString': '2990130'},
  {'FeatureName': 'isFraud', 'ValueAsString': '0'},
  {'FeatureName': 'TransactionDT', 'ValueAsString': '152647'},
  {'FeatureName': 'TransactionAmt', 'ValueAsString': '75.0'},
  {'FeatureName': 'ProductCD', 'ValueAsString': 'H'},
  {'FeatureName': 'card1', 'ValueAsString': '4577'},
...
```

### Generate Hive DDL Commands<a name="feature-store-generate-hive-ddl-commands-feature-group"></a>

 The SageMaker Python SDK’s `FeatureStore` class also provides the functionality to generate Hive DDL commands\. The schema of the table is generated based on the feature definitions\. Columns are named after feature name and data\-type are inferred based on feature type\. 

```
print(feature_group.as_hive_ddl())
```

Example output: 

```
CREATE EXTERNAL TABLE IF NOT EXISTS sagemaker_featurestore.identity-feature-group-27-19-33-00 (
  TransactionID INT
  id_01 FLOAT
  id_02 FLOAT
  id_03 FLOAT
  id_04 FLOAT
 ...
```

### Build a Training Dataset<a name="feature-store-build-training-dataset"></a>

 Feature Store automatically builds an AWS Glue data catalog when you create feature groups and you can turn this off if you want\. The following describes how to create a single training dataset with feature values from both identity and transaction feature groups created earlier in this topic\. Also, the following describes how to run an Amazon Athena query to join data stored in the offline store from both identity and transaction feature groups\.  

 To start, create an Athena query using `athena_query()` for both identity and transaction feature groups\. The `table\_name` is the AWS Glue table that is autogenerated by Feature Store\.  

```
identity_query = identity_feature_group.athena_query()
transaction_query = transaction_feature_group.athena_query()

identity_table = identity_query.table_name
transaction_table = transaction_query.table_name
```

### Write and Execute an Athena Query<a name="feature-store-write-athena-query"></a>

 You write your query using SQL on these feature groups, and then execute the query with the `.run()` command and specify your S3 bucket location for the data set to be saved there\.  

```
# Athena query
query_string = 'SELECT * FROM "'+transaction_table+'" LEFT JOIN "'+identity_table+'" ON "'+transaction_table+'".transactionid = "'+identity_table+'".transactionid'

# run Athena query. The output is loaded to a Pandas dataframe.
dataset = pd.DataFrame()
identity_query.run(query_string=query_string, output_location='s3://'+default_s3_bucket_name+'/query_results/')
identity_query.wait()
dataset = identity_query.as_dataframe()
```

 From here you can train a model using this data set and then perform inference\.  

### Delete a Feature Group<a name="feature-store-delete-feature-group"></a>

 You can delete a feature group with the `delete` function\.  

```
feature_group.delete()
```

 The following code example is from the fraud detection example\. 

```
identity_feature_group.delete()
transaction_feature_group.delete()
```

For more information, see the [Delete a feature group API](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DeleteFeatureGroup.html)