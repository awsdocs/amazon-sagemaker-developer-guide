# Shut Down Data Wrangler<a name="data-wrangler-shut-down"></a>

When you are not using Data Wrangler, it is important to shut down the instance on which it runs to avoid incurring additional fees\. 

To avoid loosing work, save your data flow before shutting Data Wrangler down\. To save your data flow in Studio, select **File** and then select **Save Data Wrangler Flow**\. Note that Data Wrangler automatically saves your data flow every 60 seconds\. 

**To shut down the Data Wrangler instance in Studio:**

1. In Studio, select the **Running Instances and Kernels** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Running_squid.png)\)\. 

1. Under **RUNNING APPS** is the **sagemaker\-data\-wrangler\-1\.0** app\. Select the shut down icon next to this app \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Shutdown_light.png)\)\. 

   Data Wrangler runs on an ml\.m5\.4xlarge instance\. This instance disappears from **RUNNING INSTANCES** when you shut down the Data Wrangler app\.

After you shut down the Data Wrangler app, it has to restart the next time you open a Data Wrangler flow file\. This can take a few minutes\. 