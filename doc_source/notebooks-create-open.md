# Create or Open an Amazon SageMaker Studio Notebook<a name="notebooks-create-open"></a>

When you [Create a Notebook from the File Menu](#notebooks-create-file-menu) in Amazon SageMaker Studio or [Open a notebook in Studio](#notebooks-open) for the first time, you are prompted to set up your environment by choosing a SageMaker image, a kernel, an instance type, and, optionally, a lifecycle configuration script that runs on image start\-up\. SageMaker launches the notebook on an instance of the chosen type\. By default, the instance type is set to `ml.t3.medium` \(available as part of the [AWS Free Tier](http://aws.amazon.com/free)\) for CPU\-based images\. For GPU\-based images, the default instance type is `ml.g4dn.xlarge`\.

If you create or open additional notebooks that use the same instance type, whether or not the notebooks use the same kernel, the notebooks run on the same instance of that instance type\.

After you launch a notebook, you can change its instance type, SageMaker image, and kernel from within the notebook\. For more information, see [Change an Instance Type](notebooks-run-and-manage-switch-instance-type.md) and [Change an Image or a Kernel](notebooks-run-and-manage-change-image.md)\.

**Note**  
You can have only one instance of each instance type\. Each instance can have multiple SageMaker images running on it\. Each SageMaker image can run multiple kernels or terminal instances\. 

Billing occurs per instance and starts when the first instance of a given instance type is launched\. If you want to create or open a notebook without the risk of incurring charges, open the notebook from the **File** menu and choose **No Kernel** from the **Select Kernel** dialog\. You can read and edit a notebook without a running kernel but you can't run cells\.

Billing ends when the SageMaker image for the instance is shut down\. For more information, see [Usage Metering](notebooks-usage-metering.md)\.

For information about shutting down the notebook, see [Shut Down Resources](notebooks-run-and-manage-shut-down.md#notebooks-run-and-manage-shut-down-sessions)\.

**Topics**
+ [Open a notebook in Studio](#notebooks-open)
+ [Create a Notebook from the File Menu](#notebooks-create-file-menu)
+ [Create a Notebook from the Launcher](#notebooks-create-launcher)
+ [List of the available instance types, images, and kernels](#notebooks-instance-image-kernels)

## Open a notebook in Studio<a name="notebooks-open"></a>

Amazon SageMaker Studio can only open notebooks listed in the Studio file browser\. For instructions on uploading a notebook to the file browser, see [Upload Files to SageMaker Studio](studio-tasks-files.md) or [Clone a Git Repository in SageMaker Studio](studio-tasks-git.md)\.

**To open a notebook**

1. In the left sidebar, choose the **File Browser** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/icons/folder.png)\) to display the file browser\.

1. Browse to a notebook file and double\-click it to open the notebook in a new tab\.

## Create a Notebook from the File Menu<a name="notebooks-create-file-menu"></a>

**To create a notebook from the File menu**

1. From the Studio menu, choose **File**, choose **New**, and then choose **Notebook**\.

1. In the **Change environment** dialog, use the dropdown menus to select your **Image**, **Kernel**, **Instance type**, and **Start\-up script**, then choose **Select**\. Your notebook launches and opens in a new Studio tab\.  
![\[Studio notebook environment setup.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-notebook-environment-setup.png)

## Create a Notebook from the Launcher<a name="notebooks-create-launcher"></a>

**To create a notebook from the Launcher**

1. To open the Launcher, choose **Amazon SageMaker Studio** at the top left of the Studio interface or use the keyboard shortcut `Ctrl + Shift + L`\.

   To learn about all the available ways to open the Launcher, see [Use the Amazon SageMaker Studio Launcher](studio-launcher.md)

1. In the Launcher, in the **Notebooks and compute resources** section, choose **Change environment**\.  
![\[SageMaker Studio set notebook environment.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-launcher-notebook-creation.png)

1. In the **Change environment** dialog, use the dropdown menus to select your **Image**, **Kernel**, **Instance type**, and **Start\-up script**, then choose **Select**\.

1. In the Launcher, choose **Create notebook**\. Your notebook launches and opens in a new Studio tab\.

To view the notebook's kernel session, in the left sidebar, choose the **Running Terminals and Kernels** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/icons/running-terminals-kernels.png)\)\. You can stop the notebook's kernel session from this view\.

## List of the available instance types, images, and kernels<a name="notebooks-instance-image-kernels"></a>

For a list of all available resources, see:
+ [Available Studio Instance Types](notebooks-available-instance-types.md)
+ [Available Amazon SageMaker Images](notebooks-available-images.md)
+ [Available Amazon SageMaker Kernels](notebooks-available-kernels.md)