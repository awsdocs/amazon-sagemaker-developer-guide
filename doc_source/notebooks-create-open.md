# Create or Open an Amazon SageMaker Studio Notebook<a name="notebooks-create-open"></a>

When you create a notebook in Amazon SageMaker Studio or open a non\-shared notebook in Studio for the first time, you have to select a SageMaker image and kernel for the notebook\. SageMaker launches the notebook on a default instance of a type based on the chosen SageMaker image\. For CPU based images, the default instance type is `ml.t3.medium` \(available as part of the [AWS Free Tier](http://aws.amazon.com/free)\)\. For GPU based images, the default instance type is `ml.g4dn.xlarge`\.

If you create or open additional notebooks that use the same instance type, whether or not the notebooks use the same kernel, the notebooks run on the same instance of that instance type\.

After a notebook is launched, you can change its instance type, and SageMaker image and kernel from within the notebook\. For more information, see [Change an Instance Type](notebooks-run-and-manage-switch-instance-type.md) and [Change an Amazon SageMaker Image](notebooks-run-and-manage-change-image.md)\.

You can have one instance of each instance type\. Each instance can have only one SageMaker image running on it at a time\. However, a SageMaker image can run multiple kernels or terminal instances\. 

Billing occurs per instance and starts when the first instance of a given instance type is launched\. If you want to create or open a notebook without the risk of incurring charges, open the notebook from the **File** menu and choose **No Kernel** from the **Select Kernel** dialog\. You can read and edit a notebook without a running kernel but you can't run cells\.

Billing ends when the SageMaker image for the instance is shut down\. For more information, see [Usage Metering](notebooks-usage-metering.md)\.

For information on shutting down the notebook, see [Shut Down Sessions and Images](notebooks-run-and-manage-shut-down.md#notebooks-run-and-manage-shut-down-sessions)\.

**Topics**
+ [Open a Studio notebook](#notebooks-open)
+ [Create a Notebook from the File Menu](#notebooks-create-file-menu)
+ [Create a Notebook from the Launcher](#notebooks-create-launcher)

## Open a Studio notebook<a name="notebooks-open"></a>

SageMaker Studio can only open notebooks listed in the Studio file browser\. For instructions on uploading a notebook to the file browser, see [Upload files to Amazon SageMaker Studio](studio-tasks.md#studio-tasks-files) or [Clone and Connect to a Git Repository](studio-tasks.md#studio-tasks-git)\.

**To open a notebook**

1. In the left sidebar, choose the **File Browser** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/File_browser_squid.png)\) to display the file browser\.

1. Browse to a notebook file and double\-click it to open the notebook in a new tab\.

The top menu of your notebook should look similar to the following:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-notebooks-manage-callouts.png)

## Create a Notebook from the File Menu<a name="notebooks-create-file-menu"></a>

**To create a notebook from the File menu**

1. From the Studio menu, choose **File**, choose **New**, and then choose **Notebook**\.

1. On the **Select Kernel** dialog, to use the default kernel, **Python 3 \(Data Science\)**, choose **Select**\. Otherwise, use the dropdown menu to select a different kernel\.

For a list of the available kernels, see [Available Amazon SageMaker Kernels](notebooks-available-kernels.md)\.

## Create a Notebook from the Launcher<a name="notebooks-create-launcher"></a>

**To create a notebook from the Launcher**

1. To open the Launcher, use the keyboard shortcut `Ctrl + Shift + L`\.

   Alternatively, from the File Browser, choose the plus \(**\+**\) sign on the left of the menu\.

1. On the Launcher, keep the default SageMaker image, **Data Science**, or use the dropdown menu to select a different image\.

1. Under **Notebook**, choose **Python3**\.

For a list of the available images, see [Available Amazon SageMaker Images](notebooks-available-images.md)\.

After you choose the kernel or image, your notebook launches and opens in a new Studio tab\. To view the notebook's kernel session, in the left sidebar, choose the **Running Terminals, Kernels, and Images** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Running_squid.png)\)\. You can stop the notebook's kernel session from this view\.