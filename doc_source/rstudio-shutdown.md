# Shut down and restart RStudio<a name="rstudio-shutdown"></a>

To shut down and restart your RStudio Workbench and the associated RStudioServerPro app, you must first shut down all of your existing RSessions\. You can shut down the RSessionGateway apps from within RStudio\. You can then shut down the RStudioServerPro app using the AWS CLI\. After the RStudioServerPro app is shut down, you must reopen RStudio through the SageMaker console\.

Any unsaved notebook information is lost in the process\. The user data in the Amazon EFS volume isn't impacted\.

**Note**  
If you are using a custom image with RStudio, ensure that your docker image is using an RStudio version that is compatible with the version of RStudio Workbench being used by SageMaker after you restart your RStudioServerPro app\.

The following topics show how to shut down the RSessionGateway and RStudioServerPro apps and restart them\.

## Suspend your RSessions<a name="rstudio-suspend"></a>

Complete the following procedure to suspend all of your RSessions\.

1. From the RStudio Launcher, identify the RSession that you want to suspend\. 

1. Select **Suspend** for the session\. 

1. Repeat this for all RSessions\.

## Delete your RSessions<a name="rstudio-delete"></a>

Complete the following procedure to shut down all of your RSessions\.

1. From the RStudio Launcher, identify the RSession that you want to delete\. 

1. Select **Quit** for the session\. This opens a new **Quit Session** window\. 

1. From the **Quit Session** window, select **Force Quit**, to end all child processes in the session\.

1. Select **Quit Session** to confirm deletion of the session\.

1. Repeat this for all RSessions\.

## Delete your RStudioServerPro app<a name="rstudio-delete-restart"></a>

Run the following commands from the AWS CLI to delete and restart your RStudioServerPro app\.

1. Delete the RStudioServerPro application by using your current domain id\. 

   ```
   aws sagemaker delete-app \
       --domain-id <domainId> \
       --user-profile-name domain-shared \
       --app-type RStudioServerPro \
       --app-name default
   ```

1. Re\-create the RStudioServerPro application\. 

   ```
   aws sagemaker create-app \
       --domain-id <domainId> \
       --user-profile-name domain-shared \
       --app-type RStudioServerPro \
       --app-name default
   ```