# Create a Notebook Instance<a name="howitworks-create-ws"></a>

An *Amazon SageMaker notebook instance* is a ML compute instance running the Jupyter Notebook App\. SageMaker manages creating the instance and related resources\. Use Jupyter notebooks in your notebook instance to prepare and process data, write code to train models, deploy models to SageMaker hosting, and test or validate your models\.

To create a notebook instance, use either the SageMaker console or the [  `CreateNotebookInstance`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateNotebookInstance.html) API\.

The notebook instance type you choose depends on how you use your notebook instance\. You want to ensure that your notebook instance is not bound by memory, CPU, or IO\. If you plan to load a dataset into memory on the notebook instance for exploration or preprocessing, we recommend that you choose an instance type with enough RAM memory for your dataset\. This would require an instance with at least 16 GB of memory \(\.xlarge or larger\)\. If you plan to use the notebook for compute intensive preprocessing, we recommend you choose a compute\-optimized instance such as a c4 or c5\.

A best practice when using a SageMaker notebook is to use the notebook instance to orchestrate other AWS services\. For example, you can use the notebook instance to manage large dataset processing by making calls to AWS Glue for ETL \(extract, transform, and load\) services or Amazon EMR for mapping and data reduction using Hadoop\. You can use AWS services as temporary forms of computation or storage for your data\.

You can store and retrieve your training and test data using an Amazon S3 bucket\. You can then use SageMaker to train and build your model, so the instance type of your notebook would have no bearing on the speed of your model training and testing\.

