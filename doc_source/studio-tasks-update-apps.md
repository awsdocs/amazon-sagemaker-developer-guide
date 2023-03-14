# Shut down and Update Studio Apps<a name="studio-tasks-update-apps"></a>

To update an Amazon SageMaker Studio app to the latest release, you must first shut down the corresponding KernelGateway app from the SageMaker console\. After the KernelGateway app is shut down, you must reopen it through SageMaker Studio by running a new kernel\. The kernel automatically updates\. Any unsaved notebook information is lost in the process\. The user data in the Amazon EFS volume isn't impacted\.

**Note**  
A KernelGateway app is associated with a single Studio user\. When you update the app for one user it doesn't effect other users\.

**To update the KernelGateway app**

1. Navigate to [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Domains**\.

1. Select the Domain that includes the application that you want to update\.

1. Under **User profiles**, select your user name\.

1. Under **Apps**, in the row displaying the **App name**, choose **Action**, then choose **Delete** 

   To update Data Wrangler, delete the app that starts with **sagemaker\-data\-wrang**\.

1. Choose **Yes, delete app**\.

1. Type **delete** in the confirmation box\.

1. Choose **Delete**\.

1. After the app has been deleted, launch a new kernel from within Studio to use the latest version\.