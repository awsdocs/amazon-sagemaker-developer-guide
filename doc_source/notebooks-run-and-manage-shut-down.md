# Shut Down Resources<a name="notebooks-run-and-manage-shut-down"></a>

You can shut down individual resources, including notebooks, terminals, kernels, and images, or you can shut down all resources in one of these categories at the same time\.

**Topics**
+ [Shut Down an Open Notebook](#notebooks-run-and-manage-shut-down-notebook)
+ [Shut Down Sessions and Images](#notebooks-run-and-manage-shut-down-sessions)

## Shut Down an Open Notebook<a name="notebooks-run-and-manage-shut-down-notebook"></a>

You can shut down an open notebook from the Amazon SageMaker Studio **File** menu or from the Sessions and Images dialog box\.

**Note**  
When you shut down a notebook, any unsaved information in the notebook is lost\. The notebook is not deleted\.

**To shut down an open notebook from the File menu**

1. Save the notebook contents by choosing the **Disk** icon on the left of the notebook menu\.

1. Choose **File** then **Close and Shutdown Notebook**\.

1. Choose **OK**\.

## Shut Down Sessions and Images<a name="notebooks-run-and-manage-shut-down-sessions"></a>

The **Running Terminals and Kernels** browser consists of three sections: **TERMINAL SESSIONS**, **KERNEL SESSIONS**, and **RUNNING IMAGES**\. Each section lists all resources of that type\. You can shut down each resource individually or shut down all resources in a section at the same time\.

When you choose to shut down all resources in a section, the following occurs:
+ **TERMINAL SESSIONS** – All terminals are shut down and their tabs deleted\.
+ **KERNEL SESSIONS** – All kernels \(notebooks and consoles/shells\) are shut down\.
+ **RUNNING IMAGES** – All SageMaker images, kernel sessions, SageMaker image terminals, and instances are shut down\. System terminals aren't shut down\.

**To shut down terminal sessions, kernel sessions, and images**

1. In the left sidebar, choose the **Running Terminals and Kernels** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Running_squid.png)\)\.

1. Do either of the following:
   + To shut down a specific resource, choose **SHUT DOWN** on the same row as the resource\. A confirmation dialog is displayed\.

     For running images, the confirmation dialog lists the terminals, consoles, and notebooks that will be shut down\.
   + To shut down all resources in a section, choose the **X** to the right of the section label\. A confirmation dialog is displayed\.

     Choose **Shut Down All** to proceed\.