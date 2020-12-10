# Create and Use a Data Wrangler Flow<a name="data-wrangler-data-flow"></a>

Use an Amazon SageMaker Data Wrangler flow, or a *data flow*, to create and modify a data preparation pipeline\. The data flow connects the datasets, transformations, and analyses, or *steps*, you create and can be used to define your pipeline\. 

## The Data Flow UI<a name="data-wrangler-data-flow-ui"></a>

When you import a dataset, the original dataset appears on the data flow and is named **Source**\. If you enabled sampling when you imported your data, this dataset is named **Source \- sampled**\. Data Wrangler automatically infers the types of each column in your dataset and creates a new dataframe named **Data types**\. You can select this frame to update the inferred data types\. You will see something similar to the following after you upload a single dataset: 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/dataflow-after-import.png)

Each time you add a transform step, you create a new dataframe\. When multiple transform steps \(other than join or concatenate\) are added to the same dataset, they are stacked\. 

Join and concatenate create stand\-alone steps that contain the new joined or concatenated dataset\. 

The following diagram shows a data flow with a join between two datasets, as well as two stacks of steps\. The first stack \(**Steps \(2\)**\) adds two transforms to the type inferred in the **Data types** dataset\. The *downstream* stack, or the stack to the right, adds transforms to the dataset resulting from a join named **demo\-join**\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/data-flow-steps.png)

The small, gray box in the bottom\-right corner of the data flow provides an overview of number of stacks and steps in the flow and the layout of the flow\. The lighter box inside the gray box indicates the steps that are within the UI view\. You can use this box to see sections of your data flow that fall outside of the UI view\. Use the fit screen icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/updates/fit-screen.png)\) to fill all steps and datasets into your UI view\. 

The bottom left navigation bar includes icons that you can use to zoom in \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/updates/zoom-in.png)\) and out \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/updates/zoom-out.png)\) of your data flow and resize the data flow to fit the screen \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/updates/fit-screen.png)\) \. Use the lock icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/updates/lock-nodes.png)\) to lock and unlock the location of each step on the screen\. 

## Add a Step to Your Data Flow<a name="data-wrangler-data-flow-add-step"></a>

Select the **\+** next to any dataset or previously added step and then select one of the following options:
+ **Edit data types** \(For **Data types** step only\): If you have not added any transforms to a **Data types** step, you can select **Edit data types** to update the data types Data Wrangler inferred when importing your dataset\. 
+ **Add transform**: Adds a new transform step\. See [Transform Data](data-wrangler-transform.md) to learn more about the data transformations you can add\. 
+ **Add analysis**: Adds an analysis\. You can use this option to analyze your data at any point in the data flow\. When you add one or more analyses to a step, an analysis icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/mohave/updates/analysis-icon.png)\) appears on that step\. See [Analyze and Visualize](data-wrangler-analyses.md) to learn more about the analyses you can add\. 
+ **Join**: Joins two datasets and adds the resulting dataset to the data flow\. To learn more, see [Join Datasets](data-wrangler-transform.md#data-wrangler-transform-join)\.
+ **Concatenate**: Concatenates two datasets and adds the resulting dataset to the data flow\. To learn more, see [Concatenate Datasets](data-wrangler-transform.md#data-wrangler-transform-concatenate)\.

## Delete a step from your Data Flow<a name="data-wrangler-data-flow-delete-step"></a>

To delete a step, select the step and select **Delete**\. When you delete a step, all subsequent steps connected to that step, or *downstream steps*, are also deleted\. 

To delete a step from a stack of steps, select the stack and then select the step you want to delete\. 

**Note**  
You cannot delete the **Data type** step directly\. To remove this dataset, you must delete the corresponding **Source** dataset\. 