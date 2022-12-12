# Find Features in Your Feature Groups<a name="feature-store-search-metadata"></a>

With Amazon SageMaker Feature Store, you can search for the features that you've created in your feature groups\. You can search through all of your features without needing to first select a feature group\. You can use the search functionality to quickly find the features that are relevant to your use case\.

To search for features in your feature groups, the feature groups must be within the same AWS account and Region\.

**Important**  
Use the latest version of Amazon SageMaker Studio to make sure that you're using the most recent version of the search functionality\. For information on updating Studio, see [Shut down and Update SageMaker Studio](studio-tasks-update-studio.md)\.

If you're on a team, you might have teammates that are looking for features to use in their models, they can search through all the features in all of the feature groups\.

You can add searchable parameters and descriptions to make your features more discoverable\. For more information, see [Adding Searchable Metadata to Your Features](feature-store-add-metadata.md)\.

The following are the types of metadata that you can use in your search\.

You can search for features using either Amazon SageMaker Studio or the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_Search.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_Search.html) operation in the SageMaker API\. The following table lists all of the searchable metadata and whether you can search for it in Studio\.


****  

| Searchable metadata | API field name | Searchable in Studio? | 
| --- | --- | --- | 
| Feature name | FeatureName | Yes | 
| Feature group name | FeatureGroupName | No | 
| Description | Description | Yes | 
| Parameters | Parameters\.key | Yes | 
| All Parameters | AllParameters | Yes | 
| Feature type | FeatureType | No | 
| Creation time | CreationTime | Yes | 
| Last modified time | LastModifiedTime | No | 

The following sections show you how to search for your features\. 

------
#### [ Studio ]

Use the following procedure to search through all the features that you've created\.

To search through your features, do the following\.

1. Sign in to Studio\. For more information, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.

1. Choose **Studio**\.

1. Choose **Launch app**\.

1. From the dropdown list, select **Studio**\.

1. Choose the Home icon\.

1. Choose **Data**\.

1. Choose **Feature Store**\.

1. Choose **Feature Catalog**\.

1. Specify a text query with at least three characters to search for your features\.

1. \(Optional\) Use advanced filters after you've specified a query\. You can use filters to specify parameters or date ranges in your search results\. If you're searching for a parameter, specify both its key and value\. To find your features more easily, you can do the following:
   + Specify time ranges\.
   + Deselect columns that you don't want to query\.

------
#### [ SDK for Python \(Boto3\) ]

The example uses the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_Search.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_Search.html) operation in the AWS SDK for Python \(Boto3\) to run the search query\. For information about the other languages to submit a query, see [See Also](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_Search.html#API_Search_SeeAlso) in the *Amazon SageMaker API Reference*\.

The following code shows different example search queries using the API\.

```
# Return all features in your feature groups
sagemaker_client.search(
    Resource="FeatureMetadata",
)  

# Search for all features that belong to a feature group that contain the "ver" substring
sagemaker_client.search(
    Resource="FeatureMetadata",
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

# Search for all features that belong to a feature group that have the EXACT name "airport"
sagemaker_client.search(
    Resource="FeatureMetadata",
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

# Search for all features that belong to a feature group that contains the name "ver"
AND have a name that contains "wha"
AND have a parameter (key or value) that contains "hea"

sagemaker_client.search(
    Resource="FeatureMetadata",
    SearchExpression={
        'Filters': [
            {
                'Name': 'FeatureGroupName',
                'Operator': 'Contains',
                'Value': 'ver'
            },
            {
                'Name': 'FeatureName',
                'Operator': 'Contains',
                'Value': 'wha'
            },
            {
                'Name': 'AllParameters', 
                'Operator': 'Contains',
                'Value': 'hea'
            },
        ]
    }
)  

# Search for all features that belong to a feature group with substring "ver" in its name
OR features that have a name that contain "wha"
OR features that have a parameter (key or value) that contains "hea"

sagemaker_client.search(
    Resource="FeatureMetadata",
    SearchExpression={
        'Filters': [
            {
                'Name': 'FeatureGroupName',
                'Operator': 'Contains',
                'Value': 'ver'
            },
            {
                'Name': 'FeatureName',
                'Operator': 'Contains',
                'Value': 'wha'
            },
            {
                'Name': 'AllParameters', 
                'Operator': 'Contains',
                'Value': 'hea'
            },
        ],
        'Operator': 'Or' # note that this is explicitly set to "Or"- the default is "And"
    }
)              


# Search for all features that belong to a feature group with substring "ver" in its name
OR features that have a name that contain "wha"
OR parameters with the value 'Sage' for the 'org' key

sagemaker_client.search(
    Resource="FeatureMetadata",
    SearchExpression={
        'Filters': [
            {
                'Name': 'FeatureGroupName',
                'Operator': 'Contains',
                'Value': 'ver'
            },
            {
                'Name': 'FeatureName',
                'Operator': 'Contains',
                'Value': 'wha'
            },
            {
                'Name': 'Parameters.org', 
                'Operator': 'Contains',
                'Value': 'Sage'
            },
        ],
        'Operator': 'Or' # note that this is explicitly set to "Or"- the default is "And"
    }
)
```

------