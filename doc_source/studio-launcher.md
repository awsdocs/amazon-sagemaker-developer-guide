# Use the Amazon SageMaker Studio Launcher<a name="studio-launcher"></a>

You can use the Amazon SageMaker Studio Launcher to create notebooks and text files, and launch terminals and interactive Python shells\.

You can open Studio Launcher in any of the following ways:
+ Choose **Amazon SageMaker Studio** at the top\-left of Studio\.
+ Use the keyboard shortcut `Ctrl + Shift + L`\.
+ From the Studio menu, choose **File** and then choose **New Launcher**\.
+ If the Studio file browser is open, choose the plus \(**\+**\) sign on the Studio file browser menu\.

The Launcher opens in a new tab in Studio\. Your screen should look similar to the following:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-new-launcher.png)

The Launcher consists of following sections:
+ **Get started** – Provides material to get started using SageMaker Studio, such as videos and tutorials, and one\-click solutions for machine learning problems\.
+ **ML tasks and components** – Create machine learning tasks and components, such as new feature groups, data flows, and projects\.
+ **Notebooks and compute resources** – Create a notebook, open an image terminal, or open a Python console\.
+ **Utilities and files** – Show contextual help from a notebook, create files, or open a system terminal\.

**Topics**
+ [Notebooks and compute resources](#studio-launcher-launch)
+ [Utilities and files](#studio-launcher-other)

## Notebooks and compute resources<a name="studio-launcher-launch"></a>

To create or launch an item, choose the SageMaker image that you want the item to run in from the **SageMaker image** dropdown menu\. You can also select the Lifecycle Configuration script that you want to run\. For more information, see [Use Lifecycle Configurations with Amazon SageMaker Studio](studio-lcc.md)\. Next, choose the item\. When you choose an item from this section, you might incur additional usage charges\. For more information, see [Usage Metering](notebooks-usage-metering.md)\.

The following items are available:
+ **Notebook**

  Launches the notebook in a kernel session on the chosen SageMaker image\. For more information, see [Change a Kernel](notebooks-run-and-manage-change-image.md)\.

  Creates the notebook in the folder that you have currently selected in the file browser\. To view the file browser, in the left sidebar of Studio, choose the **File Browser** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/File_browser_squid.png)\)\.
+ **Console**

  Launches the shell in a kernel session on the chosen SageMaker image\.

  Opens the shell in the folder that you have currently selected in the file browser\.
+ **Image terminal**

  Launches the terminal in a terminal session on the chosen SageMaker image\.

  Opens the terminal in the root folder for the user \(as shown by the Home folder in the file browser\)\.

## Utilities and files<a name="studio-launcher-other"></a>

Items in this section run in the context of SageMaker Studio and don't incur usage charges\.

The following items are available:
+ **Show Contextual Help**

  Opens a new tab that displays contextual help for functions in a Studio notebook\. To display the help, choose a function in an active notebook\. To make it easier to see the help in context, drag the help tab so that it's adjacent to the notebook tab\. To open the help tab from within a notebook, press `Ctrl + I`\.

  The following screenshot shows the contextual help for the `Experiment.create` method\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-context-help.png)
+ **System terminal**

  Opens a bash shell in the root folder for the user \(as shown by the Home folder in the file browser\)\.
+ **Text File** and **Markdown File**

  Creates a file of the associated type in the folder that you have currently selected in the file browser\. To view the file browser, in the left sidebar, choose the **File Browser** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/File_browser_squid.png)\)\.