After receiving the request, SageMaker does the following:
+ **Creates a network interface**—If you choose the optional VPC configuration, SageMaker creates the network interface in your VPC\. It uses the subnet ID that you provide in the request to determine which Availability Zone to create the subnet in\. SageMaker associates the security group that you provide in the request with the subnet\. For more information, see [Connect a Notebook Instance to Resources in a VPC](appendix-notebook-and-internet-access.md)\. 
+ **Launches an ML compute instance**—SageMaker launches an ML compute instance in a SageMaker VPC\. SageMaker performs the configuration tasks that allow it to manage your notebook instance, and if you specified your VPC, it enables traffic between your VPC and the notebook instance\.
+ **Installs Anaconda packages and libraries for common deep learning platforms**—SageMaker installs all of the Anaconda packages that are included in the installer\. For more information, see [Anaconda package list](https://docs.anaconda.com/anaconda/packages/pkg-docs)\. In addition, SageMaker installs the TensorFlow and Apache MXNet deep learning libraries\. 
+ **Attaches an ML storage volume**—SageMaker attaches an ML storage volume to the ML compute instance\. You can use the volume as a working area to clean up the training dataset or to temporarily store validation, test, or other data\. Choose any size between 5 GB and 16384 GB, in 1 GB increments, for the volume\. The default is 5 GB\. ML storage volumes are encrypted, so SageMaker can't determine the amount of available free space on the volume\. Because of this, you can increase the volume size when you update a notebook instance, but you can't decrease the volume size\. If you want to decrease the size of the ML storage volume in use, create a new notebook instance with the desired size\.

  Only files and data saved within the `/home/ec2-user/SageMaker` folder persist between notebook instance sessions\. Files and data that are saved outside this directory are overwritten when the notebook instance stops and restarts\. Each notebook instance's /tmp directory provides a minimum of 10 GB of storage in an instant store\. An instance store is temporary, block\-level storage that isn't persistent\. When the instance is stopped or restarted, SageMaker deletes the directory's contents\. This temporary storage is part of the root volume of the notebook instance\.
+ **Copies example Jupyter notebooks**— These Python code examples illustrate model training and hosting exercises using various algorithms and training datasets\.

**To create a SageMaker notebook instance:**

1. Open the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\. 

1. Choose **Notebook instances**, then choose **Create notebook instance**\.

1. On the **Create notebook instance** page, provide the following information: 

   1. For **Notebook instance name**, type a name for your notebook instance\.

   1. For **Notebook instance type**, choose an instance size suitable for your use case\. For a list of supported instance types and quotas, see [Amazon SageMaker Service Quotas](https://docs.aws.amazon.com/general/latest/gr/sagemaker.html#limits_sagemaker)\.

   1. For **Elastic Inference**, choose an inference accelerator type to associate with the notebook instance if you plan to conduct inferences from the notebook instance, or choose **none**\. For information about elastic inference, see [Use Amazon SageMaker Elastic Inference \(EI\) ](ei.md)\.

   1. \(Optional\) **Additional configuration** lets advanced users create a shell script that can run when you create or start the instance\. This script, called a lifecycle configuration script, can be used to set the environment for the notebook or to perform other functions\. For information, see [Customize a Notebook Instance Using a Lifecycle Configuration Script](notebook-lifecycle-config.md)\.

   1. \(Optional\) **Additional configuration** also lets you specify the size, in GB, of the ML storage volume that is attached to the notebook instance\. You can choose a size between 5 GB and 16,384 GB, in 1 GB increments\. You can use the volume to clean up the training dataset or to temporarily store validation or other data\.

   1. For **IAM role**, choose either an existing IAM role in your account that has the necessary permissions to access SageMaker resources or choose **Create a new role**\. If you choose **Create a new role**, SageMaker creates an IAM role named `AmazonSageMaker-ExecutionRole-YYYYMMDDTHHmmSS`\. The AWS managed policy `AmazonSageMakerFullAccess` is attached to the role\. The role provides permissions that allow the notebook instance to call SageMaker and Amazon S3\.

   1. For **Root access**, to enable root access for all notebook instance users, choose **Enable**\. To disable root access for users, choose **Disable**\.If you enable root access, all notebook instance users have administrator privileges and can access and edit all files on it\. 

   1. \(Optional\) **Encryption key** lets you encrypt data on the ML storage volume attached to the notebook instance using an AWS Key Management Service \(AWS KMS\) key\. If you plan to store sensitive information on the ML storage volume, consider encrypting the information\. 

   1. \(Optional\) **Network** lets you put your notebook instance inside a Virtual Private Cloud \(VPC\)\. A VPC provides additional security and restricts access to resources in the VPC from sources outside the VPC\. For more information on VPCs, see [Amazon VPC User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/)\.

      **To add your notebook instance to a VPC:**

      1. Choose the **VPC** and a **SubnetId**\.

      1. For **Security Group**, choose your VPC's default security group\. 

      1. If you need your notebook instance to have internet access, enable direct internet access\. For **Direct internet access**, choose **Enable**\. Internet access can make your notebook instance less secure\. For more information, see [Connect a Notebook Instance to Resources in a VPC](appendix-notebook-and-internet-access.md)\. 

   1. \(Optional\) To associate Git repositories with the notebook instance, choose a default repository and up to three additional repositories\. For more information, see [Associate Git Repositories with SageMaker Notebook Instances](nbi-git-repo.md)\.

   1. Choose **Create notebook instance**\. 

      In a few minutes, Amazon SageMaker launches an ML compute instance—in this case, a notebook instance—and attaches an ML storage volume to it\. The notebook instance has a preconfigured Jupyter notebook server and a set of Anaconda libraries\. For more information, see the [  `CreateNotebookInstance`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateNotebookInstance.html) API\. 

1. When the status of the notebook instance is `InService`, in the console, the notebook instance is ready to use\. Choose **Open Jupyter** next to the notebook name to open the classic Jupyter dashboard\.

    You can choose **Open JupyterLab** to open the JupyterLab dashboard\. The dashboard provides access to your notebook instance and sample SageMaker notebooks that contain complete code walkthroughs\. These walkthroughs show how to use SageMaker to perform common machine learning tasks\. For more information, see [Example Notebooks](howitworks-nbexamples.md)\. For more information, see [Access Notebook Instances](howitworks-access-ws.md)\.

   For more information about Jupyter notebooks, see [The Jupyter notebook](https://jupyter-notebook.readthedocs.io/en/stable/)\.