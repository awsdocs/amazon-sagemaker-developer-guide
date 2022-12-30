# Shut down and Update SageMaker Studio<a name="studio-tasks-update-studio"></a>

To update Amazon SageMaker Studio to the latest release, you must shut down the JupyterServer app\. You can shut down the JupyterServer app from the SageMaker console or from within Studio\. After the JupyterServer app is shut down, you must reopen Studio through the SageMaker console which creates a new version of the JupyterServer app\.

Any unsaved notebook information is lost in the process\. The user data in the Amazon EFS volume isn't impacted\.

Some of the services within Studio, like Data Wrangler, run on their own app\. To update these services you must delete the app for that service\. To learn more, see [Shut down and Update Studio Apps](studio-tasks-update-apps.md)\.

**Note**  
A JupyterServer app is associated with a single Studio user\. When you update the app for one user it doesn't affect other users\.

The following topic shows how to update the JupyterServer App from the SageMaker console or from inside Studio\.

**To update the JupyterServer app from the SageMaker console**

1. Navigate to [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Domains**\.

1. Select the Domain that includes the Studio application that you want to update\.

1. Under **User profiles**, select your user name\.

1. Under **Apps**, in the row displaying **JupyterServer**, choose **Action**, then choose **Delete**\.

1. Choose **Yes, delete app**\.

1. Type **delete** in the confirmation box\.

1. Choose **Delete**\.

1. After the app has been deleted, launch a new Studio app to get the latest version\.

**To update the JupyterServer app from inside Studio**

1. Launch Studio\.

1. On the top menu, choose **File** then **Shut Down**\.

1. Choose one of the following options:
   + **Shutdown Server** – Shuts down the JupyterServer app\. Terminal sessions, kernel sessions, SageMaker images, and instances aren't shut down\. These resources continue to accrue charges\.
   + **Shutdown All** – Shuts down all apps, terminal sessions, kernel sessions, SageMaker images, and instances\. These resources no longer accrue charges\.

1. Close the window\.

1. After the app has been deleted, launch a new Studio app to use the latest version\.