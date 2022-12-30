# Review Batches<a name="gtp-review-batches"></a>

Every Amazon SageMaker Ground Truth Plus project consists of one or more batches\. Each batch is made up of data objects to be labeled\. You can view all the batches for your project using the project portal as shown in the following image\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/gtb-review-batch.png)

You can use the SageMaker Ground Truth Plus project portal to track the following details about every batch: 

**Batch name**: Each batch is identified with a unique batch name\.

**Status**: A SageMaker Ground Truth Plus batch has one of the following status types:

1. **Request submitted**: You have successfully submitted a new batch\.

1. **Data transfer failed**: Data transfer failed with errors\. Check the error reason and create a new batch after fixing the error\.

1. **Data received**: We have received your unlabeled input data\.

1. **In\-progress**: Data labeling is in progress\.

1. **Ready for review**: Data labeling is completed\. A subset of labeled objects from the batch are ready for you to review\. This is an optional step\.

1. **Review submission in\-progress**: Review feedback is currently being processed\.

1. **Review complete**: You have successfully reviewed the batch\. Next, you have to accept or reject it\. This action can not be undone\.

1. **Accepted**: You have accepted the labeled data and will receive it in your Amazon S3 bucket shortly\.

1. **Rejected**: Labeled data needs to be reworked\.

1. **Sent for rework**: Labeled data is sent for rework\. You can review the batch after its status changes to **Ready for review**\.

1. **Ready for delivery**: Labeled data is ready to be transferred to your Amazon S3 bucket\.

1. **Data delivered**: Object labeling is complete and the labeled data is stored in your Amazon S3 bucket\.

1. **Paused**: Batch is paused at your request\.

**Task type**: SageMaker Ground Truth Plus lets you label five types of tasks that include text, image, video, audio, and point cloud\.

**Batch creation date**: Date when the batch was created\.

**Total objects**: Total number of objects to be labeled across a batch\.

**Completed objects**: Number of labeled objects\.

**Remaining objects**: Number of objects left to be labeled\.

**Failed objects**: Number of objects that cannot be labeled due to an issue with the input data\.

**Objects to review**: Number of objects that are ready for your review\.

**Objects with feedback**: Number of objects that have gotten feedback from the team members\.

SageMaker Ground Truth Plus lets you review a sample set of your labeled data \(determined during the initial consultation call\) through the review UI shown in the following image\.

![\[A screenshot of the project portal used to review batches.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/gtb-review-ui.png)

The portal allows your project team members and you to review a small sample set of the labeled objects for each batch\. You can provide feedback for each labeled object within that subset through this UI\. The review UI allows you to navigate across the subset of labeled objects and provide feedback for those labeled objects\.

You can perform the following actions using the review UI\.
+ Use the arrow controls on the bottom left to navigate through the data objects\.
+ You can provide feedback for each object\. The **Feedback section** is in the right panel\. Choose **Submit** to submit feedback for all images\.
+ Use the image controls in the bottom tray to zoom, pan, and control contrast\.
+ If you plan on returning to finish up your review, choose **Stop and resume later** on the top right\.
+ Choose **Save** to save your progress\. Your progress is also autosaved every 15 minutes\.
+ To exit the review UI, choose **Close** on the upper right corner of the review UI\.
+ You can verify the **Label attributes** and **Frame attributes** on each frame using the panel on the right\. You cannot create new objects or modify existing objects in this task\.