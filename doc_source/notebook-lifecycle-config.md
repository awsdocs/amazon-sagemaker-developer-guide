# Step 2\.1: \(Optional\) Customize a Notebook Instance<a name="notebook-lifecycle-config"></a>

To install packages or sample notebooks on your notebook instance, configure networking and security for it, or otherwise use a shell script to customize it, use a lifecycle configuration\. A *lifecycle configuration* provides shell scripts that run only when you create the notebook instance or whenever you start one\. When you create a notebook instance, you can create a new lifecycle configuration and the scripts it uses or apply one that you already have\.

**Note**  
Each script has a limit of 16384 characters\.  
The value of the `$PATH` environment variable that is available to both scripts is `/sbin:bin:/usr/sbin:/usr/bin`\.  
View CloudWatch Logs for notebook instance lifecycle configurations in log group `/aws/sagemaker/NotebookInstances` in log stream `[notebook-instance-name]/[LifecycleConfigHook]`\.  
Scripts cannot run for longer than 5 minutes\. If a script runs for longer than 5 minutes, it fails and the notebook instance is not created or started\.

**To create a lifecycle configuration**

1. For **Lifecycle configuration \- *Optional***, choose **Create a new lifecycle configuration**\.

1. For **Name**, type a name\.

1. \(Optional\) To create a script that runs when you create the notebook and every time you start it, choose **Start notebook**\.

1. In the **Start notebook** editor, type the script\.

1. \(Optional\) To create a script that runs only once, when you create the notebook,, choose **Create notebook**\.

1. In the **Create notebook** editor, type the script configure networking\.

1. Choose **Create configuration**\.

You can see a list of notebook instance lifecycle configurations you previously created by choosing **Lifecycle configuration** in the Amazon SageMaker console\. From there, you can view, edit, delete existing lifecycle configurations\. You can create a new notebook instance lifecycle configuration by choosing **Create configuration**\. These notebook instance lifecycle configurations are available when you create a new notebook instance\.

## Next Step<a name="gs-setup-working-env-nextstep"></a>

You are now ready to train your first model\. For step\-by\-step instructions, see [Step 3: Train a Model with a Built\-in Algorithm and Deploy It](ex1.md)\.