# Amazon SageMaker Studio UI Overview<a name="gs-studio-ui"></a>

Amazon SageMaker Studio extends the JupyterLab interface\. Previous users of JupyterLab will notice the similarity of the user interface, including the workspace\. Studio adds many additions to the interface\. The most prominent additions are detailed in the following sections\. For an overview of the basic JupyterLab interface, see [The JupyterLab Interface](https://jupyterlab.readthedocs.io/en/latest/user/interface.html)\.

The following image shows the initial screen when you sign\-on to Amazon SageMaker Studio\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-landing.png)

At the top of the screen is the *menu bar*\. At the left of the screen is the *left sidebar* which contains icons for different file and resource browsers\. At the right of the screen is the *right sidebar*, represented by the gear icon, which displays contextual preference settings\. At the bottom of the screen is the *status bar*\.

The main work area is divided horizontally into two panes\. The left pane is the *file and resource browser*\. The right pane contains one or more tabs for resources such as notebooks, terminals, metrics, and graphs\.

**Topics**
+ [Left Sidebar](#studio-ui-nav-bar)
+ [SageMaker Experiment List](#studio-ui-experiments)
+ [SageMaker Endpoint List](#studio-ui-endpoint)
+ [Using Notebooks](#studio-ui-notebook)
+ [View a Trial's Properties and Components](#studio-ui-trials)

## Left Sidebar<a name="studio-ui-nav-bar"></a>

The left sidebar includes the following icons\. When you hover over an icon, a tooltip displays the icon name\. When you select an icon, the file and resource browser displays the described functionality\. For hierarchical entries, a selectable breadcrumb at the top of the browser shows your location\.


| Icon | Description | 
| --- | --- | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/File_browser_squid@2x.png)  | File browser\. | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Running_squid@2x.png)  | Running terminals and kernels\. | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Git_squid@2x.png)  | Git\. | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Commands_squid@2x.png)  | Commands \(Ctrl \+ Shift \+ C\)\. The majority of the menu commands are available here\. | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Experiment_list_squid@2x.png)  | SageMaker Experiment List\. Displays a list of experiments, trials, or trial components\. Includes a button to create an Autopilot experiment\. | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Endpoint_squid@2x.png)  | SageMaker Endpoint List\. Selected when you choose Deploy and monitor models on the Studio landing page\. | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Open_tabs_squid@2x.png)  | Open tabs | 

## SageMaker Experiment List<a name="studio-ui-experiments"></a>

Displays a list of experiments, trials, or trial components, and allows you to create an Amazon SageMaker Autopilot experiment\. The following primary actions are available:
+ To create an Autopilot experiment, choose **Create Experiment**\.
+ To display the list of trials that are part of an experiment, double\-click the experiment\.
+ To display the list of trial components that make up a trial, double\-click the trial\.
+ To open a tab in the work area that describes a component, double\-click the component\.
+ To compare experiments or trials, multi\-select the items, right\-click one of the selections, and then choose **Open in trial component list**\.

A selectable breadcrumb above the header shows your position in the hierarchy and allows you to navigate to a higher level\.

## SageMaker Endpoint List<a name="studio-ui-endpoint"></a>

To open a tab that shows a description of an endpoint, double click the endpoint\.

## Using Notebooks<a name="studio-ui-notebook"></a>

To open a notebook, follow these steps:

1. In the left sidebar, select the **File Browser** icon\.

1. At the top of the file browser, select the **Up arrow** icon, and then a **File Upload** dialog opens\. Browse to and select the notebook and any accompanying files, and then choose **Open**\.

1. Double\-click the uploaded notebook file to open the notebook in a new tab\.

## View a Trial's Properties and Components<a name="studio-ui-trials"></a>

To display the list of trials that are part of an experiment, choose the **SageMaker Experiment List** icon and then double\-click the experiment\.

There are two ways to view a trial's properties and components\. Each method displays similar information in different formats, as well as unique information as noted in each item below\.
+ Right click one of the trials and then choose **Open in trial component list**\. A new tab opens that displays a list of the trial's components\.

  Use this view to deploy a model\.
+ Double\-click one of the trials and a list of the trial's components is displayed\. Double\-click one of the components in the list and a new tab opens that describes each component\.