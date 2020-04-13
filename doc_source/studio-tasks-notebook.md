# Tasks available from a Studio notebook<a name="studio-tasks-notebook"></a>

Amazon SageMaker Studio can open only notebooks listed in the Studio file browser\. For instructions on adding a notebook to the browser, see [Upload files to Amazon SageMaker Studio](gs-studio-tasks.md#studio-tasks-files) or [Clone and connect to a Git repository](gs-studio-tasks.md#studio-tasks-git)\.

For more information on Studio notebooks, see [Use Amazon SageMaker Studio notebooks](notebooks.md)\.

**To open a notebook**

1. In the left sidebar, choose the **File Browser** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/File_browser_squid.png) \) to display the file browser\.

1. Browse to and double\-click a notebook file to open the notebook in a new tab\.

Your screen should look similar to the following:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-tasks-notebook.png)

From the open notebook, you can perform the following tasks:

**Topics**
+ [Share a Studio notebook](#studio-tasks-notebook-share)
+ [Change a notebook kernel](#studio-tasks-notebook-change-kernel-type)
+ [Shutdown a notebook instance](#studio-tasks-notebook-shutdown-instance)

## Share a Studio notebook<a name="studio-tasks-notebook-share"></a>

You can share a notebook with a colleague by sending them a link\. When your colleague opens the link, a read\-only version of the notebook is opened in their Amazon SageMaker Studio\. They can save an editable copy and continue working on the notebook\.

**To share a notebook**

1. Choose **Share** at the top right of the notebook to display the **Create shareable snapshot** dialog\.

1. Optionally, choose any of the following items:
   + **Include Git repo information** – Includes a link to the Git repository that contains the notebook
   + **Include output** – Includes all notebook output that has been saved

1. Choose **Create**\.

1. After the snapshot is created, choose **Copy link** and then choose **Close**\.

1. Share the link with your colleague\.

## Change a notebook kernel<a name="studio-tasks-notebook-change-kernel-type"></a>

**To change the kernel type**

1. The current kernel name is displayed at the top right of a notebook, which is `Python 3 (Data Science)` in the preceding screenshot\. Choose the kernel name to open the **Select Kernel** dialog\.

1. Use the dropdown menu to choose a new kernel then choose **Select**\.

The status of the kernel is displayed on the lower left of the screen\. While waiting for the kernel to start, you can edit text cells in the notebook but you must wait until the kernel is started to run a code cell\.

## Shutdown a notebook instance<a name="studio-tasks-notebook-shutdown-instance"></a>

When you shutdown a notebook instance, any unsaved information in the notebook is lost\. The notebook is not deleted\.

**To shutdown the notebook**

1. To save the notebook contents, on the notebook menu, choose the **Disk** icon\.

1. On the top menu of Studio, choose **File** > **Close and Shutdown Notebook**\.

1. Choose **OK**\.