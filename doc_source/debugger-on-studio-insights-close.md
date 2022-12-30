# Shut Down the Amazon SageMaker Debugger Insights Instance<a name="debugger-on-studio-insights-close"></a>

When you are not using the Debugger insights dashboard, shut down the instance on which it runs to avoid incurring additional fees\.

**To shut down the Debugger insights instance in Studio**

![\[An animated screenshot that shows how to shut down a Debugger insights dashboard instance.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-studio-insights-shut-down.png)

1. In Studio, select the **Running Instances and Kernels** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Running_squid.png)\)\. 

1. Under the **RUNNING APPS** list, look for the **sagemaker\-debugger\-1\.0** app\. Select the shutdown icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Shutdown_light.png)\) next to the app\. The Debugger insights dashboards run on an `ml.m5.4xlarge` instance\. This instance also disappears from the **RUNNING INSTANCES** when you shut down the **sagemaker\-debugger\-1\.0** app\. 