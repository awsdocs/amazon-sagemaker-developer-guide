# Amazon SageMaker Studio basic tasks<a name="gs-studio-tasks"></a>

The following sections describe how to perform basic tasks in Amazon SageMaker Studio\.

**Topics**
+ [Tasks available from a Studio notebook](studio-tasks-notebook.md)
+ [Upload files to Amazon SageMaker Studio](#studio-tasks-files)
+ [Clone and connect to a Git repository](#studio-tasks-git)
+ [Create an Amazon SageMaker Autopilot experiment](#studio-tasks-autopilot)
+ [View experiments, trials, and trial components](#studio-tasks-experiments)
+ [Stop a training job](#studio-tasks-stop-training-job)

## Upload files to Amazon SageMaker Studio<a name="studio-tasks-files"></a>

Amazon SageMaker Studio can open only files listed in the Studio file browser\. The following procedure shows how to add files to the browser\.

**To upload files**

1. In the left sidebar, choose the **File Browser** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/File_browser_squid.png) \) to display the file browser\.

1. At the top of the file browser, choose the **Upload Files** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/File_upload_squid.png) \) to open the **File Upload** dialog\.

1. Browse to and choose the files, and then choose **Open**\.

1. Double\-click a file to open the file in a new tab in Studio\.

## Clone and connect to a Git repository<a name="studio-tasks-git"></a>

For this example, we connect to a local clone of the [awslabs/amazon\-sagemaker\-examples](https://github.com/awslabs/amazon-sagemaker-examples) repository \(repo\)\. Studio can't connect to a remote repo\.

**To connect to a repo**

1. On the top menu, choose **File** > **New** > **Terminal**\.

1. At the command prompt, run the following command and wait for the repo to finish cloning\.

   `git clone https://github.com/awslabs/amazon-sagemaker-examples.git`

1. In the left sidebar, choose the **Git** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Git_squid.png) \)\.

1. Choose **Go find a repo**\. The file browser is opened showing the cloned repo\.

1. Double click the repo to open it\.

1. Choose the **Git** icon to view a list of Git tools\.

## Create an Amazon SageMaker Autopilot experiment<a name="studio-tasks-autopilot"></a>

When you create an Amazon SageMaker Autopilot experiment, Amazon SageMaker analyzes your data and creates a notebook with candidate model definitions\. If you choose to run the complete experiment, Amazon SageMaker trains and tunes these models on your behalf\. You can view statistics while the experiment is running\. Afterwards, you can compare trials and delve into the details\.

**To create an Autopilot experiment**

1. At the top left of the screen, choose **Amazon SageMaker Studio** to open the Studio landing page\.

1. Under **Build models automatically**, choose **Create Autopilot experiment** to open the **Create experiment** tab\.

1. Fill in the required information\.

1. Choose **Create Experiment**\.

For more information, see [Use Amazon SageMaker Autopilot to Automate Model Development](autopilot-automate-model-development.md)\.

## View experiments, trials, and trial components<a name="studio-tasks-experiments"></a>

An experiment consists of multiple trials with a related objective\. A trial consists of one or more trial components, such as a data preprocessing job and a training job\. You can drill into an experiment and view detailed information about these components\.

**To view an experiment**

1. In the left sidebar, choose the **SageMaker Experiments List** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Experiment_list_squid.png) \) to display the experiments and trials browser\.

1. Double\-click an experiment to display the trials in the experiment\.

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

## Stop a training job<a name="studio-tasks-stop-training-job"></a>

When you stop a training job, its status changes to `Stopping`\.  An algorithm can delay termination in order to save model artifacts after which the job status changes to `Stopped`\.

**To stop a training job**

1. Follow the [View experiments, trials, and trial components](#studio-tasks-experiments) procedure on this page until you open the **Describe Trial Component** tab\.

1. At the top right of the tab, choose **Stop training job**\. The **Status** at the top left of the tab changes to **Stopped**\.

1. To view the training time and billing time, choose the **AWS Settings** header\.