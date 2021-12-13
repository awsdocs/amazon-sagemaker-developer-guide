# Join data that you've imported into SageMaker Canvas<a name="canvas-joining-data"></a>

You can use Amazon SageMaker Canvas to join multiple datasets into a single dataset\. A join combines the two datasets\. By default, SageMaker Canvas automatically joins the datasets on their matching column names\. The option to combine multiple datasets might give you the ability to get more insight from the models that you build\.

You can make the following joins for your datasets:
+ **Inner** – Returns a dataset with matching values in both datasets\.
+ **Left** – Returns a dataset that has:
  + All the rows from the dataset to the left of the join\.
  + All the rows from the dataset to the right of the join that have matching values with the columns to the left of the join\.
+ **Right** – Returns a dataset that has:
  + All the rows from the dataset to the right of the join\.
  + All the rows from the dataset to the left of the join that have matching values with the columns to the right of the join\.
+ **Outer** – Returns all the rows when there is a match in either the left or the right dataset\. The dataset from an outer join might have null values that SageMaker Canvas might impute when you build a model\.

Use the following procedure to join your datasets\.

To join datasets, do the following\.

1. Navigate to the **Datasets** page\.

1. Choose **Join data**\.

1. Drag and drop the datasets that you're joining into the **Drag and drop datasets to join** box\.

1. Configure the join\. Amazon SageMaker Canvas shows you a preview of the joined data after you configure it\.

1. Choose **Save joined data** to save the output of the join\.

The following images show the workflow of the preceding procedure\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-drag-drop-join-datasets.png)

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-join-datasets.png)

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-join-add-key.png)

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/canvas/canvas-join-on-key.png)