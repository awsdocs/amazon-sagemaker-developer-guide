# Update Amazon SageMaker Studio<a name="studio-tasks-update"></a>

When you update Amazon SageMaker Studio to the latest release, Amazon SageMaker shuts down and restarts the JupyterServer App\. Any unsaved notebook information is lost in the process\. The user data in the Amazon EFS volume isn't touched\.

After the JupyterServer App is restarted, you must reopen Studio through the SageMaker Studio Control Panel\.

**To update Studio**

1. \(Optional\) View the current version number\. On the top\-left corner of Studio, choose **Amazon SageMaker Studio** to open the Studio landing page\. The version is shown on the bottom\-left of the page\.

1. On the top menu, choose **File** then **Shut Down**\.

1. Choose one of the following options:
   + **Shutdown Server** – Shuts down the JupyterServer App\. Terminal sessions, kernel sessions, SageMaker images, and instances aren't shut down\. These resources continue to accrue charges\.
   + **Shutdown All** – Shuts down all Apps, terminal sessions, kernel sessions, SageMaker images, and instances\. These resources no longer accrue charges\.

1. Close the window and reopen Studio\.