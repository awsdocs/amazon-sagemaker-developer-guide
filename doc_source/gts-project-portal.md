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