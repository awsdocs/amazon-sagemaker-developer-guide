# View and Compare Amazon SageMaker Experiments, Trials, and Trial Components<a name="experiments-view-compare"></a>

An Amazon SageMaker *experiment* consists of multiple *trials* with a related objective\. A trial consists of one or more *trial components*, such as a data preprocessing job and a training job\.

You use the experiments browser to display a list of these entities\. You can filter the list by entity name, type, and tags\. The entities are presented in a hierarchical view \(that is, experiments > trials > trial components\)\. Double\-click an entity to see other entities that are at a lower level in the hierarchy\. Use the breadcrumbs above the list to move to a higher level in the hierarchy\.

For a tutorial using a SageMaker example notebook, see [Track and Compare Tutorial](experiments-mnist.md)\. For an overview of the Studio user interface, see [Amazon SageMaker Studio UI Overview](studio-ui.md)\.

**Topics**
+ [View Experiments, Trials, and Trial Components](#experiments-view)
+ [Compare Experiments, Trials, and Trial Components](#experiments-compare)

## View Experiments, Trials, and Trial Components<a name="experiments-view"></a>

Amazon SageMaker Studio provides an experiments browser that you can use to view lists of experiments, trials, and trial components\. You can choose one of these entities to view detailed information about the entity or choose multiple entities for comparison\.

**To view experiments, trials, and trial components**

1. In the left sidebar of Studio, choose the **SageMaker Experiment List** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Experiment_list_squid.png)\)\. A list of experiments and their properties is displayed\. The list includes all the SageMaker experiments in your account, including experiments created outside of SageMaker Studio\.
**Note**  
To view all the properties, you might have to expand the width of the experiments browser by dragging the right border\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-view-experiment-list.png)

1. In the experiments list, double\-click an experiment to display a list of the trials in the experiment\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-view-trial-list.png)

1. Double\-click a trial to display a list of the components in the trial\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-view-component-list.png)

1. Double\-click one of the components to open the **Describe Trial Component** tab\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-view-describe-component.png)

1. On the **Describe Trial Component** tab, choose any of the following column headings to see available information about each trial component:
   + **Charts** – Build your own charts\.
   + **Metrics** – Metrics that are logged by a `Tracker` during a trial run\.
   + **Parameters** – Hyperparameter values and instance information\.
   + **Artifacts** – Amazon S3 bucket storage locations for the input dataset and the output model\.
   + **AWS Settings** – Job name, ARN, status, creation time, training time, billable time, instance information, and others\.
   + **Debugger** – A list of debugger rules and any issues found\.
   + **Trial Mappings**

For information about comparing Experiments entities, see [View and Compare Amazon SageMaker Experiments, Trials, and Trial Components](#experiments-view-compare)\.

## Compare Experiments, Trials, and Trial Components<a name="experiments-compare"></a>

You can compare experiments, trials, and trial components by selecting the entities and opening them in the trial components list\. The trial components list is referred to as the Studio Leaderboard\. In the Leaderboard you can do the following:
+ View detailed information about the entities
+ Compare entities
+ Stop a training job
+ Deploy a model

**To compare experiments, trials, and trial components**

1. In the left sidebar of SageMaker Studio, choose the **SageMaker Experiment List** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Experiment_list_squid.png) \)\.

1. In the **Experiments** browser, choose either the experiment or trial list\. For more information, see [View Experiments, Trials, and Trial Components](#experiments-view)\.

1. Choose the experiments or trials that you want to compare, right\-click the selection, and then choose **Open in trial component list**\. The Leaderboard opens and lists the associated Experiments entities as shown in the following screenshot\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-search-components-list.png)

The Leaderboard has a **TABLE PROPERTIES** pane on the right side\. Use the **Settings** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Settings_squid.png) \) to open and close the pane\. You can hide or display properties by category or by individual columns\. When you display a chart, the pane changes to display chart properties\.

For information on searching the Experiments entities, see [Search Experiments Using Amazon SageMaker Studio](experiments-search-studio.md)\.