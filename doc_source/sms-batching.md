# Batches for Labeling Tasks<a name="sms-batching"></a>

Amazon SageMaker Ground Truth sends data objects to your workers in batches\. There are one or more tasks for each data object\. For each task a worker annotates one of your data objects\. A batch provides the following:
+ It sets the number of data objects that are available to workers\. Once the objects are annotated another batch is sent\.
+ It breaks the work into smaller chunks to avoid overloading your workforce\.
+ It provides chunks of data for the iterative training of automated labeling models\.

Ground Truth first sends a batch of 10 tasks to your workers\. It uses this small batch to set up the labeling job and to make sure that the job is correctly configured\.

After the small batch, Ground Truth sends larger batches to your workers\. You can configure the batch size when you create the job using the [CreateLabelingJob](API_CreateLabelingJob.md)\. When you use the Amazon SageMaker console your job uses 1,000 tasks in each batch\.