# Adding Searchable Metadata to Your Features<a name="feature-store-add-metadata"></a>

In Amazon SageMaker Feature Store, you can search through all of your features\. To make your features more discoverable, you can add metadata to them\. You can add the following types of metadata:
+ Description – A searchable description of the feature\.
+ Parameters – Searchable key\-value pairs\.

The description can have up to 255 characters\.

For parameters, you must specify a key\-value pair in your search\. You can add up to 25 parameters\.

To update the metadata of a feature, you can use either Amazon SageMaker Studio or the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateFeatureMetadata.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateFeatureMetadata.html) operation\.

Use the following procedure to update the metadata using Amazon SageMaker Studio\.

To update the feature metadata with Studio, do the following\.

1. Sign in to Studio\. For more information, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.

1. Choose **Studio**\.

1. Choose **Launch app**\.

1. From the dropdown list, select **Studio**\.

1. Choose the Home icon\.

1. Choose **Data**\.

1. Choose **Feature Store**\.

1. Choose **Feature catalog**\.

1. In the **Feature name** column, choose a feature name\.

1. Choose **Edit metadata**\.

1. In the **Description** field, add or update the description\.

1. In the **Parameters** field under **Parameters**, specify a key\-value pair for the parameter\.

1. \(Optional\) Choose **Add new parameter** to add another parameter\.

1. Choose **Save changes**\.

1. Choose **Confirm**\.

The following describes how you can use the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateFeatureMetadata.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateFeatureMetadata.html) operation for different scenarios\.

------
#### [ Add a list of parameters to a feature ]

To add a list of parameters to a feature, specify values for the following fields:
+ `FeatureGroupName`
+ `Feature`
+ `Parameters`

The following example code uses the AWS SDK for Python \(Boto3\) to add two parameters\.

```
sagemaker_client.update_feature_metadata(
    FeatureGroupName="feature_group_name",
    FeatureName="feature-name",
    ParameterAdditions=[
        {"Key": "example-key-0", "Value": "example-value-0"},
        {"Key": "example-key-1", "Value": "example-value-1"},
    ]
)
```

------
#### [ Add a description to a feature ]

To add a description to a feature, specify values for the following fields:
+ `FeatureGroupName`
+ `Feature`
+ `Description`

```
sagemaker_client.update_feature_metadata(
    FeatureGroupName="feature-group-name",
    FeatureName="feature-name",
    Description="description"
)
```

------
#### [ Remove parameters for a feature ]

To remove all parameters for a feature, do the following\.

Specify values for the following fields:
+ `FeatureGroupName`
+ `Feature`

Specify the keys for the parameters that you're removing under `ParameterRemovals`\.

```
sagemaker_client.update_feature_metadata(
    FeatureGroupName="feature_group_name",
    FeatureName="feature-name",
        ParameterRemovals=[
        {"Key": "example-key-0"},
        {"Key": "example-key-1"},
    ]
)
```

------
#### [ Remove the description for a feature ]

To remove the description for a feature, do the following\.

Specify values for the following fields:
+ `FeatureGroupName`
+ `Feature`

Specify an empty string for `Description`\.

```
sagemaker_client.update_feature_metadata(
    FeatureGroupName="feature-group-name",
    FeatureName="feature-name",
    Description=""
)
```

------

After you've updated the metadata for a feature, you can use the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeFeatureMetadata.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeFeatureMetadata.html) operation to see the updates that you've made\.

The following code goes through an example workflow using the AWS SDK for Python \(Boto3\)\.

## Example Code<a name="feature-store-add-metadata-python-sdk-example"></a>

The example code does the following:

1. Sets up your SageMaker environment\.

1. Creates a feature group\.

1. Adds features to the group\.

1. Adds metadata to the features\.

### Step 1: Setup<a name="feature-store-add-metadata-step-1"></a>

To start using Feature Store, create SageMaker, boto3 and Feature Store sessions\. Then set up the S3 bucket you want to use for your features\. This is your offline store\. The following code uses the SageMaker default bucket and adds a custom prefix to it\. 

**Note**  
 The role that you use must have the following managed policies attached to it: `AmazonS3FullAccess` and `AmazonSageMakerFeatureStoreAccess`\. 

```
# SageMaker Python SDK version 2.x is required
%pip install 'sagemaker>=2.0.0'
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
from botocore.exceptions import ClientError


prefix = 'sagemaker-featurestore-introduction'
role = get_execution_role()

sagemaker_session = sagemaker.Session()
region = sagemaker_session.boto_region_name
s3_bucket_name = sagemaker_session.default_bucket()
```

### Step 2: Create a Feature Group and Add Features<a name="feature-store-add-metadata-step-2"></a>

```
feature_group_name = "test-for-feature-metadata"
feature_definitions = [
    {"FeatureName": "feature-1", "FeatureType": "String"},
    {"FeatureName": "feature-2", "FeatureType": "String"},
    {"FeatureName": "feature-3", "FeatureType": "String"},
    {"FeatureName": "feature-4", "FeatureType": "String"},
    {"FeatureName": "feature-5", "FeatureType": "String"}
]
try:
    sagemaker_client.create_feature_group(
        FeatureGroupName=feature_group_name,
        RecordIdentifierFeatureName="feature-1",
        EventTimeFeatureName="feature-2",
        FeatureDefinitions=feature_definitions,
        OnlineStoreConfig={"EnableOnlineStore": True}
    )
except ClientError as e:
    if e.response["Error"]["Code"] == "ResourceInUse":
        pass
    else:
        raise e
```

### Step 3: Add Metadata<a name="feature-store-add-metadata-step-3"></a>

Before you add metadata, use the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeFeatureGroup.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeFeatureGroup.html) operation to make sure that the status of the feature group is `Created`\.

```
sagemaker_client.describe_feature_group(
        FeatureGroupName=feature_group_name
    )
```

Add a description to the feature\.

```
sagemaker_client.update_feature_metadata(
    FeatureGroupName=feature_group_name,
    FeatureName="feature-1",
    Description="new description"
)
```

You can use the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeFeatureMetadata.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeFeatureMetadata.html) operation to see if you' have successfully updated the description for the feature group\.

```
    sagemaker_client.describe_feature_metadata(
    FeatureGroupName=feature_group_name,
    FeatureName="feature-1"
)
```

You can also use it to add parameters to the feature group\.

```
sagemaker_client.update_feature_metadata(
    FeatureGroupName=feature_group_name,
    FeatureName="feature-1",
    ParameterAdditions=[
        {"Key": "team", "Value": "featurestore"},
        {"Key": "org", "Value": "sagemaker"},
    ]
)
```

You can use the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeFeatureMetadata.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeFeatureMetadata.html) operation again to see if you have successfully added the parameters\.

```
    sagemaker_client.describe_feature_metadata(
    FeatureGroupName=feature_group_name,
    FeatureName="feature-1"
)
```