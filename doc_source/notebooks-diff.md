# Get Notebook Differences<a name="notebooks-diff"></a>

You can display the difference between the current notebook and the last checkpoint or the last Git commit using the Amazon SageMaker UI\.

The following screenshot shows the menu from a Studio notebook\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-notebooks-manage-callouts.png)

**Topics**
+ [Get the Difference Between the Last Checkpoint](#notebooks-diff-checkpoint)
+ [Get the Difference Between the Last Commit](#notebooks-diff-git)

## Get the Difference Between the Last Checkpoint<a name="notebooks-diff-checkpoint"></a>

When you create a notebook, a hidden checkpoint file that matches the notebook is created\. You can view changes between the notebook and the checkpoint file or revert the notebook to match the checkpoint file\.

By default, a notebook is auto\-saved every 120 seconds and also when you close the notebook\. However, the checkpoint file isn't updated to match the notebook\. To save the notebook and update the checkpoint file to match, you must choose the **Save notebook and create checkpoint** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Notebook_save.png)\) on the left of the notebook menu or use the `Ctrl + S` keyboard shortcut\.

To view the changes between the notebook and the checkpoint file, choose the **Checkpoint diff** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Checkpoint_diff.png)\) in the center of the notebook menu\.

To revert the notebook to the checkpoint file, from the main Studio menu, choose **File** then **Revert Notebook to Checkpoint**\.

## Get the Difference Between the Last Commit<a name="notebooks-diff-git"></a>

If a notebook is opened from a Git repository, you can view the difference between the notebook and the last Git commit\.

To view the changes in the notebook from the last Git commit, choose the **Git diff** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Git_diff.png)\) in the center of the notebook menu\.