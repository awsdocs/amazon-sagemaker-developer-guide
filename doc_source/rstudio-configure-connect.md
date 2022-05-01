# RStudio Connect URL<a name="rstudio-configure-connect"></a>

RStudio Connect is a publishing platform for Shiny applications, R Markdown reports, dashboards, plots, and more\. RStudio Connect makes it easy to surface machine learning and data science insights by making hosting content simple and scalable\. If you have an RStudio Connect server, then you can set the server as the default place where apps are published\. For more information about RStudio Connect, see [RStudio Connect](https://www.rstudio.com/products/connect/)\.

When you onboard to RStudio on Amazon SageMaker Domain, an RStudio Connect server is not created\. You can create an RStudio Connect server on an Amazon EC2 instance to use Connect with Amazon SageMaker Domain\. For information about how to set up your RStudio Connect server, see [Host RStudio Connect and Package Manager for ML development in RStudio on Amazon SageMaker](http://aws.amazon.com/blogs/machine-learning/host-rstudio-connect-and-package-manager-for-ml-development-in-rstudio-on-amazon-sagemaker/)\. 

 **Add an RStudio Connect URL** 

If you have an RStudio Connect URL, you can update the default URL so that your RStudio Users can publish to it\. 

1.  Navigate to the **Control Panel**\. 

1.  Under Domain, choose **Edit Settings**\. This opens a new page\. 

1.  From the new page, select **RStudio Settings** on the left side\.  

1.  Under **RStudio Connect URL**, enter the RStudio Connect URL to add\. 

1.  Select **Submit**\. 

 **CLI** 

 You can set a default RStudio Connect URL when you create your Amazon SageMaker Domain\. The only way to update your RStudio Connect URL from the AWS CLI is to delete your Domain and create a new one with the updated RStudio Connect URL\. 