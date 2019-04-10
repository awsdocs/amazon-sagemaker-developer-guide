# Step 1: Create an Amazon SageMaker Notebook Instance<a name="gs-setup-working-env"></a>

An Amazon SageMaker notebook instance is a fully managed machine learning \(ML\) EC2 compute instance running the Jupyter Notebook App\. For more information, see [Explore and Preprocess Data](how-it-works-notebooks-instances.md)\. 

**Note**  
If necessary, you can change the notebook instance settings, including the ML compute instance type, later\.

**To create an Amazon SageMaker notebook instance**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\. 

1. Choose **Notebook instances**, then choose **Create notebook instance**\.

1. On the **Create notebook instance** page, provide the following information: 

   1. For **Notebook instance name**, type **ExampleNotebookInstance**\.

   1. For **Instance type**, choose **ml\.t2\.medium**\.

   1. For **IAM role**, create an IAM role\.

      1. Choose **Create a new role**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/create-workspace-20.png)

      1. \(Optional\) If you want to use S3 buckets other than the one you created in Step 1 of this tutorial to store your input data and output, choose them\. 

         In Step 1 of this tutorial, you created an S3 bucket with `sagemaker` in its name\. This IAM role automatically has permissions to use that bucket\. The `AmazonSageMakerFullAccess` policy, which Amazon SageMaker attaches to the role, gives the role those permissions\. 

         The bucket that you created in Step 1 is sufficient for the model training exercise in Getting Started\. However, as you explore Amazon SageMaker, you might want to access other S3 buckets from your notebook instance\. Give Amazon SageMaker permissions to access those buckets\.

         **To access more S3 buckets from your Amazon SageMaker notebook instance**

         1. If you're not concerned about users in your AWS account accessing your data, choose **Any S3 bucket**\.

         1. If your account has sensitive data \(such as Human Resources information\), restrict access by choosing **Specific S3 buckets**\. You can update the permissions policy attached to the role you are creating later\.

            To explicitly control access, Restrict access by choosing **None**\. use bucket and object names and tags as supported by the `AmazonSageMakerFullAccess` policy\. For more information, see [Using the AWS Managed Permission Policy \(AmazonSageMakerFullAccess\) for an Execution Role](sagemaker-roles.md#sagemaker-roles-amazonsagemakerfullaccess-policy)\.

      1. Choose **Create role**\.

         Amazon SageMaker creates an IAM role named `AmazonSageMaker-ExecutionRole-YYYYMMDDTHHmmSS`\. For example, `AmazonSageMaker-ExecutionRole-20171125T090800`\.

         To see the policies that are attached to the role, use the IAM console\. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\. The following policies are attached to the role:
         + A trust policy that allows Amazon SageMaker to assume the role\. 
         + The `AmazonSageMakerFullAccess` AWS managed policy\. 
         + If you specified access to additional S3 bucket\(s\) when creating this role, the customer managed policy attached to the role\. The name of the customer managed policy is `AmazonSageMaker-ExecutionPolicy-YYYYMMDDTHHmmSS`\. 

         For more information about creating your own IAM role, see [Amazon SageMaker Roles ](sagemaker-roles.md)\. 

   1. For **Root Access**, choose **Enabled** to enable root access for users, or choose **Disabled** to disable root access for users\. If you disable root access for users, lifecycle configurations associated with the notebook instance still run as the root user\.

   1. \(Optional\) Choose to access resources in a Virtual Private Cloud \(VPC\)\. 

      **To access resources in your VPC from the notebook instance**

      1. Choose the **VPC** and a **SubnetId**\.

      1. For **Security Group**, choose your VPCs default security group\. For the exercises in this guide, the inbound and outbound rules of the default security group are sufficient\. 

      1. To enable connecting to a resource in your VPC, ensure that the resource resolves to a private IP address in your VPC\. For example, to ensure that an Amazon Redshift DNS name resolves to a private IP address, do one of the following: 
         + Ensure that the Amazon Redshift cluster is not publicly accessible\. 
         + If the Amazon Redshift cluster is publicly accessible, set the `DNS resolution` and `DNS hostnames` VPC parameters to `true`\. For more information, see [Managing Clusters in an Amazon Virtual Private Cloud \(VPC\)](https://docs.aws.amazon.com//redshift/latest/mgmt/managing-clusters-vpc.html) 

   1. If you chose to access resources from your VPC, enable direct internet access\. For **Direct internet access**, choose **Enable**\. Otherwise, this notebook instance won't have internet access\. Without internet access, you can't train or host models from notebooks on this notebook instance unless your VPC has a NAT gateway and your security group allows outbound connections\. For more information, see [Notebook Instances Are Internet\-Enabled by Default](appendix-additional-considerations.md#appendix-notebook-and-internet-access)\. 

   1. \(Optional\) To use shell scripts that run when you create or start the instance, specify a lifecycle configuration\. For information, see [Step 1\.1: \(Optional\) Customize a Notebook Instance ](notebook-lifecycle-config.md)

   1. \(Optional\) If you want Amazon SageMaker to use an AWS Key Management Service key to encrypt data in the ML storage volume attached to the notebook instance, specify the key\. 

   1. Specify the size, in GB, of the ML storage volume that is attached to the notebook instance\. You can choose a size between 5 GB and 16384 GB, in 1 GB increments\.

   1. \(Optional\) To associate git repositories with the notebook instance, choose a default repository and up to 3 additional repositories\. For more information, see [Associate Git Repositories with Amazon SageMaker Notebook Instances](nbi-git-repo.md)\.

   1. Choose **Create notebook instance**\. 

      In a few minutes, Amazon SageMaker launches an ML compute instance—in this case, a notebook instance—and attaches an ML storage volume to it\. The notebook instance has a preconfigured Jupyter notebook server and a set of Anaconda libraries\. For more information, see the [CreateNotebookInstance](API_CreateNotebookInstance.md) API\. 

1. When the status of the notebook instance is `InService`, choose **Open** next to its name to open the Jupyter dashboard\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/notebook-dashboard.png)

   The dashboard provides access to:
   + A tab that contains sample notebooks\. To use a sample notebook, on the **SageMaker Examples** tab, choose the sample notebook you want to use, and choose **use**\. For information about the sample notebooks, see the Amazon SageMaker [GitHub repository](https://github.com/awslabs/amazon-sagemaker-examples)\.
   + The kernels for Jupyter, including those that provide support for Python 2 and 3, Apache MXNet, TensorFlow, and PySpark\. To choose a kernel for your notebook instance, use the **New** menu\. 

   For more information, see [The Jupyter notebook](https://jupyter-notebook.readthedocs.io/en/stable/)\.

## Next Step<a name="gs-setup-working-env-nextstep"></a>

You are now ready to train your first model\. For step\-by\-step instructions, see [Step 2: Train a Model with a Built\-in Algorithm and Deploy It](ex1.md)\.