# RStudioServerPro instance type<a name="rstudio-select-instance"></a>

When deciding which Amazon EC2 instance type to use for your RStudioServerPro app, the main factor to consider is bandwidth\. Bandwidth is important because the RStudioServerPro instance is responsible for serving the RStudio UI to all users\. This includes UI heavy workflows, such as generating figures, animations, and displaying many data rows\. Therefore, there may be some UI performance degradation depending on the workload across all users\. The following are the available instance types to use for your RStudioServerPro\. For pricing information about these instances, see [Amazon SageMaker Pricing](https://aws.amazon.com/sagemaker/pricing/)\.
+ `ml.t3.medium`: This instance type is recommended for Domains with low UI use and is free to use\.
+ `ml.c5.4xlarge`: This instance type is recommended for Domains with moderate UI use\.
+ `ml.c5.9xlarge`: This instance type is recommended for Domains with heavy UI use\.

 **Changing RStudio instance type** 

To change the instance type of your RStudioServerPro, pass the new instance type as part of a call to the `update-domain` CLI command\. You then need to delete the existing RStudioServerPro app using the `delete-app` CLI command and create a new RStudioServerPro app using the `create-app` CLI command\. 