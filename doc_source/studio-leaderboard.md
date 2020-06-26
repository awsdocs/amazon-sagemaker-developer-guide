# Use the Studio Leaderboard to Compare and Evaluate Trials<a name="studio-leaderboard"></a>

The trial components list in Amazon SageMaker Studio is referred to as the Leaderboard\. The Leaderboard is where you compare the trials in your experiments\. The Leaderboard contains multiple variable width columns that list properties of the trials, such as the trial status, debugger status, tags, metrics, hyperparameters, and input and output artifacts\.

The following image shows the SageMaker Studio Leaderboard\. At the top left is the **Search** box\. At the right is the **TABLE PROPERTIES** pane\. Use the **Settings** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Settings_squid.png) \) to open and closed the pane\. You can hide or display properties by category or by individual columns\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-leaderboard.png)

**Topics**
+ [Search for an Experiment, Trial, or Trial Component](#studio-search-experiments)
+ [Search the Trial Components List](#studio-search-trial-components)
+ [View Trial Component Details](#studio-trial-details)

## Search for an Experiment, Trial, or Trial Component<a name="studio-search-experiments"></a>

In the Experiments browser, experiments, trials, and trial components are accessed as a hierarchy\. Double\-click an entity to drill down the hierarchy\. Use the breadcrumb above the list to go up the hierarchy\.

**To search for an experiment, trial, or trial component by name**

1. In the left sidebar, choose the **SageMaker Experiment List** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Experiment_list_squid.png) \)\.

1. In the Experiments browser, choose the entity type you want to search for, experiment, trial, or trial component\.

1. In the search box, start typing a substring of at least three characters that's part of the entity's name\. As you type, the list of entities will be continuously filtered to only those entities where the substring is part of the entity name\.

## Search the Trial Components List<a name="studio-search-trial-components"></a>

You can add searchable tags to experiments, trials, and trial components when the entities are created or afterwards using the [AddTags](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AddTags.html) API\. A tag consists of a unique case\-sensitive key and an optional value\. Multiple tags can be added to an entity\. The same tag can be added to multiple entities\.

**To search by tag**

1. In the left sidebar, choose the **SageMaker Experiment List** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Experiment_list_squid.png) \)\.

1. In the Experiments browser, choose either the experiments or trials list\.

1. Choose the experiments or trials that you want to compare\. Hold down the CTRL/CMD key and choose each entity individually, or to choose a continuous range of entities, hold down the Shift key and choose the top entity and the bottom entity that define the range\. Right\-click the selection and choose **Open in trial component list**\. A new tab opens that lists the associated trials and trial components\.

1. If the **TABLE PROPERTIES** pane isn't open, choose the **Settings** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Settings_squid.png) \) in the upper right corner to open it\.

1. Verify that the **Tags** column is enabled, then choose the **Settings** icon to close the pane\.

1. In the **Search column name** box, choose the column to search by typing **tags**, then choose **Tags**\.

1. In **Search tag key**, start typing a substring of at least three characters that's part of the tag's key\. A list of tag keys will be displayed and continuously filtered to only those keys where the substring is part of the tag's key\.

1. Choose a tag key\.

1. In **Search tag value**, type a substring of the tag's value then choose **Apply**\.

1. The trial component list will displays only those components that have tags that match the key\-value pair\.

1. Add additional filters by following steps 6 through 9\. A trial component's tag must match all filters for the component to be displayed\. You can have up to 20 filters\.

## View Trial Component Details<a name="studio-trial-details"></a>

You can view the details of a trial component in the **Describe Trial Component** pane\. This pane includes charts, metrics, hyperparameters, input and output artifacts, AWS settings, debugger data, and trial mappings\. AWS settings contains links to Amazon CloudWatch logs\.

**To view the details of a trial component**

1. From the **Trial Components** pane, right\-click a trial component and choose **Open in trial details**\.

1. Choose the various headings to view the associated properties\.

1. To view the links to Amazon CloudWatch logs, choose the **AWS Settings** header and scroll to the bottom of the page\.