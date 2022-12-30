# Manage Your Amazon EFS Storage Volume in SageMaker Studio<a name="studio-tasks-manage-storage"></a>

The first time a user on your team onboards to Amazon SageMaker Studio, Amazon SageMaker creates an Amazon Elastic File System \(Amazon EFS\) volume for the team\. A home directory is created in the volume for each user who onboards to Studio as part of your team\. Notebook files and data files are stored in these directories\. Users don't have access to other team member's home directories\. Amazon SageMaker Domain does not support mounting custom or additional Amazon EFS volumes\.

**Important**  
Don't delete the Amazon EFS volume\. If you delete it, the domain will no longer function and all of your users will lose their work\.

**To find your Amazon EFS volume**

1. Open the [SageMaker console](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Control Panel** at the top left of the page\.

1. From the **Control Panel**, under **Domain**, find the **Domain ID**\. The ID will be in the following format: `d-xxxxxxxxxxxx`\.

1. Pass the `Domain ID`, as `DomainId`, to the [describe\_domain](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.describe_domain) method\.

1. In the response from `describe_domain`, note the value for the `HomeEfsFileSystemId` key\. This is the Amazon EFS file system ID\.

1. Open the [Amazon EFS console](https://console.aws.amazon.com/efs#/file-systems/)\. Make sure the AWS Region is the same Region that's used by Studio\.

1. Under **File systems**, choose the file system ID from the previous step\.

1. To verify that you've chosen the correct file system, select the **Tags** heading\. The value corresponding to the `ManagedByAmazonSageMakerResource` key should match the `Studio ID`\.

For information on how to access the Amazon EFS volume, see [Using file systems in Amazon EFS](https://docs.aws.amazon.com/efs/latest/ug/using-fs.html)\.

To delete the Amazon EFS volume, see [Deleting an Amazon EFS file system](https://docs.aws.amazon.com/efs/latest/ug/delete-efs-fs.html)\.