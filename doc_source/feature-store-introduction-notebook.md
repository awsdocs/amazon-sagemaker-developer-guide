# Introduction to Feature Store<a name="feature-store-introduction-notebook"></a>

The example code in this topic refers to the [Introduction to Amazon SageMaker Feature Store](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-featurestore/feature_store_introduction.html) example notebook\. It is recommended that you run this notebook in Amazon SageMaker Studio because the code in this guide is conceptual and not fully functional if copied\. 

## Step 1: Set Up<a name="feature-store-setup"></a>

 To start using Feature Store, create SageMaker, boto3 and a Feature Store sessions\. Then set up the S3 bucket you want to use for your features\. This is your offline store\. The following code uses the SageMaker default bucket and adds a custom prefix to it\. 

**Note**  
 The role that you use must have the following managed policies attached to it: `AmazonS3FullAccess` and `AmazonSageMakerFeatureStoreAccess`\. 

```
# SageMaker Python SDK version 2.x is required
import sagemaker
import sys
```

```
import boto3
import pandas as pd
import numpy as np
import io
from sagemaker.session import Session
from sagemaker import get_execution_role

prefix = 'sagemaker-featurestore-introduction'
role = get_execution_role()

sagemaker_session = sagemaker.Session()
region = sagemaker_session.boto_region_name
s3_bucket_name = sagemaker_session.default_bucket()
```

## Step 2: Inspect your data<a name="feature-store-load-datasets"></a>

In this notebook example we ingest synthetic data from the [Github repository](https://github.com/aws/amazon-sagemaker-examples/tree/master/sagemaker-featurestore/data) that hosts the full notebook\.

```
customer_data = pd.read_csv("data/feature_store_introduction_customer.csv")
orders_data = pd.read_csv("data/feature_store_introduction_orders.csv")

print(customer_data.head())
print(orders_data.head())
```

The following diagram illustrates the steps the data goes through before it is ingested into Feature Store\. In this notebook, we illustrate the use\-case where you have data from multiple sources and want to store them independently in a feature store\. Our example considers data from a data warehouse \(customer data\), and data from a real\-time streaming service \(order data\)\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/feature-store-intro-diagram.png)

## Step 3: Create feature groups<a name="feature-store-set-up-feature-groups"></a>

We first start by creating feature group names for customer\_data and orders\_data\. Following this, we create two Feature Groups, one for `customer_data` and another for `orders_data`\.

```
import time
customers_feature_group_name = 'customers-feature-group-' + strftime('%d-%H-%M-%S', gmtime())
orders_feature_group_name = 'orders-feature-group-' + strftime('%d-%H-%M-%S', gmtime())

current_time_sec = int(round(time.time()))
record_identifier_feature_name = "customer_id"
```

Append `EventTime` feature to your data frame\. This parameter is required, and time stamps each data point\.

```
customer_data["EventTime"] = pd.Series([current_time_sec]*len(customer_data), dtype="float64")
orders_data["EventTime"] = pd.Series([current_time_sec]*len(orders_data), dtype="float64")
```

Load feature definitions to your feature group\.

```
customers_feature_group.load_feature_definitions(data_frame=customer_data)
orders_feature_group.load_feature_definitions(data_frame=orders_data)
```

Below we call create to create two feature groups, `customers_feature_group` and `orders_feature_group` respectively\.

```
customers_feature_group.create(
    s3_uri=f"s3://{s3_bucket_name}/{prefix}",
    record_identifier_name=record_identifier_feature_name,
    event_time_feature_name="EventTime",
    role_arn=role,
    enable_online_store=True
)

orders_feature_group.create(
    s3_uri=f"s3://{s3_bucket_name}/{prefix}",
    record_identifier_name=record_identifier_feature_name,
    event_time_feature_name="EventTime",
    role_arn=role,
    enable_online_store=True
)
```

To confirm that your FeatureGroup has been created we use `DescribeFeatureGroup` and `ListFeatureGroups` APIs to display the created feature group\. 

```
customers_feature_group.describe()
```

```
orders_feature_group.describe()
```

```
sagemaker_session.boto_session.client('sagemaker', region_name=region).list_feature_groups() # We use the boto client to list FeatureGroups
```

## Step 4: Ingest data into a feature group<a name="feature-store-set-up-record-identifier-event-time"></a>

After the FeatureGroups have been created, we can put data into the FeatureGroups\. If you are using the SageMaker Python SDK, use the `ingest` API call\. If you are using by using boto3 then use the `PutRecord` API\. It will take less than 1 minute to ingest data both of these FeatureGroups\. This example uses the SageMaker Python SDK, and so it uses the `ingest` API call\. 

```
def check_feature_group_status(feature_group):
    status = feature_group.describe().get("FeatureGroupStatus")
    while status == "Creating":
        print("Waiting for Feature Group to be Created")
        time.sleep(5)
        status = feature_group.describe().get("FeatureGroupStatus")
    print(f"FeatureGroup {feature_group.name} successfully created.")

check_feature_group_status(customers_feature_group)
check_feature_group_status(orders_feature_group)
```

```
customers_feature_group.ingest(
    data_frame=customer_data, max_workers=3, wait=True
)
```

```
orders_feature_group.ingest(
    data_frame=orders_data, max_workers=3, wait=True
)
```

Using an arbirary customer record id, 573291 we use `get_record` to check that the data has been ingested into the feature group\.

```
customer_id = 573291
sample_record = sagemaker_session.boto_session.client('sagemaker-featurestore-runtime', region_name=region).get_record(FeatureGroupName=customers_feature_group_name, RecordIdentifierValueAsString=str(customer_id))
```

```
sample_record
```

## Step 5: Clean up<a name="feature-store-load-feature-definitions"></a>

Here we remove the Feature Groups we created\.

```
customers_feature_group.delete()
orders_feature_group.delete()
```

## Step 6: Next steps<a name="feature-store-setup-create-feature-group"></a>

In this example notebook, you learned how to quickly get started with Feature Store, create feature groups, and ingest data into them\.

For an advanced example on how to use Feature Store for a Fraud Detection use\-case, see [Fraud Detection with Feature Store](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-featurestore/sagemaker_featurestore_fraud_detection_python_sdk.html)\.

## Step 7: Programmers note<a name="feature-store-working-with-feature-groups"></a>

In this notebook we used a variety of different API calls\. Most of them are accessible through the Python SDK, however some only exist within boto3\. You can invoke the Python SDK API calls directly on your Feature Store objects, whereas to invoke API calls that exist within boto3, you must first access a boto client through your boto and sagemaker sessions: e\.g\., `sagemaker_session.boto_session.client()`\.

Below we list API calls used in this notebook that exist within the Python SDK and ones that exist in boto3 for your reference\. 

**Python SDK API Calls **

```
describe()
ingest()
delete()
create()
load_feature_definitions()
```

**Boto3 API Calls**

```
list_feature_groups()
get_record()
```