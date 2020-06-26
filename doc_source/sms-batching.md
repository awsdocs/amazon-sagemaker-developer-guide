# Batches for Labeling Tasks<a name="sms-batching"></a>

Amazon SageMaker Ground Truth sends data objects to your workers in batches\. There are one or more tasks for each data object\. For each task, a worker annotates one of your data objects\. A batch does the following:
+ Sets the number of data objects that are available to workers\. After the objects are annotated, another batch is sent\.
+ Breaks the work into smaller chunks to avoid overloading your workforce\.
+ Provides chunks of data for the iterative training of automated data labeling models\.

Ground Truth first sends a batch of 10 tasks to your workers\. It uses this small batch to set up the labeling job and to make sure that the job is correctly configured\.

Ground Truth then sends larger batches to your workers\. 

You configure batch size when you create the job using the [ `CreateLabelingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateLabelingJob.html) operation\. If you use the Amazon SageMaker console to create the labeling job, Ground Truth automatically configures your job to use 1,000 tasks in each batch\.