# Amazon SageMaker Studio UI Overview<a name="studio-ui"></a>

Amazon SageMaker Studio extends the JupyterLab interface\. Previous users of JupyterLab will notice the similarity of the user interface, including the workspace\. Studio adds many additions to the interface\. The most prominent additions are detailed in the following sections\. For an overview of the basic JupyterLab interface, see [The JupyterLab Interface](https://jupyterlab.readthedocs.io/en/latest/user/interface.html)\.

The following image shows SageMaker Studio with the file browser open and the Studio landing page displayed\. The Studio version is shown on the bottom\-left of the landing page\. To update to the latest version, see [Update Amazon SageMaker Studio](studio-tasks.md#studio-tasks-update)\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-landing.png)

At the top of the screen is the *menu bar*\. At the left of the screen is the *left sidebar* which contains icons to open file browsers, resource browsers, and tools\. At the right of the screen is the *right sidebar*, represented by the **Settings** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Settings_squid.png) \), which displays contextual property settings when open\. Above the **Settings** icon, there's a button to provide feedback about your experiences with SageMaker Studio\. At the bottom of the screen is the *status bar*\.

The main work area is divided horizontally into two panes\. The left pane is the *file and resource browser*\. The right pane contains one or more tabs for resources such as notebooks, terminals, metrics, and graphs\.

**Topics**
+ [Left sidebar](#studio-ui-nav-bar)
+ [File and resource browser](#studio-ui-browser)
+ [Main work area](#studio-ui-work)
+ [Settings](#studio-ui-prefs)

## Left sidebar<a name="studio-ui-nav-bar"></a>

The left sidebar includes the following icons\. When you hover over an icon, a tooltip displays the icon name\. When you choose an icon, the file and resource browser displays the described functionality\. For hierarchical entries, a selectable breadcrumb at the top of the browser shows your location in the hierarchy\.


| Icon | Description | 
| --- | --- | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/File_browser_squid@2x.png)  |  **File Browser** Choose the **Upload Files** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/File_upload_squid.png)\) to add files to Studio\. Double\-click a file to open the file in a new tab\. To have adjacent files open, choose a tab that contains a notebook, Python, or text file, then choose **New View for File**\. Choose the plus \(**\+**\) sign on the menu at the top of the file browser to open the Studio Launcher\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Git_squid@2x.png)  |  **Git** You can connect to a Git repository and then access a full range of Git tools and operations\. For more information, see [Clone and Connect to a Git Repository](studio-tasks.md#studio-tasks-git)\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Running_squid@2x.png)  |  **Running Terminals, Kernels, and Images** For more information, see [Use the Amazon SageMaker Studio Launcher](studio-launcher.md)\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Commands_squid@2x.png)  |  **Commands \(Ctrl \+ Shift \+ C\)** The majority of the menu commands are available here\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Experiment_list_squid@2x.png)  |  **SageMaker Experiment List** Displays a list of experiments, trials, or trial components\. For more information, see [View and Compare Experiments, Trials, and Trial Components](experiments-view-compare.md)\. This icon is selected when you choose **Create Autopilot experiment** from the Studio landing page\. To create an Autopilot experiment, choose **Create Experiment**\. For more information, see [Automate model development with Amazon SageMaker Autopilot](autopilot-automate-model-development.md)\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Notebook_tools_squid@2x.png)  |  **Notebook Tools** You can access a notebook's metadata through the **Advanced Tools** section\. This icon is displayed only when a notebook is open\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Endpoint_squid@2x.png)  |  **SageMaker Endpoint List** This icon is selected when you choose **Deploy and monitor** from the Studio landing page\. For more information, see [Deploy Models for Inference](deploy-model.md)\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Open_tabs_squid@2x.png)  |  **Open Tabs** Provides a list of open tabs, which is useful if you have multiple open tabs\.  | 

## File and resource browser<a name="studio-ui-browser"></a>

The file and resource browser displays lists of your notebooks, experiments, trials, trial components, and endpoints\. On the menu at the top of the file browser, choose the plus \(**\+**\) sign to open the Studio Launcher\. The Launcher allows you to create a notebook, launch a Python interactive shell, or open a terminal\.

## Main work area<a name="studio-ui-work"></a>

The main work area consists of multiple tabs that contain your open notebooks and terminals, and detailed information about your experiments and endpoints\. One commonly used tab is the **Trial Component List**\. This list is referred to as the *Leaderboard* because it's where you can compare experiments and trials\. For more information, see [View and Compare Amazon SageMaker Experiments, Trials, and Trial Components](experiments-view-compare.md)\.

## Settings<a name="studio-ui-prefs"></a>

The settings pane allows you to adjust table and chart properties\. By default, the pane is hidden on the far right of the screen\. To open the pane, choose the **Settings** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Settings_squid.png) \) on the top right of the screen\.