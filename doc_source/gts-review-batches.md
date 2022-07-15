# Review Batches<a name="gts-review-batches"></a>

Every Amazon SageMaker Ground Truth synthetic data project consists of one or more batches\. Each batch is made up of labeled synthetic images\. Batches are of two types, **Test Batch** and **Production Batch**\. A test batch provides a small preview of how the synthetic images look using your 3D assets and environment\. Images in the test batch are not counted towards the total number of synthetic images you contract\. After you approve the test batch for a specific configuration of images, Ground Truth synthetic data starts generating images for your production batch\. Images in a production batch are counted towards the total required images\. 

![\[Screenshot of the Ground Truth synthetic data batches.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/gts-batches.png)

For every batch, Ground Truth synthetic data provides a **Synthetic Image Fidelity and Diversity Report**\. This report provides image and object level statistics and plots that help you make sense of the generated synthetic images\. The statistics are used to describe the diversity and the fidelity of the synthetic images and compare with real images\. Examples of the statistics and plots provided are the distributions of object classes, object sizes, image brightness, image contrast, as well as the plots evaluating the indistinguishability between synthetic and real images\. The raw data for all the computed dataset statistics is also provided as CSV files to help you accelerate model debugging and enable further analyses\. 

![\[The histogram of image brightness from the synthetic image fidelity and diversity report\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/gts-report.png)

You can view all the batches for your project using the project portal\.

You can use the Ground Truth synthetic data project portal to track the following details about every batch:

**Batch name**: Each batch is identified with a unique batch name\.

**Status**: A Ground Truth synthetic data batch has one of the following status types:

1. **In progress**: We are currently generating labeled images for this batch\. It will soon be ready for your review\.

1. **Ready for review**: A batch of labeled synthetic images is now ready for your review\. The images and their manifest files are also uploaded to your S3 bucket\.

1. **Accepted**: You have accepted this batch\.

1. **Rejected**: You have rejected this batch and it needs to be reworked\. When you reject a batch, an AWS expert contacts you to discuss this further\.

**Batch type**: A batch can either be a test batch or a production batch\.

**Creation date**: Date when the batch was created\.

**Images**: Total number of images in the batch\.