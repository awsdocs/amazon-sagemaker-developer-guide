# Create a Notebook Instance<a name="howitworks-create-ws"></a>

To create a notebook instance, use either the Amazon SageMaker console or the [CreateNotebookInstance](API_CreateNotebookInstance.md) API\.

After receiving the request, Amazon SageMaker does the following:
+ **Creates a network interface**—If you choose the optional VPC configuration, it creates the network interface in your VPC\. It uses the subnet ID that you provide in the request to determine which Availability Zone to create the subnet in\. Amazon SageMaker associates the security group that you provide in the request with the subnet\. For more information, see [Notebook Instance Security](appendix-additional-considerations.md)\. 
+ **Launches an ML compute instance**—Amazon SageMaker launches an ML compute instance in an Amazon SageMaker VPC\. Amazon SageMaker performs the configuration tasks that allow it to manage your notebook instance, and if you specified your VPC, it enables traffic between your VPC and the notebook instance\.
+ **Installs Anaconda packages and libraries for common deep learning platforms**—Amazon SageMaker installs all of the Anaconda packages that are included in the installer\. For more information, see [Anaconda package list](https://docs.anaconda.com/anaconda/packages/pkg-docs)\. In addition, Amazon SageMaker installs the TensorFlow and Apache MXNet deep learning libraries\. 
+ **Attaches an ML storage volume**—Amazon SageMaker attaches an ML storage volume to the ML compute instance\. You can use the volume to clean up the training dataset or to temporarily store other data to work with\. Choose any size between 5 GB and 16384 GB, in 1 GB increments, for the volume\. The default is 5 GB\. ML storage volumes are encrypted, so Amazon SageMaker can't determine the amount of available free space on the volume\. Because of this, you can increase the volume size when you update a notebook instance, but you can't decrease the volume size\. If you want to decrease the size of the ML storage volume in use, create a new notebook instance with the desired size\.
**Important**  
Only files and data saved within the `/home/ec2-user/SageMaker` folder persist between notebook instance sessions\. Files and data that are saved outside this directory are overwritten when the notebook instance stops and restarts\.
**Note**  
Each notebook instance's /tmp directory provides a minimum of 10 GB of storage in an instant store\. An instance store is temporary, block\-level storage that isn't persistent\. When the instance is stopped or restarted, Amazon SageMaker deletes the directory's contents\. This temporary storage is part of the root volume of the notebook instance\.
+ **Copies example Jupyter notebooks**— These Python code examples illustrate model training and hosting exercises using various algorithms and training datasets\.

**To create a notebook instance:**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\. 

1. Choose **Notebook instances**, then choose **Create notebook instance**\.

1. On the **Create notebook instance** page, provide the following information: 

   1. For **Notebook instance name**, type a name for your notebook instance\.

   1. For **Instance type**, choose an instance type for your notebook instance\. For a list of supported instance types, see [Amazon SageMaker Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_sagemaker)\.

   1. For **Elastic Inference**, choose an inference accelerator type to associate with the notebook instance, or choose **none**\. For information about elastic inference, see [Amazon SageMaker Elastic Inference \(EI\) ](ei.md)\.

   1. For **IAM role**, choose either an existing IAM role in your account that has the necessary permissions to access Amazon SageMaker resources or **Create a new role**\. If you choose **Create a new role**, for **Create an IAM role**:

      1. If you want to use S3 buckets other than the one you created in [Step 1: Create an Amazon S3 Bucket](gs-config-permissions.md) to store your input data and output, choose them\. 

         The IAM role automatically has permissions to use any bucket that has `sagemaker` as part of its name\. The `AmazonSageMakerFullAccess` policy, which Amazon SageMaker attaches to the role, gives the role those permissions\. 

         To give access to other S3 buckets from your notebook instance
         + If you're not concerned about users in your AWS account accessing your data, choose **Any S3 bucket**\.
         + If your account has sensitive data \(such as Human Resources information\), restrict access to certain buckets by choosing **Specific S3 buckets**\. You can update the permissions policy attached to the role you are creating later\.
         + To explicitly control access, restrict access by choosing **None**\. Use bucket and object names and tags as supported by the `AmazonSageMakerFullAccess` policy\. For more information, see [Using the AWS Managed Permission Policy \(AmazonSageMakerFullAccess\) for an Execution Role](sagemaker-roles.md#sagemaker-roles-amazonsagemakerfullaccess-policy)\.

      1. Choose **Create role**\.

         Amazon SageMaker creates an IAM role named `AmazonSageMaker-ExecutionRole-YYYYMMDDTHHmmSS`\. For example, `AmazonSageMaker-ExecutionRole-20171125T090800`\.

         To see the policies that are attached to the role, use the IAM console\. 

         Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

          You can see that the following policies are attached to the role:
         + A trust policy that allows Amazon SageMaker to assume the role\. 
         + The `AmazonSageMakerFullAccess` AWS managed policy\. 
         + If you gave access to additional S3 bucket\(s\) when creating this role, the customer managed policy attached to the role\. The name of the customer managed policy is `AmazonSageMaker-ExecutionPolicy-YYYYMMDDTHHmmSS`\. 

         For more information about creating your own IAM role, see [Amazon SageMaker Roles ](sagemaker-roles.md)\. 

   1. For **Root access**, to enable root access for all notebook instance users,choose **Enabled**\. To disable root access for users, choose **Disabled**\.If you enable root access, all notebook instance users have administrator privileges and can access and edit all files on it\. 
**Note**  
If you disable root access, you will still be able to set up lifecycle configurations, as described later in this procedure\. 

   1. \(Optional\) Allow access to resources in your Virtual Private Cloud \(VPC\)\. 

      **To access resources in your VPC from the notebook instance**

      1. Choose the **VPC** and a **SubnetId**\.

      1. For **Security Group**, choose your VPC's default security group\. For this exercise and others in this guide\) the inbound and outbound rules of the default security group are sufficient\. 

      1. To allow connecting to a resource in your VPC, ensure that the resource resolves to a private IP address in your VPC\. For example, to ensure that an Amazon Redshift DNS name resolves to a private IP address, do one of the following: 
         + Ensure that the Amazon Redshift cluster is not publicly accessible\. 
         + If the Amazon Redshift cluster is publicly accessible, set the `DNS resolution` and `DNS hostnames` VPC parameters to `true`\. For more information, see [Managing Clusters in an Amazon Virtual Private Cloud \(VPC\)](https://docs.aws.amazon.com//redshift/latest/mgmt/managing-clusters-vpc.html)

      1. By default, a notebook instance can't connect to on\-premises resources or to a peer VPC\. You can create a lifecycle configuration that creates an entry in your route table that enables connection to on\-premises resources or to a peer VPC\. For information, see [Understanding Amazon SageMaker notebook instance networking configurations and advanced routing options](https://aws.amazon.com/blogs/machine-learning/understanding-amazon-sagemaker-notebook-instance-networking-configurations-and-advanced-routing-options/)\.

   1. If you allowed access to resources from your VPC, enable direct internet access\. For **Direct internet access**, choose **Enable**\. Without internet access, you can't train or host models from notebooks on this notebook instance unless your VPC has a NAT gateway and your security group allows outbound connections\. For more information, see [Notebook Instances Are Internet\-Enabled by Default](appendix-additional-considerations.md#appendix-notebook-and-internet-access)\. 

   1. \(Optional\) To use shell scripts that run when you create or start the instance, specify a lifecycle configuration\. For information, see [Customize a Notebook Instance ](notebook-lifecycle-config.md)

   1. \(Optional\) If you want Amazon SageMaker to use an AWS Key Management Service \(AWS KMS\) key to encrypt data in the ML storage volume attached to the notebook instance, specify the key\. 

   1. Specify the size, in GB, of the ML storage volume that is attached to the notebook instance\. You can choose a size between 5 GB and 16,384 GB, in 1 GB increments\. You can use the volume to clean up the training dataset when you no longer need it or to temporarily store other data to work with\.

   1. \(Optional\) To associate Git repositories with the notebook instance, choose a default repository and up to three additional repositories\. For more information, see [Associate Git Repositories with Amazon SageMaker Notebook Instances](nbi-git-repo.md)\.

   1. Choose **Create notebook instance**\. 

      In a few minutes, Amazon SageMaker launches an ML compute instance—in this case, a notebook instance—and attaches an ML storage volume to it\. The notebook instance has a preconfigured Jupyter notebook server and a set of Anaconda libraries\. For more information, see the [CreateNotebookInstance](API_CreateNotebookInstance.md) API\. 

1. When the status of the notebook instance is `InService`, choose **Open Jupyter** next to its name to open the classic Jupyter dashboard, or choose **Open JupyterLab** to open the JupyterLab dashboard\. For more information, see [Access Notebook Instances ](howitworks-access-ws.md)\.

   The dashboard provides access to:
   + Sample notebooks\. Amazon SageMaker provides sample notebooks that contain complete code walkthroughs\. These walkthroughs show how to use Amazon SageMaker to perform common machine learning tasks\. For more information, see [Use Example Notebooks](howitworks-nbexamples.md)\.
   + The kernels for Jupyter, including those that provide support for Python 2 and 3, Apache MXNet, TensorFlow, and PySpark\. To create a new notebook and choose a kernel for that notebook, use the **New** menu\.

   For more information about Jupyter notebooks, see [The Jupyter notebook](https://jupyter-notebook.readthedocs.io/en/stable/)\.