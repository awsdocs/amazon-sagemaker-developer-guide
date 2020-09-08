# Perform Common Tasks in Amazon SageMaker Studio<a name="studio-tasks"></a>

The following sections describe how to perform common tasks in Amazon SageMaker Studio\. For an overview of the Studio interface, see [Amazon SageMaker Studio UI Overview](studio-ui.md)\.

**Topics**
+ [Upload files to Amazon SageMaker Studio](#studio-tasks-files)
+ [Clone and Connect to a Git Repository](#studio-tasks-git)
+ [Stop a Training Job](#studio-tasks-stop-training-job)
+ [Manage Your EFS Storage Volume](#studio-tasks-manage-storage)
+ [Provide Feedback on Amazon SageMaker Studio](#studio-tasks-provide-feedback)
+ [Update Amazon SageMaker Studio](#studio-tasks-update)

## Upload files to Amazon SageMaker Studio<a name="studio-tasks-files"></a>

When you onboard to Amazon SageMaker Studio, a home directory is created for you in the Amazon Elastic File System \(Amazon EFS\) volume that was created for your team\. Studio can open only files that have been uploaded to your directory\. The Studio file browser maps to your home directory\.

**To upload files to your home directory**

1. In the left sidebar, choose the **File Browser** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/File_browser_squid.png)\)\.

1. In the file browser, choose the **Upload Files** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/File_upload_squid.png)\)\.

1. Select the files you want to upload and then choose **Open**\.

1. Double\-click a file to open the file in a new tab in Studio\.

## Clone and Connect to a Git Repository<a name="studio-tasks-git"></a>

Amazon SageMaker Studio can connect only to a local repository\. For this procedure, we connect to a clone of the [awslabs/amazon\-sagemaker\-examples](https://github.com/awslabs/amazon-sagemaker-examples) repository \(repo\)\. 

**To clone and connect to a repo**

1. On the top menu, choose **File** then **New** then **Terminal**\.

1. At the command prompt, run the following command and wait for the repo to finish cloning\.

   `git clone https://github.com/awslabs/amazon-sagemaker-examples.git`

1. In the left sidebar, choose the **File Browser** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/File_browser_squid.png)\)\.

1. Double click the repo to open it\.

1. In the left sidebar, choose the **Git** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Git_squid.png)\) to view the Git user interface\.

## Stop a Training Job<a name="studio-tasks-stop-training-job"></a>

You can stop a training job with the Amazon SageMaker Studio UI\. When you stop a training job, its status changes to `Stopping` at which time billing ceases\. An algorithm can delay termination in order to save model artifacts after which the job status changes to `Stopped`\. For more information, see the [stop\_training\_job](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.stop_training_job) method in the AWS SDK for Python \(Boto3\)\.

**To stop a training job**

1. Follow the [View and Compare Experiments, Trials, and Trial Components](experiments-view-compare.md) procedure on this page until you open the **Describe Trial Component** tab\.

1. At the upper\-right side of the tab, choose **Stop training job**\. The **Status** at the top left of the tab changes to **Stopped**\.

1. To view the training time and billing time, choose **AWS Settings**\.

## Manage Your EFS Storage Volume<a name="studio-tasks-manage-storage"></a>

The first time a user on your team onboards to Amazon SageMaker Studio, Amazon SageMaker creates an Amazon Elastic File System \(Amazon EFS\) volume for the team\. A home directory is created in the volume for each user who onboards to Studio as part of your team\. Notebook files and data files are stored in these directories\. Users don't have access to other team member's home directories\.

**Important**  
Don't delete the Amazon EFS volume\. If you delete it, the domain will no longer function and all of your users will lose their work\.

**To find your Amazon EFS volume**

1. From the Amazon SageMaker Studio Control Panel, under **Studio Summary**, find the **Studio ID**\. The ID will be in the following format: `d-xxxxxxxx`\.

1. Pass the `Studio ID`, as `DomainId`, to the [describe\_domain](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.describe_domain) method\.

1. In the response from `describe_domain`, note the value for the `HomeEfsFileSystemId` key\. This is the Amazon EFS file system ID\.

1. Open the [Amazon EFS console](https://console.aws.amazon.com/efs#/file-systems/)\. Make sure the AWS Region is the same as the Region used by Studio\.

1. Under **File systems**, choose the file system ID from the previous step\.

1. To verify that you've chosen the correct file system, select the **Tags** heading\. The value corresponding to the `ManagedByAmazonSageMakerResource` key should match the `Studio ID`\.

For information on how to access the Amazon EFS volume, see [Using file systems in Amazon EFS](https://docs.aws.amazon.com/efs/latest/ug/using-fs.html)\.

To delete the Amazon EFS volume, see [Deleting an Amazon EFS file system](https://docs.aws.amazon.com/efs/latest/ug/delete-efs-fs.html)\.

## Provide Feedback on Amazon SageMaker Studio<a name="studio-tasks-provide-feedback"></a>

Amazon SageMaker takes your feedback seriously\. We encourage you to provide feedback\.

**To provide feedback**

1. At the upper\-right of SageMaker Studio, choose **Feedback**\.

1. Choose a smiley emoji to let us know how satisfied you are with SageMaker Studio and add any feedback you'd care to share with us\.

1. Decide whether to share your identity with us, then choose **Submit**\.

## Update Amazon SageMaker Studio<a name="studio-tasks-update"></a>

When you update Amazon SageMaker Studio to the latest release, Amazon SageMaker shuts down and restarts the JupyterServer App\. Any unsaved notebook information is lost in the process\. The user data in the Amazon EFS volume isn't touched\.

After the JupyterServer App is restarted, you must reopen Studio through the SageMaker Studio Control Panel\.

**To update Studio**

1. \(Optional\) View the current version number\. On the top\-left corner of Studio, choose **Amazon SageMaker Studio** to open the Studio landing page\. The version is shown on the bottom\-left of the page\.

1. On the top menu, choose **File** then **Shut Down**\.

1. Choose one of the following options:
   + **Shutdown Server** – Shuts down the JupyterServer App\. Terminal sessions, kernel sessions, SageMaker images, and instances aren't shut down\. These resources continue to accrue charges\.
   + **Shutdown All** – Shuts down all Apps, terminal sessions, kernel sessions, SageMaker images, and instances\. These resources no longer accrue charges\.

1. Close the window and reopen Studio\.