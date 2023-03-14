# RStudio Package Manager<a name="rstudio-configure-pm"></a>

RStudio Package Manager is a repository management server used to organize and centralize packages across your organization\. For more information on RStudio Package Manager, see [RStudio Package Manager](https://www.rstudio.com/products/package-manager/)\. If you don't supply your own Package Manager URL, Amazon SageMaker Domain uses the default Package Manager repository when you onboard RStudio following the steps in [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\. For more information, see [Host RStudio Connect and Package Manager for ML development in RStudio on Amazon SageMaker](http://aws.amazon.com/blogs/machine-learning/host-rstudio-connect-and-package-manager-for-ml-development-in-rstudio-on-amazon-sagemaker/)\. 

 **Update Package Manager URL** 

You can update the Package Manager URL used for your RStudio\-enabled Domain as follows\.

1. Navigate to the **Domains** page\. 

1. Select the desired Domain\.

1. Choose **Domain Settings**\.

1. Under **General Settings**, select **Edit**\.

1.  From the new page, select **RStudio Settings** on the left side\.  

1.  Under **RStudio Package Manager**, enter your RStudio Package Manager URL\. 

1.  Select **Submit**\. 

 **CLI** 

The only way to update your Package Manager URL from the AWS CLI is to delete your Domain and create a new one with the updated Package Manager URL\. 