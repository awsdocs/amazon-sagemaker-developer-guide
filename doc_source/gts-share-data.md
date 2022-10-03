# Share Data from Your Amazon S3 Bucket<a name="gts-share-data"></a>

After an AWS expert reaches out to discuss your project, you may be required to fill out an intake form with questions specific to your synthetic data requirements\. The intake form, along with assets shared with Ground Truth synthetic data, allows the Ground Truth synthetic data team to evaluate your project and the estimated work required to complete your project\.

Create an Amazon S3 bucket to share your project assets with Ground Truth synthetic data and store your project’s output data\.

**To create an Amazon S3 bucket and share it with us:**

1. Follow the instructions in [Create a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/create-bucket-overview.html) in the *Amazon Simple Storage Service Console User Guide*\.

1. We recommend using the following naming convention while storing your data in an Amazon S3 bucket\.

   1. The *bucket\-name* should contain fewer than 63 characters\.

   1. The *bucket\-name* can include hyphens, but no spaces and underscores\.

1. In the **Buckets** list, choose the name of the bucket you created\.

1. Choose **Permissions**\.

1. In the **Bucket policy** section, choose **Edit**\.

1. Confim that **ACLs disabled** is selected\.
**Note**  
Under **Object Ownership** should have ACLs disabled as shown in the image below\.  
![\[Screenshot of the Amazon S3 console with ACLs disabled.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/gts-share-data.png)

1. Choose **Save changes**\.

**Note**  
If you have additional requirements for accessing your data in an Amazon S3 bucket, please contact your AWS expert\. 

To share your project assets with the Ground Truth synthetic data team for project evaluation, work estimation, and synthetic data generation, follow the steps in the [Send Project Data to Ground Truth Synthetic Data](#gts-transfer-data) section below\.

After receiving your intake form and project assets, we return a statement of work \(SOW\) within 5 business days\. The SOW outlines your engagement with Ground Truth synthetic data generation and labeling\. After you approve the SOW, the Ground Truth synthetic data team produces a test batch consisting of 50 synthetic images\. An AWS expert meets with you to the review the test batch, approve or reject images, and complete the final production\. The timeline for this is based on the responses in your intake form\.

## Send Project Data to Ground Truth Synthetic Data<a name="gts-transfer-data"></a>

After an AWS expert has been assigned to your project, you can send project data to the Ground Truth synthetic data team to assist in project evaluation, work estimation, and synthetic data generation\.

**To send project data to Ground Truth synthetic data:**

1. Under the **Project data transfers** table in the project portal, choose **Send project data**\.

1. Enter the name of your S3 bucket from which you would like to send project data as the Amazon S3 source location of the project data transfer\.

1. Select an IAM role for the project data transfer\. If you select **Automatic**, Ground Truth synthetic data creates an IAM role in your account with the required permissions to run the project data transfer and call other services on your behalf \(recommended\)\. If you select an existing IAM role in your account, Ground Truth synthetic data uses that IAM role to run the project data transfer and call other services on your behalf\.

1. Choose **Create** to create and start the project data transfer\.

After creating a project data transfer, you can view the status of the transfer in the **Project data transfers** table on the project details page in the project portal\. When the project data transfer status is **Completed**, the project data is available to the Ground Truth synthetic data team\.