# Shut down and Update SageMaker Studio<a name="studio-tasks-update-studio"></a>

To update Amazon SageMaker Studio to the latest release, you must shut down the JupyterServer app\. Any unsaved notebook information is lost in the process\. The user data in the Amazon EFS volume isn't impacted\.

After the JupyterServer app is shut down, you must reopen Studio through the SageMaker console which creates a new version of the JupyterServer app\.

You can shut down the JupyterServer app from the SageMaker console or from within Studio\.

Some of the services within Studio, like Data Wrangler, run on their own app\. To update these services you must delete the app for that service\. To learn more, see [Shut down and Update Studio Apps](studio-tasks-update-apps.md)\.

**Note**  
A JupyterServer app is associated with a single Studio user\. When you update the app for one user it doesn't affect other users\.

**To shut down the JupyterServer app from the SageMaker console**

1. Navigate to [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. Choose your user name\.

1. Under **Apps**, in the row displaying **JupyterServer**, choose **Delete app**\.

1. Choose **Yes, delete app**\.

1. Type **delete** in the confirmation box\.

1. Choose **Delete**\.

**To shut down the JupyterServer app from inside Studio**

1. \(Optional\) View the current Studio version number\.

   1. Open the Studio Launcher\. Choose **Amazon SageMaker Studio** in the top\-left of Studio\.

   1. Open **Utilities and files**\.

   1. Choose **System terminal**\.

   1. Run the following command: `jupyter labextension list`

      The version is specified similar to `@amzn/sagemaker-ui v2.13.1`\.

1. On the top menu, choose **File** then **Shut Down**\.

1. Choose one of the following options:
   + **Shutdown Server** – Shuts down the JupyterServer app\. Terminal sessions, kernel sessions, SageMaker images, and instances aren't shut down\. These resources continue to accrue charges\.
   + **Shutdown All** – Shuts down all apps, terminal sessions, kernel sessions, SageMaker images, and instances\. These resources no longer accrue charges\.

1. Close the window\.