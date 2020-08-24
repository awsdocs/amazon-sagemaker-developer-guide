# Get Notebook and App Metadata<a name="notebooks-run-and-manage-metadata"></a>

You can access notebook metadata and App metadata using the Amazon SageMaker UI\.

**Topics**
+ [Get Notebook Metadata](#notebooks-run-and-manage-metadata-notebook)
+ [Get App Metadata](#notebooks-run-and-manage-metadata-app)

## Get Notebook Metadata<a name="notebooks-run-and-manage-metadata-notebook"></a>

Jupyter notebooks contain optional metadata that you can access through the Amazon SageMaker UI\.

**To view the notebook metadata**

1. In the left sidebar, choose the **Notebook Tools** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Notebook_tools_squid.png)\)\.

1. Open the **Advanced Tools** section\.

The metadata should look similar to the following\.

```
{
    "instance_type": "ml.t3.medium",
    "kernelspec": {
        "display_name": "Python 3 (Data Science)",
        "language": "python",
        "name": "python3__SAGEMAKER_INTERNAL__arn:aws:sagemaker:us-west-2:<acct-id>:image/datascience-1.0"
    },
    "language_info": {
        "codemirror_mode": {
            "name": "ipython",
            "version": 3
        },
        "file_extension": ".py",
        "mimetype": "text/x-python",
        "name": "python",
        "nbconvert_exporter": "python",
        "pygments_lexer": "ipython3",
        "version": "3.7.6"
    }
}
```

## Get App Metadata<a name="notebooks-run-and-manage-metadata-app"></a>

When you create a notebook in Amazon SageMaker Studio, the App metadata is written to a file named `resource-metadata.json` in the folder `/opt/ml/metadata/`\. You can get the App metadata by opening an Image terminal from within the notebook\. The metadata gives you the following information, which includes the SageMaker image and instance type the notebook runs in:
+ **AppType** – `KernelGateway` 
+ **DomainId** – Same as the StudioID
+ **UserProfileName** – The profile name of the current user
+ **ResourceArn** – The Amazon Resource Name \(ARN\) of the App, which includes the instance type
+ **ResourceName** – The name of the SageMaker image

The following screenshot shows the menu from a Studio notebook\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-notebooks-manage-callouts.png)

**To get the App metadata**

1. In the center of the notebook menu, choose the **Launch Terminal** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Launch_terminal.png)\)\. This opens a terminal in the SageMaker image that the notebook runs in\.

1. Run the following commands to display the contents of the `resource-metadata.json` file\.

   ```
   cd /opt/ml/metadata/
   cat resource-metadata.json
   ```

The file should look similar to the following\.

```
{
    "AppType": "KernelGateway",
    "DomainId": "d-xxxxxxxxxxxx",
    "UserProfileName": "profile-name",
    "ResourceArn": "arn:aws:sagemaker:us-east-2:account-id:app/d-xxxxxxxxxxxx/profile-name/KernelGateway/datascience--1-0-ml-t3-medium",
    "ResourceName": "datascience--1-0-ml"
}
```