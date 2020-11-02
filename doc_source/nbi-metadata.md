# Notebook Instance Metadata<a name="nbi-metadata"></a>

When you create a notebook instance, Amazon SageMaker creates a JSON file on the instance at the location `/opt/ml/metadata/resource-metadata.json` that contains the `ResourceName` and `ResourceArn` of the notebook instance\. You can access this metadata from anywhere within the notebook instance, including in lifecycle configurations\. For information about notebook instance lifecycle configurations, see [Customize a Notebook Instance Using a Lifecycle Configuration Script](notebook-lifecycle-config.md)\.

The `resource-metadata.json` file has the following structure:

```
{
    "ResourceArn": "NotebookInstanceArn",
    "ResourceName": "NotebookInstanceName"
}
```

You can use this metadata from within the notebook instance to get other information about the notebook instance\. For example, the following commands get the tags associated with the notebook instance:

```
NOTEBOOK_ARN=$(jq '.ResourceArn'
            /opt/ml/metadata/resource-metadata.json --raw-output)
aws sagemaker list-tags --resource-arn $NOTEBOOK_ARN
```

The out put looks like the following:

```
{
    "Tags": [
        {
            "Key": "test",
            "Value": "true"
        }
    ]
}
```