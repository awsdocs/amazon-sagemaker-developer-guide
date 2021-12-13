# Shut down resources<a name="studio-lab-use-shutdown"></a>

In this guide, you will learn how to shut down individual resources, including notebooks, terminals, and kernels\. You can also shut down all resources in one of these categories at the same time\.

**Topics**
+ [Shut down an open notebook](#studio-lab-use-shutdown-notebook)
+ [Shut down resources](#studio-lab-use-shutdown-resources)

## Shut down an open notebook<a name="studio-lab-use-shutdown-notebook"></a>

You can shut down an open notebook from the Amazon SageMaker Studio Lab **File** menu or from the **Running Terminals and Kernels** pane\.

**Note**  
When you shut down a notebook, any unsaved information in the notebook is lost\. The notebook is not deleted\.

**To shut down an open notebook from the File menu**

1. Save the notebook contents by choosing the ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/notebook-save-and-checkpoint.png) icon, located in the notebook menu\.

1. Choose **File** then **Close and Shutdown Notebook**\.

1. Choose **OK**\.

## Shut down resources<a name="studio-lab-use-shutdown-resources"></a>

On the left sidebar of Studio Lab, you will find the **Running Terminals and Kernels** pane and ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Running_squid@2x.png) icon\. The **Running Terminals and Kernels** pane has three sections\. Each section lists all of the resources of that type\. You can shut down each resource individually, or shut down all resources in a section simultaneously\.

When you shut down all resources in a section, the following occurs:
+ **KERNELS** – All kernels, notebooks, and consoles are shut down\.
+ **TERMINALS** – All terminals are shut down\.

**To shut down resources**

1. In the left sidebar, choose the **Running Terminals and Kernels** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Running_squid@2x.png)\)\.

1. Do either of the following:
   + To shut down a specific resource: Choose the **SHUT DOWN** icon on the same row as the resource\.
   + To shut down all resources in a section: Choose **Shut Down All**, which is located to the right of the section label\. After a confirmation dialog box appears, choose **Shut down all** to proceed\.