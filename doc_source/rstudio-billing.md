# Manage billing and cost<a name="rstudio-billing"></a>

 To track the costs associated with your RStudio environment, you can use the AWS Billing and Cost Management service\. AWS Billing and Cost Management provides useful tools to help you gather information related to your cost and usage, analyze your cost drivers and usage trends, and take action to budget your spending\. For more information, see [What is AWS Billing and Cost Management?](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-what-is.html)\. 

 The following describes components required to run RStudio on Amazon SageMaker and how each component factors into billing for your RStudio instance\. 
+  RStudio License –There is no extra charge for using your RStudio license with Amazon SageMaker\. For more information about your RStudio license, see [RStudio license](rstudio-license.md)\.
+  RSession \- These are RStudio working sessions launched by end users\. You are charged while the RSession is running\.
+  RStudio Server \- A multi\-tenant server manages all the RSessions\. You can choose the instance type to run RStudio Server on, and pay the related costs\. The default instance, "system", is free, but you can choose to pay for higher tiers\. For more information about the available instance types for your RStudio Server, see [RStudioServerPro instance type](rstudio-select-instance.md)\. 

 **Tracking billing at user level** 

 To track billing at the user level using Cost Allocation Tags, see [Using Cost Allocation Tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html)\.