# Accept or Reject Batches<a name="gtp-accept-reject-batch"></a>

After you have reviewed a batch, you must choose to accept or reject it\.

If you accept a batch, the output from that labeling job is placed in the Amazon S3 bucket that you specify\. Once the data is delivered to your S3 bucket, the status of your batch changes from **Accepted** to **Data delivered**\.

If you reject a batch, you can provide feedback and explain your reasons for rejecting the batch\.

Ground Truth Plus allows you to provide feedback at the data object level as well as the batch level\. You can provide feedback for data objects through the review UI\. You can use the project portal to provide feedback for each batch\. When you reject a batch, an AWS expert contacts you to determine the rework process and the next steps for the batch\. 

**Note**  
 Accepting or rejecting a batch is a one\-time action and cannot be undone\. It is necessary to either accept or reject every batch of the project\. 