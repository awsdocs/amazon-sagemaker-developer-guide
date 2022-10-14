# Open RStudio Launcher and launch RSessions<a name="rstudio-launcher"></a>

 The following topics show how to use the RStudio Launcher to launch RSessions\. 

## Open RStudio Launcher<a name="rstudio-launcher-open"></a>

### Open RStudio Launcher from the Amazon SageMaker Console<a name="rstudio-launcher-console"></a>

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1.  Navigate to the **Control Panel**\.

1.  After your user has been created, select **Launch app**\. 

1.  Select either **Studio** or **RStudio** to launch a new app of that type\. 

### Open RStudio Launcher from the AWS CLI<a name="rstudio-launcher-cli"></a>

The procedure to open the RStudio Launcher using the AWS CLI differs depending on the method used to manage your users\. 

 **IAM Identity Center** 

1.  Use the AWS access portal to open your Amazon SageMaker Domain\. 

1.  Modify the URL path to “/rstudio/default” as follows\. 

   ```
   #Studio URL
   https://<domain-id>.studio.<region>.sagemaker.aws/jupyter/default/lab
   
   #modified URL
   https://<domain-id>.studio.<region>.sagemaker.aws/rstudio/default
   ```

 **IAM** 

 To open the RStudio Launcher from the AWS CLI in IAM mode, complete the following procedure\. 

1.  Create a presigned URL using the following command\. 

   ```
   aws sagemaker create-presigned-domain-url --region <REGION> \
       --domain-id <DOMAIN-ID> \
       --user-profile-name <USER-PROFILE-NAME>
   ```

1.  Append *&redirect=RStudioServerPro* to the generated URL\. 

1.  Navigate to the updated URL\. 

## Launch RSessions<a name="rstudio-launcher-launch"></a>

 After you’ve launched the RStudio Launcher, you can create a new RSession\. 

1.  Select **New Session**\. 

1.  Enter a **Session Name**\. 

1.  Select an instance type that your RSession runs on\. This defaults to `ml.t3.medium`\.

1.  Select an Image that your RSession uses as the kernel\. 

1.  Select Start Session\. 

1.  After your session has been created, you can start it by selecting the name\.  

## Suspend your RSessions<a name="rstudio-launcher-suspend"></a>

1. From the RStudio Launcher, identify the RSession that you want to suspend\. 

1. Select **Suspend** for the session\. 

## Delete your RSessions<a name="rstudio-launcher-delete"></a>

1. From the RStudio Launcher, identify the RSession that you want to delete\. 

1. Select **Quit** for the session\. This opens a new **Quit Session** window\. 

1. From the **Quit Session** window, select **Force Quit**, to end all child processes in the session\.

1. Select **Quit Session** to confirm deletion of the session\.