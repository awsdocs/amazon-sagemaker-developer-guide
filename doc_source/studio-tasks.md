# Perform Common Tasks in Amazon SageMaker Studio<a name="studio-tasks"></a>

The following sections describe how to perform common tasks in Amazon SageMaker Studio\. For an overview of the Studio interface, see [Amazon SageMaker Studio UI Overview](studio-ui.md)\.

**Topics**
+ [Perform Common Tasks Using the SageMaker Studio Launcher](studio-tasks-launcher.md)
+ [Perform Common Tasks in a SageMaker Studio Notebook](studio-tasks-notebook.md)
+ [Upload files to Amazon SageMaker Studio](#studio-tasks-files)
+ [Clone and Connect to a Git Repository](#studio-tasks-git)
+ [Create an Amazon SageMaker Autopilot Experiment](#studio-tasks-autopilot)
+ [View Experiments, Trials, and Trial Components](#studio-tasks-experiments)
+ [Stop a Training Job](#studio-tasks-stop-training-job)
+ [Manage Your Storage Volume](#studio-tasks-manage-storage)
+ [Provide Feedback on Amazon SageMaker Studio](#studio-tasks-provide-feedback)
+ [Update Amazon SageMaker Studio](#studio-tasks-update)

## Upload files to Amazon SageMaker Studio<a name="studio-tasks-files"></a>

Amazon SageMaker Studio can open only files listed in the Studio file browser\. The following procedure shows how to add files to the browser\.

**To upload files**

1. In the left sidebar, choose the **File Browser** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/File_browser_squid.png)\) to display the file browser\.

1. In the File Browser, choose the **Upload Files** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/File_upload_squid.png)\) to open the dialog\.

1. In **File Upload**, browse to the files, choose the files you want to upload, and then choose **Open**\.

1. Double\-click a file to open the file in a new tab in SageMaker Studio\.

## Clone and Connect to a Git Repository<a name="studio-tasks-git"></a>

For this example, we connect to a local clone of the [awslabs/amazon\-sagemaker\-examples](https://github.com/awslabs/amazon-sagemaker-examples) repository \(repo\)\. Studio can't connect to a remote repo\.

**To connect to a repo**

1. On the top menu, choose **File** > **New** > **Terminal**\.

1. At the command prompt, run the following command and wait for the repo to finish cloning\.

   `git clone https://github.com/awslabs/amazon-sagemaker-examples.git`

1. In the left sidebar, choose the **Git** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Git_squid.png)\)\.

1. Choose **Go find a repo**\.

1. In File Browser, the cloned repo appears in the list\. Double click the repo to open it\.

1. Choose the **Git** icon to view a list of Git tools\.

## Create an Amazon SageMaker Autopilot Experiment<a name="studio-tasks-autopilot"></a>

When you create an Amazon SageMaker Autopilot experiment, Amazon SageMaker analyzes your data and creates a notebook with candidate model definitions\. If you choose to run the complete experiment, Amazon SageMaker trains and tunes these models on your behalf\. You can view statistics while the experiment is running\. Afterwards, you can compare trials and delve into the details\.

**To create an Autopilot experiment**

1. In the upper\-left corner of SageMaker Studio, choose **Amazon SageMaker Studio** to open the Studio landing page\.

1. Under **Build models automatically**, choose **Create Autopilot experiment**\.

1. On the **Create experiment** tab, enter the required information\.

1. Choose **Create Experiment**\.

For more information, see [Use Amazon SageMaker Autopilot to automate model development](autopilot-automate-model-development.md)\.

## View Experiments, Trials, and Trial Components<a name="studio-tasks-experiments"></a>

An experiment consists of multiple trials with a related objective\. A trial consists of one or more trial components, such as a data preprocessing job and a training job\. You can use the Experiments browser to view detailed information about these entities\.

In the Experiments browser, experiments, trials, and trial components are accessed as a hierarchy\. Double\-click an entity to drill down the hierarchy\. Use the breadcrumb above the list to go up the hierarchy\.

**To view an experiment**

1. In the left sidebar, choose the **SageMaker Experiment List** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Experiment_list_squid.png)\)\.

1. In the experiments and trials browser, double\-click an experiment to display the trials in the experiment\.

1. Double\-click a trial to display the components that make up the trial\.

1. Double\-click a component to open the **Describe Trial Component** tab\.

1. Choose the following headers to see available information about the trial component\.
   + **Charts** – Build your own charts
   + **Metrics** – Metrics that are logged by a `Tracker` during a trial run
   + **Parameters** – Hyperparameter values and instance information
   + **Artifacts** – Amazon S3 Storage locations for the input dataset and the output model
   + **AWS Settings** – Job name, ARN, status, creation time, training time, billable time, instance information, and others
   + **Debugger** – A list of debugger rules and any issues found
   + **Trial Mappings**

For more information, including a tutorial, see [Manage Machine Learning with Amazon SageMaker Experiments](experiments.md)\.

## Stop a Training Job<a name="studio-tasks-stop-training-job"></a>

When you stop a training job, its status changes to `Stopping`\.  An algorithm can delay termination in order to save model artifacts after which the job status changes to `Stopped`\. For more information, see the  [StopTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_StopTrainingJob.html) API\.

**To stop a training job**

1. Follow the [View Experiments, Trials, and Trial Components](#studio-tasks-experiments) procedure on this page until you open the **Describe Trial Component** tab\.

1. At the upper\-right side of the tab, choose **Stop training job**\. The **Status** at the top left of the tab changes to **Stopped**\.

1. To view the training time and billing time, choose **AWS Settings**\.

## Manage Your Storage Volume<a name="studio-tasks-manage-storage"></a>

The first time a user on your team opens Amazon SageMaker Studio, Amazon SageMaker creates a domain for the team\. The domain includes an Amazon Elastic File System \(Amazon EFS\) volume with home directories for each of your users\. Notebook files and data files are stored in these directories\. Users can't access each other's home directories\.

**Important**  
Don't delete the Amazon EFS volume\. If you delete it, the domain will no longer function and all of your users will lose their work\.

### Find Your Amazon EFS Storage Volume<a name="studio-tasks-manage-storage-find"></a>

**To find your Amazon EFS volume**

1. From the Amazon SageMaker Studio Control Panel, under **Studio Summary**, find the **Studio ID**\. The ID will be in the following format: `d-xxxxxxxx`\.

1. Pass the `Studio ID`, which is the same as `DomainId`, to the [DescribeDomain](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeDomain.html) API\.

   In the response from `DescribeDomain`, note the value in the `HomeEfsFileSystemId` field\.

1. Open the [Amazon EFS console](https://console.aws.amazon.com/efs/)\.

1. Under **File systems**, scroll through the list to find the `HomeEfsFileSystemId` value, which is the same as the **File system ID**\.

## Provide Feedback on Amazon SageMaker Studio<a name="studio-tasks-provide-feedback"></a>

**To provide feedback**

1. At the upper\-right of SageMaker Studio, choose **Feedback**\.

1. Choose a smiley emoji to let us know how satisfied you are with SageMaker Studio and add any feedback you'd care to share with us\.

1. Decide whether to share your identity with us, then choose **Submit**\.

## Update Amazon SageMaker Studio<a name="studio-tasks-update"></a>

When you update Amazon SageMaker Studio to the latest release, Amazon SageMaker shuts down and restarts the JupyterServer App\. Any unsaved notebook information is lost in the process\. The user data in the Amazon EFS volume isn't touched\.

After the JupyterServer App is restarted, you must reopen Studio through the Amazon SageMaker Studio Control Panel\.

**To update Studio**

1. \(Optional\) Choose **Amazon SageMaker Studio** on the top\-left of Studio to open the landing page\. The Studio version is shown on the bottom\-left of the page\.

1. On the top menu, choose **File** then **Shut Down**\.

1. Choose one of the following options:
   + **Shutdown Server** – Shuts down the JupyterServer App\. Terminal sessions, kernel sessions, SageMaker images, and instances aren't shut down\. These resources continue to accrue charges\.
   + **Shutdown All** – Shuts down all Apps, terminal sessions, kernel sessions, SageMaker images, and instances\. These resources no longer accrue charges\.

1. Reopen Studio\.