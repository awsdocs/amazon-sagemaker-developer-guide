# Usage Metering<a name="notebooks-usage-metering"></a>

There is no additional charge for using Amazon SageMaker Studio\. The costs incurred for running Amazon SageMaker Studio notebooks, interactive shells, consoles, and terminals are based on Amazon Elastic Compute Cloud \(Amazon EC2\) instance usage\.

When you run the following resources, you must choose a SageMaker image and kernel:

**From the Studio Launcher**
+ Notebook
+ Interactive Shell
+ Image Terminal

**From the **File** menu**
+ Notebook
+ Console

When launched, the resource is run on an Amazon EC2 instance of an instance type based on the chosen SageMaker image and kernel\. If an instance of that type was previously launched and is available, the resource is run on that instance\.

For CPU based images, the default instance type is `ml.t3.medium`\. For GPU based images, the default instance type is `ml.g4dn.xlarge`\.

The costs incurred are based on the instance type\. You are billed separately for each instance\.

Metering starts when an instance is created\. Metering ends when all the apps on the instance are shut down, or the instance is shut down\. For information on how to shut down an instance, see [Shut Down Resources](notebooks-run-and-manage-shut-down.md)\.

**Important**  
You must shut down the instance to stop incurring charges\. If you shut down the notebook running on the instance but don't shut down the instance, you will still incur charges\.

When you open multiple notebooks on the same instance type, the notebooks run on the same instance even if they're using different kernels\. You are billed only for the time that one instance is running\.

You can change the instance type from within the notebook after you open it\. For more information, see [Change an Instance Type](notebooks-run-and-manage-switch-instance-type.md)\.

For information about billing along with pricing examples, see [Amazon SageMaker Pricing](http://aws.amazon.com/sagemaker/pricing/)\.