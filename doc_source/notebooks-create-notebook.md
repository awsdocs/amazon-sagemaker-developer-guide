# Create an Amazon SageMaker Studio Notebook<a name="notebooks-create-notebook"></a>

 When you create a notebook in Amazon SageMaker Studio or open a non\-shared notebook in Studio for the first time, you have to select a SageMaker image and kernel for the notebook\. Amazon SageMaker launches the notebook on a default instance of a type based on the chosen SageMaker image\. For CPU based images, the default instance type is `ml.t3.medium` \(available as part of the [AWS Free Tier](http://aws.amazon.com/free)\)\. For GPU based images, the default instance type is `ml.g4dn.xlarge`\. 

 If you create or open additional notebooks that use the same instance type, whether or not the notebooks use the same kernel, the notebooks run on the same instance of that instance type\. 

 Billing occurs per instance and starts when the first instance of a given instance type is launched\. If you want to create or open a notebook without the risk of incurring charges, open the notebook from the **File** menu and choose **No Kernel** from the **Select Kernel** dialog\. You can read and edit a notebook without a kernel but you can't run code cells\. 

 Billing ends when the SageMaker image for the instance is shutdown\. For more information, see [Usage Metering](notebooks-usage-metering.md)\. 

 After a notebook is launched, you can change the instance type from within the notebook\. For more information, see [Change an Instance Type](notebooks-run-and-manage.md#notebooks-run-and-manage-switch-instance-type)\. 

 For information on stopping the notebook, see [Shut Down Sessions and Images](notebooks-run-and-manage-shut-down.md#notebooks-run-and-manage-shut-down-sessions)\. 

 After you're logged in to Amazon SageMaker Studio, you can create a notebook in the following ways: 
+ From the **File** menu
+ From the Amazon SageMaker Studio Launcher

**Topics**
+ [Create a Notebook from the File Menu](#notebooks-create-notebook-file-menu)
+ [Create a Notebook from the Launcher](#notebooks-create-notebook-launcher)

## Create a Notebook from the File Menu<a name="notebooks-create-notebook-file-menu"></a>

**To create a notebook from the File menu**

1. From the menu at the top of Studio, choose **File**, choose **New**, and then choose **Notebook**\.

1. On the **Select Kernel** dialog, to use the default kernel, **Python 3 \(Data Science\)**, choose **Select**\. Otherwise, use the dropdown menu to select a different kernel\.

For a list of the available kernels, see [Available SageMaker Kernels](notebooks-available-kernels.md)\.

## Create a Notebook from the Launcher<a name="notebooks-create-notebook-launcher"></a>

**To create a notebook from the Launcher**

1. In the left sidebar, choose the **File Browser** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/File_browser_squid.png)\)\.

1. On the File Browser menu, choose the plus \(**\+**\) sign to display the Launch in a new tab\.

   The keyboard shortcut for the these steps is `Ctrl + Shift + L`\.

1. On the Launcher, keep the default SageMaker image, **Data Science**, or use the dropdown menu to select a different image\.

1. Under **Notebook**, choose **Python3**\.

For a list of the available images, see [Available SageMaker Images](notebooks-available-images.md)\.

After you choose the kernel, your new notebook launches and opens in a new Studio tab\. To view the notebook's kernel session, in the left sidebar, choose the **Running Terminals, Kernels, and Images** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Running_squid.png)\)\. You can stop the notebook's kernel session from this view\.