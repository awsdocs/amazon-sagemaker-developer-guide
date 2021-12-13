# Use the Amazon SageMaker Studio Lab notebook toolbar<a name="studio-lab-use-menu"></a>

Amazon SageMaker Studio Lab notebooks extend the JupyterLab interface\. For an overview of the basic JupyterLab interface, see [The JupyterLab Interface](https://jupyterlab.readthedocs.io/en/latest/user/interface.html)\.

The following image shows the toolbar and an empty cell from a Studio Lab notebook\.

![\[The layout of the notebook toolbar, including the toolbar icons.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio-lab-menu.png)

When you hover over a toolbar icon, a tooltip displays the icon function\. You can find additional notebook commands in the Studio Lab main menu\. The toolbar includes the following icons:


| Icon | Description | 
| --- | --- | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/studio-lab-save-and-checkpoint.png)  |  **Save and checkpoint** Saves the notebook and updates the checkpoint file\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/studio-lab-insert-cell.png)  |  **Insert cell** Inserts a code cell below the current cell\. The current cell is noted by the blue vertical marker in the left margin\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/studio-lab_cut_copy_paste.png)  |  **Cut, copy, and paste cells** Cuts, copies, and pastes the selected cells\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/studio-lab-run.png)  |  **Run cells** Runs the selected cells\. The cell that follows the last\-selected cell becomes the new\-selected cell\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/studio-lab-interrupt-kernel.png)  |  **Interrupt kernel** Interrupts the kernel, which cancels the currently\-running operation\. The kernel remains active\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/studio-lab-restart-kernel.png)  |  **Restart kernel** Restarts the kernel\. Variables are reset\. Unsaved information is not affected\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/studio-lab_cell.png)  |  **Cell type** Displays or changes the current cell type\. The cell types are: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/studio-lab-use-menu.html)  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/studio-lab-checkpoint-diff.png)  |  **Checkpoint diff** Opens a new tab that displays the difference between the notebook and the checkpoint file\. For more information, see [Get notebook differences](studio-lab-use-diff.md)\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/studio-lab-git-diff.png)  |  **Git diff** Only enabled if the notebook is opened from a Git repository\. Opens a new tab that displays the difference between the notebook and the last Git commit\. For more information, see [Get notebook differences](studio-lab-use-diff.md)\.  | 
|  **default**  |  **Kernel** Displays or changes the kernel that processes the cells in the notebook\. `No Kernel` indicates that the notebook was opened without specifying a kernel\. You can edit the notebook, but you can't run any cells\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/studio-lab-kernel.png)  |  **Kernel busy status** Displays a kernel's busy status by showing the circle's edge and its interior as the same color\. The kernel is busy when it is starting and when it is processing cells\. Additional kernel states are displayed in the status bar at the bottom\-left corner of Studio Lab\.  | 