# Manage Your Storage Volume<a name="notebooks-personal-storage-manage"></a>

The first time a user on your team opens Amazon SageMaker Studio, Amazon SageMaker creates a domain for the team\. The domain includes an Amazon Elastic File System \(Amazon EFS\) volume with home directories for each of your users\. Notebook files and data files are stored in these directories\. Users can't access each other's home directories\.

**Important**  
Don't delete the Amazon EFS volume\. If you delete it, the domain will no longer function and all of your users will lose their work\.

## Find Your Amazon EFS Storage Volume<a name="notebooks-personal-storage-manage-find"></a>

**To find your Amazon EFS volume**

1. From the Amazon SageMaker Studio Control Panel, under **Studio Summary**, find the **Studio ID**\. The ID will be in the following format: `d-xxxxxxxx`\.

1. Pass the `Studio ID`, which is the same as `DomainId`, to the [DescribeDomain](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeDomain.html) API\.

   In the response from `DescribeDomain`, note the value in the `HomeEfsFileSystemId` field\.

1. Open the [Amazon EFS console](https://console.aws.amazon.com/efs/)\.

1. Under **File systems**, scroll through the list to find the `HomeEfsFileSystemId` value, which is the same as the **File system ID**\.