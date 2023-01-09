# Create a Batch<a name="gtp-create-batches"></a>

You can use the project portal to create batches for a project after the project status is changed to **Request approved**\.

![\[The intake form to create a batch using Amazon SageMaker Ground Truth Plus.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/gtp-create-batch.png)

To create a batch, do the following\.

1. Select a project by choosing the project name\.

1. A page titled with the project name opens\. Under the **Batches** section, choose **Create batch**\.

1. Enter the **Batch name**, **Batch description**, **S3 location for input datasets**, and **S3 location for output datasets**\.

1. Choose **Submit**\.

**To create a batch successfully, make sure you meet the following criteria:**
+ Your data is in the US East \(N\. Virginia\) Region\.
+ The maximum size for each file is no more than 2 gigabytes\.
+ The maximum number of files in a batch is 10,000\.
+ The total size of a batch is less than 100 gigabytes\.
+ You have no more than 5 batches with the **Data transfer in\-progress** status\.

**Note**  
You cannot create a batch before the project status changes to **Request approved**\.