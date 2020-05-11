# Amazon SageMaker Studio UI Overview<a name="studio-ui"></a>

Amazon SageMaker Studio extends the JupyterLab interface\. Previous users of JupyterLab will notice the similarity of the user interface, including the workspace\. Studio adds many additions to the interface\. The most prominent additions are detailed in the following sections\. For an overview of the basic JupyterLab interface, see [The JupyterLab Interface](https://jupyterlab.readthedocs.io/en/latest/user/interface.html)\.

The following image shows the initial screen when you sign\-on to Amazon SageMaker Studio\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-landing.png)

At the top of the screen is the *menu bar*\. At the left of the screen is the *left sidebar* which contains icons to open different file and resource browsers\. At the right of the screen is the *right sidebar*, represented by the **Settings** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Settings_squid.png) \), which displays contextual property settings\. At the bottom of the screen is the *status bar*\.

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
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/File_browser_squid@2x.png)  | File Browser\. On the menu at the top of the file browser, choose the plus \(\+\) sign to open the Studio Launcher\. | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Running_squid@2x.png)  | Running Terminals, Kernels, and Images\. | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Git_squid@2x.png)  | Git\. | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Commands_squid@2x.png)  | Commands \(Ctrl \+ Shift \+ C\) – The majority of the menu commands are available here\. | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Experiment_list_squid@2x.png)  | SageMaker Experiment List – Displays a list of experiments, trials, or trial components\. Selected when you choose Create Autopilot experiment on the Studio landing page\. | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Notebook_tools_squid@2x.png)  | Notebook Tools\. | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Endpoint_squid@2x.png)  | SageMaker Endpoint List – Selected when you choose Deploy and monitor on the Studio landing page\. | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Open_tabs_squid@2x.png)  | Open Tabs\. | 

## File and resource browser<a name="studio-ui-browser"></a>

The file and resource browser displays lists of your notebooks, experiments, trials, trial components, and endpoints\. On the menu at the top of the file browser, choose the plus \(**\+**\) sign to open the Studio Launcher\. The Launcher allows you to create a notebook, launch a Python interactive shell, or open a terminal\.

## Main work area<a name="studio-ui-work"></a>

The main work area consists of multiple tabs that contain your open notebooks and terminals, and detailed information about your experiments and endpoints\.

## Settings<a name="studio-ui-prefs"></a>

The settings pane allows you to adjust table and chart properties\. By default, the pane is hidden on the far right of the screen\. To open the pane, choose the **Settings** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Settings_squid.png) \) on the top right of the screen\.