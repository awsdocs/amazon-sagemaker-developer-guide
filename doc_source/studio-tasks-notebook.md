# Perform Common Tasks in a SageMaker Studio Notebook<a name="studio-tasks-notebook"></a>

The following sections describe how to perform basic tasks in an Amazon SageMaker Studio notebook\.

For more detailed information on Studio notebooks and these tasks, see [Use Amazon SageMaker Studio Notebooks](notebooks.md)\.

**Topics**
+ [Open a Studio notebook](#studio-tasks-notebook-open)
+ [Create a SageMaker Studio Notebook](#studio-tasks-notebook-create)
+ [Change a notebook's instance type](#studio-tasks-notebook-change-instance-type)
+ [Change a notebook's image](#studio-tasks-notebook-change-image)
+ [Share a Studio notebook](#studio-tasks-notebook-share)
+ [Shut down a notebook](#studio-tasks-notebook-shut-down-instance)

## Open a Studio notebook<a name="studio-tasks-notebook-open"></a>

Amazon SageMaker Studio can open only notebooks listed in the Studio file browser\. For instructions on adding a notebook to the browser, see [Upload files to Amazon SageMaker Studio](studio-tasks.md#studio-tasks-files) or [Clone and Connect to a Git Repository](studio-tasks.md#studio-tasks-git)\.

**To open a notebook**

1. In the left sidebar, choose the **File Browser** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/File_browser_squid.png)\) to display the file browser\.

1. Browse to a notebook file and double\-click it to open the notebook in a new tab\.

The top menu of your notebook should look similar to the following:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-notebooks-manage-callouts.png)

## Create a SageMaker Studio Notebook<a name="studio-tasks-notebook-create"></a>

You can create a notebook from the Amazon SageMaker Studio file menu or the Studio launcher\.

**To create a notebook from the file menu**

1. On the top Studio menu, choose **File** > **New** > **Notebook**\. The notebook is opened in a new tab\.

1. In the **Select Kernel** dialog, choose a kernel, then choose **Select**\.

**To create a notebook from the launcher**

1. If the launcher is not displayed, on the top Studio menu, choose **File** > **New Launcher**\. The launcher is opened in a new tab\.

1. To create a notebook that uses the default kernel, `Python 3 (Data Science)`, under **Notebook**, choose **Python 3**\. To change the kernel, see [Change a notebook's image](#studio-tasks-notebook-change-image)\.

1. To create a notebook that uses a different kernel, use the selector under **Choose a SageMaker Image to start**\. Next, choose **Python 3**\. The notebook is opened in a new tab\.

For more information, see [Create an Amazon SageMaker Studio Notebook](notebooks-create-notebook.md)\.

## Change a notebook's instance type<a name="studio-tasks-notebook-change-instance-type"></a>

Information about the current instance type is displayed at the top right of a notebook\. In the screenshot at the top of the page, this is `2 vCPU + 4 GiB`\.

**Note**  
If you change the instance type, existing setting for the notebook are lost and installed package must be re\-installed\.

**To change the instance type**

1. Choose the instance type to open the **Select instance** dialog\.

1. Only fast launch instance types are initially displayed\. Choose one of these types or to display all instance types, toggle the **Fast launch only** selector off\.

1. After choosing a type, choose **Save and continue**\.

1. Wait for the new instance to become enabled\.

For more information, see [Change an Instance Type](notebooks-run-and-manage-switch-instance-type.md)\.

## Change a notebook's image<a name="studio-tasks-notebook-change-image"></a>

The current kernel name is displayed at the top right of a notebook\. In the screenshot at the top of the page, this is `Python 3 (Data Science)`\. `Python 3` denotes the kernel and `Data Science` denotes the SageMaker image that contains the kernel\.

**To change the image**

1. Choose the kernel name to open the **Select Kernel** dialog\.

1. Use the dropdown menu to choose a new kernel and then choose **Select**\.

The status of the kernel is displayed on the lower left of the screen\. While waiting for the kernel to start, you can edit text cells in the notebook but you must wait until the kernel is started to run a code cell\.

For more information, see [Change a SageMaker Image](notebooks-run-and-manage-change-environment.md)\.

## Share a Studio notebook<a name="studio-tasks-notebook-share"></a>

You can share a notebook with a colleague by sending them a link\. When your colleague opens the link, a read\-only version of the notebook is opened in their Amazon SageMaker Studio\. They can save an editable copy to continue working on the notebook\.

**To share a notebook**

1. Choose **Share** at the top right of the notebook to display the **Create shareable snapshot** dialog\.

1. Optionally, choose any of the following items:
   + **Include Git repo information** – Includes a link to the Git repository that contains the notebook\. This enables you and your colleague to collaborate and contribute to the same Git repository\.
   + **Include output** – Includes all notebook output that has been saved\.

1. Choose **Create**\.

1. After the snapshot is created, choose **Copy link** and then choose **Close**\.

1. Share the link with your colleague\.

For more information, see [Share and Use a Studio Notebook](notebooks-sharing.md)\.

## Shut down a notebook<a name="studio-tasks-notebook-shut-down-instance"></a>

When you shut down a notebook, any unsaved information in the notebook is lost\. The notebook is not deleted\.

**To shut down a notebook**

1. To save the notebook contents, on the left of the notebook menu, choose the **Disk** icon\.

1. On the top menu of Studio, choose **File** > **Close and Shutdown Notebook**\.

1. Choose **OK**\.

For more information, see [Shut Down Resources](notebooks-run-and-manage-shut-down.md)\.