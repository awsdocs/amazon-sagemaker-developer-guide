# Search Experiments Using Amazon SageMaker Studio<a name="experiments-search-studio"></a>

You can search your experiments, trials, and trial components using the experiments browser\. After choosing the entities, you can search on properties of these entities on the Amazon SageMaker Leaderboard\.

**Topics**
+ [Search Experiments, Trials, and Trial Components](#experiments-search-studio-experiments)
+ [Search the SageMaker Studio Leaderboard](#experiments-search-studio-leaderboard)
+ [Search by Tag](#experiments-search-studio-tags)

## Search Experiments, Trials, and Trial Components<a name="experiments-search-studio-experiments"></a>

You can search and create multiple filters to display a subset of experiments, trials, and trial components in the experiments browser\. After you search and filter your list, you can open the entities in the Leaderboard and search on additional properties\.

**To search for experiments, trials, and trial components, and apply filters**

For more information about the following steps, along with screenshots, see [Search the SageMaker Studio Leaderboard](#experiments-search-studio-leaderboard)\.

1. In the experiments browser, navigate to the entity type you want to search for: experiment, trial, or trial component\.

1. Place your cursor in the search box to display the list of column that are searchable\.

1. Choose the column that you want to search on and one of the following dialog boxes opens that's specific to the column you chose:
   + **String** – Enter a text string in the search dialog box that opens\. Enter a string of at least three characters that's part of the component's property value\. This filter limits the list to those components with a property value that contains the text string\.
   + **Discrete values** – Choose one or more property values from the list\. This filter limits the list to those components with a property value that matches at least one of the chosen values\.
   + **Tag** – In **Search tag key**, start entering the tag's key\. A list of tag keys is filtered to only those keys with a name that starts with the text string that you enter\. Choose the tag key that you want to search on from the list\. In **Search tag value**, enter the complete tag value, and then choose **Apply**\.

1. To create the filter, choose **Apply**\.

1. To apply additional filters, repeat the preceding steps\. You can have a maximum of 20 filters\. An entity must match all filters to be displayed\.

## Search the SageMaker Studio Leaderboard<a name="experiments-search-studio-leaderboard"></a>

The Amazon SageMaker Studio trial components list is referred to as the Leaderboard\. You can use the Leaderboard to compare your trials and experiments\. The Leaderboard lists properties of the trials, such as the trial status, debugger status, tags, metrics, hyperparameters, and input and output artifacts\.

You can search the Leaderboard to find specific trial components\. You apply the results of a search to create a filter that displays only those components that satisfy the filter\. You can apply up to 20 filters\. A component is displayed only if it matches all filters\. To remove a filter, choose the **X** that displays to the right of the filter\.

**To search the Leaderboard and apply filters**

1. If the **TABLE PROPERTIES** pane isn't open in SageMaker Studio, choose the **Settings** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Settings_squid.png) \) in the upper\-right corner to open it\.

1. In the **Settings** pane, select the check boxes for the columns that you want to view, and then choose the **Settings** icon to close the pane\.

1. Place your cursor in the **Search column name** box to display the following list of **TABLE PROPERTIES** column names that are searchable:

   **Summary section**
   + Trial component name
   + Trial component type
   + Tags

   **Detail sections** \(all columns\)
   + Metrics
   + Parameters
   + Input artifacts
   + Output artifacts  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-search-column-list.png)

1. To limit the column names when you search the detail sections, start typing the column name\. The list of column names displays only those columns with a name that starts with the text string that you enter\.

1. Choose the column name to search on that column\. 

1. One of the following types of dialog boxes opens that's specific to the column you chose in the previous step:
   + **String** – Enter a text string in the search dialog box that opens\. Enter a string of at least three characters that's part of the component's property value\. This filter limits the list to those components with a property value that contains the text string\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-search-string.png)  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-search-string2.png)
   + **Discrete values** – Choose one or more of the property values from the list\. This filter limits the list to those components with a property value that matches at least one of the chosen values\. For the **Trial component type** column, **Others** matches components with no value in the column\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-search-values.png)  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-search-values2.png)
   + **Tag** – Choose the tag to search\. For more information, see [Search by Tag](#experiments-search-studio-tags)\.
   + **Detail columns** – Specify the conditions that the component's property value must satisfy\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-search-conditions.png)  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-search-conditions2.png)  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-search-conditions3.png)

1. To create the filter, choose **Apply**\.

1. To apply additional filters, repeat the preceding steps\. A component must match all filters to be displayed\. You can have a maximum of 20 filters\.

## Search by Tag<a name="experiments-search-studio-tags"></a>

You can add searchable tags to experiments, trials, and trial components when the entities are created or afterwards using the [AddTags](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AddTags.html) API\. A tag consists of a unique case\-sensitive key and an optional value\. Multiple tags can be added to an entity\. You can add the same tag to multiple entities\.

**To search by tag and apply filters**

1. If the **TABLE PROPERTIES** pane isn't open in SageMaker Studio, choose the **Settings** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Settings_squid.png) \) in the upper\-right corner to open it\.

1. In the **Settings** pane, select **Tags** if it isn't selected, and then choose the **Settings** icon to close the pane\.

1. Place your cursor in the **Search column name** box and then choose **Tags**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-search-tags-1.png)

1. In **Search tag key**, start entering the tag's key\. A list of tag keys displays only those keys with a name that starts with the text string that you enter\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-search-tags-2.png)

1. Choose the tag key that you want to search on from the list\.

1. In **Search tag value**, enter the complete tag value, and then choose **Apply**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-search-tags-3.png)

1. The trial component list displays only those components that have tags that match the key\-value pair you chose\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-search-tags-4.png)

1. To add more tag filters, repeat the previous steps\. A tag must match all filters for the component to be displayed\.