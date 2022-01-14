# Control root access to a SageMaker notebook instance<a name="nbi-root-access"></a>

By default, when you create a notebook instance, users that log into that notebook instance have root access\. Data science is an iterative process that might require the data scientist to test and use different software tools and packages, so many notebook instance users need to have root access to be able to install these tools and packages\. Because users with root access have administrator privileges, users can access and edit all files on a notebook instance with root access enabled\.

If you don't want users to have root access to a notebook instance, when you call [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateNotebookInstance.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateNotebookInstance.html) or [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateNotebookInstance.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateNotebookInstance.html) operations, set the `RootAccess` field to `Disabled`\. You can also disable root access for users when you create or update a notebook instance in the Amazon SageMaker console\. For information, see [Step 1: Create an Amazon SageMaker Notebook Instance](gs-setup-working-env.md)\.

**Note**  
Lifecycle configurations need root access to be able to set up a notebook instance\. Because of this, lifecycle configurations associated with a notebook instance always run with root access even if you disable root access for users\.

**Note**  
For security reasons, Rootless Docker is installed on root\-disabled notebook instances instead of regular Docker\. For more information, see [Run the Docker daemon as a non\-root user \(Rootless mode\)](https://docs.docker.com/engine/security/rootless/)