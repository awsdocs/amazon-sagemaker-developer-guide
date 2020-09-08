# Use the Amazon SageMaker Studio Launcher<a name="studio-launcher"></a>

You can use the Amazon SageMaker Studio Launcher to create notebooks and text files, and launch terminals and interactive Python shells\.

You can open Studio Launcher in any of the following ways:
+ From the Studio menu, choose **File** and then choose **New Launcher**\.
+ Use the keyboard shortcut `Ctrl + Shift + L`\.
+ If the Studio file browser is open, choose the plus \(**\+**\) sign on the Studio file browser menu\.

The Launcher opens in a new tab in Studio\. Your screen should look similar to the following:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-launcher.png)

The Launcher consists of two sections, **Launch a new activity** and **Other**\. When you choose an item in the **Launch** section, a kernel session or terminal session is started in the chosen SageMaker image\. When you choose an item in the **Other** section, a session isn't started and the item runs in the context of Studio\.

**Topics**
+ [Launch a New Activity](#studio-launcher-launch)
+ [Other Actions](#studio-launcher-other)

## Launch a New Activity<a name="studio-launcher-launch"></a>

To create or launch an item, choose the SageMaker image that you want the item to run in from the **SageMaker image** dropdown menu\. Next, choose the item\. When you choose an item from this section, you might incur additional usage charges\. For more information, see [Usage Metering](notebooks-usage-metering.md)\.

The following items are available:
+ **Notebook**

  Launches the notebook in a kernel session on the chosen SageMaker image\. For more information, see [Change an Amazon SageMaker Image](notebooks-run-and-manage-change-image.md)\.

  Creates the notebook in the folder that you have currently selected in the file browser\. To view the file browser, in the left sidebar of Studio, choose the **File Browser** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/File_browser_squid.png)\)\.
+ **Interactive \(Python\) Shell**

  Launches the shell in a kernel session on the chosen SageMaker image\.

  Opens the shell in the folder that you have currently selected in the file browser\.
+ **Terminal**

  Launches the terminal in a terminal session on the chosen SageMaker image\.

  Opens the terminal in the root folder for the user \(as shown by the Home folder in the file browser\)\.

## Other Actions<a name="studio-launcher-other"></a>

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