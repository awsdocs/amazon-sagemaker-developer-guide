# Step 1: Create an Amazon SageMaker Notebook Instance<a name="gs-setup-working-env"></a>

An Amazon SageMaker notebook instance is a fully managed machine learning \(ML\) Amazon Elastic Compute Cloud \(Amazon EC2\) compute instance that runs the Jupyter Notebook App\. You use the notebook instance to create and manage Jupyter notebooks for preprocessing data and to train and deploy machine learning models\.

**To create a SageMaker notebook instance**  
![\[Animated screenshot that shows how to create a SageMaker notebook instance.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/get-started-ni/gs-ni-create-instance.gif)

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Notebook instances**, and then choose **Create notebook instance**\.

1. On the **Create notebook instance** page, provide the following information \(if a field is not mentioned, leave the default values\):

   1. For **Notebook instance name**, type a name for your notebook instance\.

   1. For **Notebook Instance type**, choose `ml.t2.medium`\. This is the least expensive instance type that notebook instances support, and it suffices for this exercise\. If a `ml.t2.medium` instance type isn't available in your current AWS Region, choose `ml.t3.medium`\.

   1. For **Platform Identifier**, choose a platform type to create the notebook instance on\. This platform type dictates the Operating System and the JupyterLab version that your notebook instance is created with\. For information about platform identifier type, see [Amazon Linux 2 vs Amazon Linux notebook instances](nbi-al2.md)\. For information about JupyterLab versions, see [JupyterLab versioning](nbi-jl.md)\.

   1. For **IAM role**, choose **Create a new role**, and then choose **Create role**\. This IAM role automatically gets permissions to access any S3 bucket that has `sagemaker` in the name\. It gets these permissions through the `AmazonSageMakerFullAccess` policy, which SageMaker attaches to the role\. 
**Note**  
If you want to grant the IAM role permission to access S3 buckets without `sagemaker` in the name, you need to attach the `S3FullAccess` policy or limit the permissions to specific S3 buckets to the IAM role\. For more information and examples of adding bucket policies to the IAM role, see [Bucket Policy Examples](https://docs.aws.amazon.com/AmazonS3/latest/dev/example-bucket-policies.html)\.

   1. Choose **Create notebook instance**\. 

      In a few minutes, SageMaker launches an ML compute instance—in this case, a notebook instance—and attaches a 5 GB of Amazon EBS storage volume to it\. The notebook instance has a preconfigured Jupyter notebook server, SageMaker and AWS SDK libraries, and a set of Anaconda libraries\.

      For more information about creating a SageMaker notebook instance, see [Create a Notebook Instance](https://docs.aws.amazon.com/sagemaker/latest/dg/howitworks-create-ws.html)\. 

## \(Optional\) Change SageMaker Notebook Instance Settings<a name="gs-change-ni-settings"></a>

If you want to change the ML compute instance type or the size of the Amazon EBS storage of a SageMaker notebook instance that's already created, you can edit the notebook instance settings\.

**To change and update the SageMaker Notebook instance type and the EBS volume**

1. On the **Notebook instances** page in the SageMaker console, choose your notebook instance\.

1. Choose **Actions**, choose **Stop**, and then wait until the notebook instance fully stops\.

1. After the notebook instance status changes to **Stopped**, choose **Actions**, and then choose **Update settings**\.  
![\[Animated screenshot that shows how to update SageMaker notebook instance settings.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/get-started-ni/gs-ni-update-instance.gif)

   1. For **Notebook instance type**, choose a different ML instance type\.

   1. For **Volume size in GB**, type a different integer to specify a new EBS volume size\.
**Note**  
EBS storage volumes are encrypted, so SageMaker can't determine the amount of available free space on the volume\. Because of this, you can increase the volume size when you update a notebook instance, but you can't decrease the volume size\. If you want to decrease the size of the ML storage volume in use, create a new notebook instance with the desired size\. 

1. At the bottom of the page, choose **Update notebook instance**\. 

1. When the update is complete, **Start** the notebook instance with the new settings\.

For more information about updating SageMaker notebook instance settings, see [Update a Notebook Instance](https://docs.aws.amazon.com/sagemaker/latest/dg/nbi-update.html)\. 

## \(Optional\) Advanced Settings for SageMaker Notebook Instances<a name="gs-ni-advanced-settings"></a>

The following tutorial video shows how to set up and use SageMaker notebook instances through the SageMaker console with advanced options, such as SageMaker lifecycle configuration and importing GitHub repositories\. \(Length: 26:04\)

[![AWS Videos](http://img.youtube.com/vi/https://www.youtube.com/embed/X5CLunIzj3U/0.jpg)](http://www.youtube.com/watch?v=https://www.youtube.com/embed/X5CLunIzj3U)

For complete documentation about SageMaker notebook instance, see [Use Amazon SageMaker notebook Instances](https://docs.aws.amazon.com/sagemaker/latest/dg/nbi.html)\.