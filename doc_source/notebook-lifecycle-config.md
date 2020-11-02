# Customize a Notebook Instance Using a Lifecycle Configuration Script<a name="notebook-lifecycle-config"></a>

To install packages or sample notebooks on your notebook instance, configure networking and security for it, or otherwise use a shell script to customize it, use a lifecycle configuration\. A *lifecycle configuration* provides shell scripts that run only when you create the notebook instance or whenever you start one\. When you create a notebook instance, you can create a new lifecycle configuration and the scripts it uses or apply one that you already have\.

You can also use a lifecycle configuration script to access AWS services from your notebook\. For example, you can create a script that lets you use your notebook to control other AWS resources, such as an Amazon EMR instance\.

We maintain a public repository of notebook lifecycle configuration scripts that address common use cases for customizing notebook instances at [https://github\.com/aws\-samples/amazon\-sagemaker\-notebook\-instance\-lifecycle\-configuration\-samples](https://github.com/aws-samples/amazon-sagemaker-notebook-instance-lifecycle-configuration-samples)\.

**Note**  
Each script has a limit of 16384 characters\.  
The value of the `$PATH` environment variable that is available to both scripts is `/usr/local/sbin:/usr/local/bin:/usr/bin:/usr/sbin:/sbin:/bin`\. The working directory, which is the value of the `$PWD` environment variable, is `/`\.  
View CloudWatch Logs for notebook instance lifecycle configurations in log group `/aws/sagemaker/NotebookInstances` in log stream `[notebook-instance-name]/[LifecycleConfigHook]`\.  
Scripts cannot run for longer than 5 minutes\. If a script runs for longer than 5 minutes, it fails and the notebook instance is not created or started\. To help decrease the run time of scripts, try the following:  
Cut down on necessary steps\. For example, limit which conda environments in which to install large packages\.
Run tasks in parallel processes\.
Use the `nohup` command in your script\.

**To create a lifecycle configuration**

1. For **Lifecycle configuration \- *Optional***, choose **Create a new lifecycle configuration**\.

1. For **Name**, type a name using alphanumeric characters and "\-", but no spaces\. The name can have a maximum of 63 characters\.

1. \(Optional\) To create a script that runs when you create the notebook and every time you start it, choose **Start notebook**\.

1. In the **Start notebook** editor, type the script\.

1. \(Optional\) To create a script that runs only once, when you create the notebook, choose **Create notebook**\.

1. In the **Create notebook** editor, type the script configure networking\.

1. Choose **Create configuration**\.

You can see a list of notebook instance lifecycle configurations you previously created by choosing **Lifecycle configuration** in the SageMaker console\. From there, you can view, edit, delete existing lifecycle configurations\. You can create a new notebook instance lifecycle configuration by choosing **Create configuration**\. These notebook instance lifecycle configurations are available when you create a new notebook instance\.