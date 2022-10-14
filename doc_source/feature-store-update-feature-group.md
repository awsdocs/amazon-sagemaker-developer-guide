# Add Features to a Feature Group<a name="feature-store-update-feature-group"></a>

You can use the Amazon SageMaker Feature Store API or Amazon SageMaker Studio to add features to your feature group\. You can think of a feature group as a data table and a feature as a column in the table\. When you add a feature to the feature group, you're effectively adding a column to the table\.

The features that you've added don't have any data\. You can add new records to the feature group or overwrite them\. You can think of a record as a row in the data table\.

The following sections provide an overview of using the API and Studio to add features to a feature group\. With the API, you can also add or overwrite records after you've updated the feature group\.

## Studio<a name="feature-store-update-feature-group-studio"></a>

To search through your features, do the following\.

1. In Amazon SageMaker Studio, navigate to **SageMaker resources**\.

1. On the navigation pane, choose **Feature Store**\.

1. Choose **Feature group catalog**\.

1. Under **Feature group name**, choose a feature group\.

1. From the dropdown list that says **Actions**, choose **Add feature definitions**\.

1. Specify a name for the **Feature name** field\.

1. For **Type**, select the feature's data type\.

1. Choose **Add new feature definition**\.

1. \(Optional\) Choose **Add new feature definition** to add feature definitions\.

1. Specify information for the additional features\.

1. Choose **Save changes**\.

1. Choose **Confirm**\.

## API<a name="feature-store-update-feature-group-api"></a>

Use the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateFeatureGroup.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateFeatureGroup.html) operation to add features to a feature group\.

You can use the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeFeatureGroup.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeFeatureGroup.html) operation to see if you've added the features successfully\.

To add or overwrite records, use the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_feature_store_PutRecord.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_feature_store_PutRecord.html) operation\.

To see the updates that you've made to a record, use the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_feature_store_GetRecord.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_feature_store_GetRecord.html) operation\. To see the updates that you've made to multiple records, use the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_feature_store_BatchGetRecord.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_feature_store_BatchGetRecord.html) operation\. It can take up to five minutes for the updates that you've made to appear\.

You can use the example code in the following section to walk through adding features and records using the AWS SDK for Python \(Boto3\)\.

## Example Code<a name="feature-store-update-feature-group-example"></a>

The example code walks you through the following process: 

1. Adding features to the feature group

1. Verifying that you've added them successfully

1. Adding a record to the feature group

1. Verifying that you've added it successfully

### Step 1: Add Features To a Feature Group<a name="feature-store-update-feature-group-step-1"></a>

The following code uses the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateFeatureGroup.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateFeatureGroup.html) operation to add new features to the feature group\. It assumes that you've set up Feature Store and created a feature group\. For more information about getting started, see [Introduction to Feature Store](feature-store-introduction-notebook.md)\.

```
import boto3

sagemaker_client = boto3.client("sagemaker")

sagemaker_client.update_feature_group(
    FeatureGroupName=feature_group_name,
    FeatureAdditions=[
        {"FeatureName": "new-feature-1", "FeatureType": "Integral"},
        {"FeatureName": "new-feature-2", "FeatureType": "Fractional"},
        {"FeatureName": "new-feature-3", "FeatureType": "String"}
    ]
)
```

The following code uses the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeFeatureGroup.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeFeatureGroup.html) operation to check the status of the update\. If the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeFeatureGroup.html#sagemaker-DescribeFeatureGroup-response-LastUpdateStatus](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeFeatureGroup.html#sagemaker-DescribeFeatureGroup-response-LastUpdateStatus) field is `Successful`, you've added the features successfully\.

```
sagemaker_client.describe_feature_group(
    FeatureGroupName=feature_group_name
)
```

### Step 2: Add a New Record To The Feature Group<a name="feature-store-update-feature-group-step-2"></a>

The following code uses the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_feature_store_PutRecord.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_feature_store_PutRecord.html) operation to add records to the feature group that you've created\.

```
record_identifier_value = 'new_record'

sagemaker_featurestore_runtime_client = boto3.client("sagemaker-featurestore-runtime")

sagemaker_runtime_client.put_record(
    FeatureGroupName=feature_group_name,
    Record=[
        {
            'FeatureName': "record-identifier-feature-name",
            'ValueAsString': record_identifier_value
        },
        {
            'FeatureName': "event-time-feature",
            'ValueAsString': "timestamp-that-feature-store-returns"
        },
        {
            'FeatureName': "new-feature-1", 
            'ValueAsString': "value-as-string"
        },
        {
            'FeatureName': "new-feature-2", 
            'ValueAsString': "value-as-string"
        },
        {
            'FeatureName': "new-feature-3", 
            'ValueAsString': "value-as-string"
        },
    ]
)
```

Use the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_feature_store_GetRecord.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_feature_store_GetRecord.html) operation to see which records in your feature group don't have data for the features that you've added\. You can use the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_feature_store_PutRecord.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_feature_store_PutRecord.html) operation to overwrite the records that don't have data for the features that you've added\.