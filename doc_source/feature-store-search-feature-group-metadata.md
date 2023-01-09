# Find Feature Groups in Your Feature Store<a name="feature-store-search-feature-group-metadata"></a>

With Amazon SageMaker Feature Store, you can search for the feature groups using either Amazon SageMaker Studio or the [Search](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_Search.html) operation\. You can use the search functionality to find features and feature groups that are relevant to the models that you're creating\. You can use the search functionality to quickly find the feature groups that are relevant to your use case\.

**Note**  
The feature groups that you're searching for must be within the same AWS account and AWS Region\.

**Important**  
Use the latest version of Amazon SageMaker Studio to make sure that you're using the most recent version of the search functionality\. For information on updating Studio, see [Shut down and Update SageMaker Studio](studio-tasks-update-studio.md)\.

The following table shows the searchable fields and whether you can use Studio to search for a specific field\.

You can search for features using either Amazon SageMaker Studio or the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_Search.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_Search.html) operation in the SageMaker API\. The following table lists all of the searchable metadata and whether you can search for it in Studio\.


****  

| Searchable metadata | API field name | Searchable in Studio? | 
| --- | --- | --- | 
| Creation time | CreationTime | Yes | 
| Description | Description | Yes | 
| Event Time Feature Name | EventTimeFeatureName | No | 
| Creation Failure Reason | FailureReason | No | 
| Feature Definitions | [FeatureDefinitions](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_FeatureDefinition.html) | No | 
| Feature Group ARN | [FeatureGroupARN](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_FeatureGroup.html) | No | 
| Feature Group Name | [FeatureGroupName](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_FeatureGroup.html) | Yes | 
| Creation Status | [FeatureGroupStatus](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_FeatureGroup.html) | Yes | 
| Last Update Status | [LastUpdateStatus](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_LastUpdateStatus.html) | No | 
| Last Update Status | LastUpdateStatus | No | 
| Record Identfier Feature Name | RecordIdentifierFeatureName | Yes | 
| Offline Store Configuration | [OfflineStoreConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_OfflineStoreConfig.html) | No | 
| Offline Store Status | [OfflineStoreStatus](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_OfflineStoreStatus.html) | Yes | 
| Tags | Tags\.key | Yes | 
| All Tags | AllTags | Yes | 

The following sections show you how to search for your features\. 

------
#### [ Studio ]

Use the following procedure to search through all the feature groups that you've created\.

To search through your feature groups, do the following\.

1. Sign in to Studio\. For more information, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.

1. Choose **Studio**\.

1. Choose **Launch app**\.

1. From the dropdown list, select **Studio**\.

1. Choose the Home icon\.

1. Choose **Data**\.

1. Choose **Feature Store**\.

1. Under **Feature Group Catalog**, specify a text query with at least three characters to search for your feature groups\.

1. \(Optional\) Use advanced filters after you've specified a query\. You can use filters to specify parameters or date ranges in your search results\. If you're searching for a parameter, specify both its key and value\. To find your features more easily, you can do the following:
   + Specify time ranges\.
   + Deselect columns that you don't want to query\.
   + Select whether you're searching for feature groups in the:
     + Offline store
     + Online store
     + Offline store and online store
   + Specify feature groups with a specific status, such as whether they've been created\.

------
#### [ SDK for Python \(Boto3\) ]

