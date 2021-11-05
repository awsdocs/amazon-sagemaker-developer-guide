# Use RStudio on Amazon SageMaker<a name="rstudio-use"></a>

With RStudio support in Amazon SageMaker, you can put your production workflows in place and take advantage of SageMaker features\. The following topics show how to launch an RStudio session and complete key workflows\. For information about managing RStudio on SageMaker, see [Manage RStudio on Amazon SageMaker](rstudio-manage.md)\. 

For information about the onboarding steps to create an Amazon SageMaker Domain with RStudio enabled, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.  

For information about the AWS Regions that RStudio on SageMaker is supported in, see [Supported Regions and Quotas](regions-quotas.md)\.  

**Topics**
+ [Collaborate in RStudio](#rstudio-collaborate)
+ [Base R image](#rstudio-base-image)
+ [Open RStudio Launcher and launch RSessions](rstudio-launcher.md)
+ [Publish to RStudio Connect](rstudio-connect.md)
+ [Access Amazon SageMaker features with RStudio on Amazon SageMaker](rstudio-sm-features.md)

## Collaborate in RStudio<a name="rstudio-collaborate"></a>

 To share your RStudio project, you can connect RStudio to your Git repo\. For information on setting this up, see [ Version Control with Git and SVN](https://support.rstudio.com/hc/en-us/articles/200532077-Version-Control-with-Git-and-SVN)\. 

 Note: Project sharing and realtime collaboration are not currently supported when using RStudio on Amazon SageMaker\.  

## Base R image<a name="rstudio-base-image"></a>

 When launching your RStudio instance, the Base R image serves as the basis of your instance\. This image extends the [r\-session\-complete](https://hub.docker.com/r/rstudio/r-session-complete) Docker image\.  

 This Base R image includes the following: 
+  R v4\.0 or higher
+  `awscli`, `sagemaker`, and `boto3` Python packages 
+  [Reticulate](https://rstudio.github.io/reticulate/) package for R SDK integration 