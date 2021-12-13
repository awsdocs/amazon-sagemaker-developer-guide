# Get notebook differences<a name="studio-lab-use-diff"></a>

You can display the difference between the current notebook and the last checkpoint, or the last Git commit, using the Amazon SageMaker Studio Lab project UI\.

**Topics**
+ [Get the difference between the last checkpoint](#studio-lab-use-diff-checkpoint)
+ [Get the difference between the last commit](#studio-lab-use-diff-git)

## Get the difference between the last checkpoint<a name="studio-lab-use-diff-checkpoint"></a>

When you create a notebook, a hidden checkpoint file that matches the notebook is created\. You can view changes between the notebook and the checkpoint file, or revert the notebook to match the checkpoint file\.

To save the Studio Lab notebook and update the checkpoint file to match: Choose the **Save notebook and create checkpoint** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Notebook_save.png)\)\. This is located on the Studio Lab menu's left side\. The keyboard shortcut for **Save notebook and create checkpoint** is `Ctrl + s`\.

To view changes between the Studio Lab notebook and the checkpoint file: Choose the **Checkpoint diff** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Checkpoint_diff.png)\), located in the center of the Studio Lab menu\.

To revert the Studio Lab notebook to the checkpoint file: On the main Studio Lab menu, choose **File**, and then **Revert Notebook to Checkpoint**\.

## Get the difference between the last commit<a name="studio-lab-use-diff-git"></a>

If a notebook is opened from a Git repository, you can view the difference between the notebook and the last Git commit\.

To view the changes in the notebook from the last Git commit: Choose the **Git diff** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Git_diff.png)\) in the center of the notebook menu\.