The example uses the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_Search.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_Search.html) operation in the AWS SDK for Python \(Boto3\) to run the search query\. For information about the other languages to submit a query, see [See Also](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_Search.html#API_Search_SeeAlso) in the *Amazon SageMaker API Reference*\.

The following code shows different example search queries using the API\.

```
# Return all feature groups
sagemaker_client.search(
    Resource="FeatureGroups",
)  

# Search for all feature groups with a name that contains the "ver" substring
sagemaker_client.search(
    Resource="FeatureGroups",
    SearchExpression={
        'Filters': [
            {
                'Name': 'FeatureGroupName',
                'Operator': 'Contains',
                'Value': 'ver'
            },
        ]
    }
)

# Search for all feature groups that have the EXACT name "airport"
sagemaker_client.search(
    Resource="FeatureGroups",
    SearchExpression={
        'Filters': [
            {
                'Name': 'FeatureGroupName',
                'Operator': 'Equals',
                'Value': 'airport'
            },
        ]
    }
)

# Search for all feature groups that contains the name "ver"
# AND have a record identifier feature name that contains "wha"
# AND have a tag (key or value) that contains "hea"
sagemaker_client.search(
    Resource="FeatureGroups",
    SearchExpression={
        'Filters': [
            {
                'Name': 'FeatureGroupName',
                'Operator': 'Contains',
                'Value': 'ver'
            },
            {
                'Name': 'RecordIdentifierFeatureName',
                'Operator': 'Contains',
                'Value': 'wha'
            },
            {
                'Name': 'AllTags', 
                'Operator': 'Contains',
                'Value': 'hea'
            },
        ]
    }
)  

# Search for all feature groups with substring "ver" in its name
# OR feature groups that have a record identifier feature name that contains "wha"
# OR feature groups that have a tag (key or value) that contains "hea"
sagemaker_client.search(
    Resource="FeatureGroups",
    SearchExpression={
        'Filters': [
            {
                'Name': 'FeatureGroupName',
                'Operator': 'Contains',
                'Value': 'ver'
            },
            {
                'Name': 'RecordIdentifierFeatureName',
                'Operator': 'Contains',
                'Value': 'wha'
            },
            {
                'Name': 'AllTags', 
                'Operator': 'Contains',
                'Value': 'hea'
            },
        ],
        'Operator': 'Or' # note that this is explicitly set to "Or"- the default is "And"
    }
)              


# Search for all feature groups with substring "ver" in its name
# OR feature groups that have a record identifier feature name that contains "wha"
# OR tags with the value 'Sage' for the 'org' key
sagemaker_client.search(
    Resource="FeatureGroups",
    SearchExpression={
        'Filters': [
            {
                'Name': 'FeatureGroupName',
                'Operator': 'Contains',
                'Value': 'ver'
            },
            {
                'Name': 'RecordIdentifierFeatureName',
                'Operator': 'Contains',
                'Value': 'wha'
            },
            {
                'Name': 'Tags.org', 
                'Operator': 'Contains',
                'Value': 'Sage'
            },
        ],
        'Operator': 'Or' # note that this is explicitly set to "Or"- the default is "And"
    }
)

# Search for all offline only feature groups
sagemaker_client.search(
    Resource="FeatureGroups",
    SearchExpression={
        'Filters': [
            {
                'Name': 'OnlineStoreConfig.EnableOnlineStore',
                'Operator': 'NotEquals',
                'Value': 'true'
            },
            {
                'Name': 'OfflineStoreConfig.S3StorageConfig.S3Uri',
                'Operator': 'Exists'
            }
        ]
    }
)

# Search for all online only feature groups
sagemaker_client.search(
    Resource="FeatureGroups",
    SearchExpression={
        'Filters': [
            {
                'Name': 'OnlineStoreConfig.EnableOnlineStore',
                'Operator': 'Equals',
                'Value': 'true'
            },
            {
                'Name': 'OfflineStoreConfig.S3StorageConfig.S3Uri',
                'Operator': 'NotExists'
            }
        ]
    }
)

# Search for all feature groups that are BOTH online and offline
sagemaker_client.search(
    Resource="FeatureGroups",
    SearchExpression={
        'Filters': [
            {
                'Name': 'OnlineStoreConfig.EnableOnlineStore',
                'Operator': 'Equals',
                'Value': 'true'
            },
            {
                'Name': 'OfflineStoreConfig.S3StorageConfig.S3Uri',
                'Operator': 'Exists'
            }
        ]
    }
)
```

------