# Create a custom SageMaker image \(Console\)<a name="studio-byoi-create"></a>

This topic describes how you can create a custom SageMaker image using the SageMaker console\. You can also create the image using the SageMaker Studio control panel\. The steps are the same\. For information on using the Studio control panel, see [Attach a custom SageMaker image \(Control Panel\)](studio-byoi-attach.md)\.

When you create an image, SageMaker also creates an initial image version\. The image version represents a container image in [Amazon Elastic Container Registry \(ECR\)](https://console.aws.amazon.com/ecr/)\. The container image must satisfy the requirements to be used in Amazon SageMaker Studio\. For more information, see [Custom SageMaker image specifications](studio-byoi-specs.md)\.

**To create an image**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. In the left navigation pane, choose **Images**\.

1. On the **Custom images** page, choose **Create image**\.

1. For **Image source**, enter the registry path to the container image in Amazon ECR\. The path is in the following format:

   ` acct-id.dkr.ecr.region.amazonaws.com/repo-name[:tag] or [@digest] `

1. Choose **Next**\.

1. Under **Image properties**, enter the following:
   + Image name – The name must be unique to your account in the current AWS Region\.
   + \(Optional\) Display name – The name displayed in the Studio user interface\. When not provided, `Image name` is displayed\.
   + \(Optional\) Description – A description of the image\.
   + IAM role – The role must have the [AmazonSageMakerFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerFullAccess) policy attached\. Use the dropdown menu to choose one of the following options:
     + Create a new role – Specify any additional Amazon Simple Storage Service \(Amazon S3\) buckets that you want users of your notebooks to have access to\. If you don't want to allow access to additional buckets, choose **None**\.

       SageMaker attaches the `AmazonSageMakerFullAccess` policy to the role\. The role allows users of your notebooks access to the S3 buckets listed next to the checkmarks\.
     + Enter a custom IAM role ARN – Enter the Amazon Resource Name \(ARN\) of your IAM role\.
     + Use existing role – Choose one of your existing roles from the list\.
   + \(Optional\) Image tags – Choose **Add new tag**\. You can add up to 50 tags\. Tags are searchable using the Studio user interface, the SageMaker console, or the SageMaker `Search` API\.

1. Choose **Submit**\.

The new image is displayed in the **Custom images** list and briefly highlighted\. After the image has been successfully created, you can choose the image name to view its properties or choose **Create version** to create another version\.

**To create another image version**

1. Choose **Create version** on the same row as the image\.

1. For **Image source**, enter the registry path to the ECR container image\. The container image shouldn't be the same image as used in a previous version of the SageMaker image\.

To use the custom image in Studio, you must attach it to your domain\. For more information, see [Attach a custom SageMaker image \(Control Panel\)](studio-byoi-attach.md)\.