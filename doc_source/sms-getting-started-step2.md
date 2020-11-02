# Step 2: Create a Labeling Job<a name="sms-getting-started-step2"></a>

In this step you use the console to create a labeling job\. You tell Amazon SageMaker Ground Truth the Amazon S3 bucket where the manifest file is stored and configure the parameters for the job\. For more information about storing data in an Amazon S3 bucket, see [Use Input and Output Data](sms-data.md)\.

**To create a labeling job**

1. Open the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. From the left navigation, choose **Labeling jobs**\.

1. Choose **Create labeling job** to start the job creation process\.

1. In the **Job overview** section, provide the following information:
   + **Job name** – Give the labeling job a name that describes the job\. This name is shown in your job list\. The name must be unique in your account in an AWS Region\.
   + **Label attribute name** – Leave this unchecked as the default value is the best option for this introductory job\.
   + **Input dataset location** – Enter the S3 location of the manifest file that you created in step 1\.
   + **Output dataset location** – The location where your output data is written\.
   + **IAM role** – Create or choose an IAM role with the SageMakerFullAccess IAM policy attached\.

1. In the **Task type** section, for the **Dataset type** field, choose **Bounding box** as the task type\.

1. Choose **Next** to move on to configuring your labeling job\.

## Next<a name="step2-next"></a>

[Step 3: Select Workers](sms-getting-started-step3.md)