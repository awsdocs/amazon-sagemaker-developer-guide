# Shut Down Resources<a name="notebooks-run-and-manage-shut-down"></a>

You can shut down individual resources, including notebooks, terminals, kernels, apps, and instances\. You can also shut down all resources in one of these categories at the same time\.

**Topics**
+ [Shut Down an Open Notebook](#notebooks-run-and-manage-shut-down-notebook)
+ [Shut Down Resources](#notebooks-run-and-manage-shut-down-sessions)

## Shut Down an Open Notebook<a name="notebooks-run-and-manage-shut-down-notebook"></a>

You can shut down an open notebook from the Amazon SageMaker Studio **File** menu or from the Running Terminal and Kernels pane\.

**Note**  
When you shut down a notebook, any unsaved information in the notebook is lost\. The notebook is not deleted\.

**To shut down an open notebook from the File menu**

1. Optionally, save the notebook contents by choosing the **Disk** icon on the left of the notebook menu\.

1. Choose **File** then **Close and Shutdown Notebook**\.

1. Choose **OK**\.

## Shut Down Resources<a name="notebooks-run-and-manage-shut-down-sessions"></a>

The **Running Terminals and Kernels** pane consists of four sections\. Each section lists all the resources of that type\. You can shut down each resource individually or shut down all the resources in a section at the same time\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-notebook-shutdown-resource.png)

When you choose to shut down all resources in a section, the following occurs:
+ **RUNNING INSTANCES/RUNNING APPS** – All instances, apps, notebooks, kernel sessions, consoles/shells, and image terminals are shut down\. System terminals aren't shut down\. Choose this option to stop the accrual of all charges\.
+ **KERNEL SESSIONS** – All kernels, notebooks and consoles/shells are shut down\.
+ **TERMINAL SESSIONS** – All image terminals and system terminals are shut down\.

**To shut down resources**

1. In the left sidebar, choose the **Running Terminals and Kernels** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Shutdown_light.png)\)\.

1. Do either of the following:
   + To shut down a specific resource, choose the **SHUT DOWN** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/File_upload_squid.png)\) on the same row as the resource\.

     For running instances, a confirmation dialog lists all the resources that will be shut down\. For running apps, a confirmation dialog is displayed\. Choose **Shut down all** to proceed\.
**Note**  
No confirmation dialog is displayed for kernel sessions or terminal sessions\.
   + To shut down all resources in a section, choose the **X** to the right of the section label\. A confirmation dialog is displayed\. Choose **Shut down all** to proceed\.