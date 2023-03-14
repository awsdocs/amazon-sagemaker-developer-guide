# Project Portal<a name="gts-project-portal"></a>

Each project consists of one or more batches\. A batch is a collection of similar generated and labeled images\. The project portal provides you access to the projects you have contracted with Ground Truth synthetic data\. You can view the status of your projects and access completed batches along with the synthetic image fidelity and diversity report\. You also review your batches to accept or reject them through the project portal\.

![\[Screenshot of the Ground Truth synthetic data project portal.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/gts-project-portal.png)

You can use the Ground Truth synthetic data project portal to track the following details about your project:

**Project name**: Each project is identified using a unique name\.

**Status**: A Ground Truth synthetic data project has one of the following status types:

1. **Request submitted**: You have successfully submitted the project request form\. Next, an AWS expert schedules a call with you to discuss the details for your project\.

1. **Review in progress**: We are reviewing your project\. An AWS expert has been assigned to your project\.

1. **Production in progress**: We are currently working on generating labeled data for your project\.

1. **Data ready for review**: At least one batch is ready for your review\.

1. **Project complete**: We have completed the generation of the required labeled images\. The images are stored in your Amazon S3 bucket\.

**Batches**: Total number of batches within a project\.

**Project start date**: Starting date of a project\.

**Total images**: Number of images you requested\.

**Completed images**: Number of labeled images generated across all accepted production batches\.

## Delete a Project<a name="gts-project-portal-del"></a>

You can delete a project using the console if the project status is **Request submitted** or **Project complete**\. To delete a project with any other status, contact your AWS expert\. Deleting a Ground Truth synthetic data project does not delete your data from the Amazon S3 buckets and can be subject to charge\.

You can delete a project based on its status as follows:
+ **Request submitted:** Deleting a requested project deletes all the project and customer information from the Ground Truth synthetic data database\.
+ **Review in progress / Production in progress / Data ready for review:** You can request your AWS expert to delete a project having one of these statuses\. Deleting a project deletes all the project and personal information from Ground Truth synthetic data database and S3 buckets\.
+ **Project complete:** Once a project is marked as **Project complete**, we delete all customer information from the Ground Truth synthetic data database and S3 buckets\. You can view the project and batches as long as you want, or delete them using the console\.

**Note**  
 Deleting a project does not delete the images from your S3 bucket\. To learn more about deleting images from your S3 bucket, refer to [Deleting Amazon S3 objects](https://docs.aws.amazon.com/AmazonS3/latest/userguide/DeletingObjects.html)\. 