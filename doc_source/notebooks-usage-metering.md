# Usage Metering<a name="notebooks-usage-metering"></a>

The costs incurred for running Amazon SageMaker Studio notebooks, interactive shells, consoles, and terminals are based on Amazon Elastic Compute Cloud \(Amazon EC2\) instance usage\.

When you launch the following resources, you must choose a SageMaker image and kernel:

**From the Studio Launcher**
+ Notebook
+ Interactive Shell
+ Image Terminal

**From the **File** menu**
+ Notebook
+ Console

When launched, the resource is run on an Amazon EC2 instance of an instance type based on the chosen SageMaker image and kernel\. If an instance of that type was previously launched and is available, the resource is run on that instance\. If an instance of that type is not available, the resource is run on a new default instance of that type\.

For CPU based images, the default instance type is `ml.t3.medium`\. For GPU based images, the default instance type is `ml.g4dn.xlarge`\.

The costs incurred are based on the instance type and the number of instances of each instance type\. You are billed separately for each instance\.

Metering starts when an instance is created\. Metering ends when the SageMaker image running on the instance is shut down\. For information on how to shut down a SageMaker image, see [Shut Down Resources](notebooks-run-and-manage-shut-down.md)\.

**Important**  
You must shut down the SageMaker image on the instance to stop incurring charges\. If you shut down the notebook running on the instance but don't shut down the SageMaker image, you will still incur charges\.

When you open multiple notebooks on the same instance type, the notebooks run on the same instance even if they're using different kernels\. You are billed only for the time that one instance is running\.

You can change the instance type from within the notebook after you open it\. For more information, see [Change an Instance Type](notebooks-run-and-manage-switch-instance-type.md)\.

**Metering Example**

In this example, the customer performed the following actions:
+ Opened notebook \#1\. Studio opens this on a `ml.t3.medium` instance type\.
+ After an hour, opened notebook \#2\. This notebook runs on the same instance as notebook \#1, a `ml.t3.medium` instance type\.
+ After another hour, opened notebook \#3 and immediately switched to a `ml.m5.large` instance type\.
+ After another hour, shut down the kernels on all instance types\.

The notebooks ran for the following amount of time:
+ Notebook \#1: 3 hours
+ Notebook \#2: 2 hours
+ Notebook \#3: 1 hours

The customer is billed for the following usage:

```
3 hours at the ml.t3.medium rate +
1 hour at the ml.m5.large rate
```

During the second and third hours, notebook \#1 and notebook \#2 ran on the same `ml.t3.medium` instance\.

**Important**  
This topic discusses costs only as it applies to the amount of time an instance is running\. Additional costs can be incurred, for example, from running the code in a notebook, such as compute time due to a training job\.

For more information about billing along with pricing examples, see [Amazon SageMaker Pricing](http://aws.amazon.com/sagemaker/pricing/)\. On the pricing page, under one of the AWS Regions supported by Amazon SageMaker Studio, review the **Building** section where two types of notebooks are listed: **On\-Demand ML Notebook Instances** and **Amazon SageMaker Studio Notebook Instances**\. In this example, we're discussing **Amazon SageMaker Studio Notebook Instances**\.