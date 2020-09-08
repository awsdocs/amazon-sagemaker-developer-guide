# Managing Vendor Workforces<a name="sms-workforce-management-vendor"></a>

You can use a vendor\-managed workforce to label your data using Amazon SageMaker Ground Truth \(Ground Truth\) and Amazon Augmented AI \(Amazon A2I\)\. Vendors have extensive experience in providing data labeling services for the purpose of machine learning\. Vendor workforces for these two services must be created and managed seperately through the Amazon SageMaker console\. 

Vendors make their services available via the AWS Marketplace\. You can find details of the vendor's services on their detail page, such as the number of workers and the hours that they work\. You can use these details to make estimates of how much the labeling job will cost and the amount of time that you can expect the job to take\. Once you have chosen a vendor you subscribe to their services using the AWS Marketplace\.

A subscription is an agreement between you and the vendor\. The agreement spells out the details of the agreement, such as price, schedule, or refund policy\. You work directly with the vendor if there are any issues with your labeling job\.

You can subscribe to any number of vendors to meet your data annotation needs\. When you create a labeling job or human review worklow you can specify that the job be routed to a specific vendor\.

Before you send sensitive data to a vendor, check the vendor's security practices on their detail page and review the end user license agreement \(EULA\) that is part of your subscription agreement\. 

You must use the console to subscribe to a vendor workforce\. Once you have a subscription, you can use the [ `ListSubscribedWorkteams`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ListSubscribedWorkteams.html) operation to list your subscribed vendors\.

**To subscribe to a vendor workforce**

1. Open the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. Choose the appropriate page in the SageMaker console\.
   + For Ground Truth labeling jobs, choose **Labeling workforces**, choose **Vendor**, and then choose **Find data labeling services**\.
   + For Amazon A2I human review workflows, choose **Human review workforces**, choose **Vendor**, and then choose **Find human review services**\. 

1. The console opens the AWS Marketplace with:
   + data labeling services category selected for Ground Truth
   + human review services category selected for Amazon A2I

   Here you see a list of the vendor services available for this service\. 

1. Choose a vendor\. The AWS Marketplace shows detailed information about the data labeling or human review service\. Use this information to determine if the vendor meets your requirements for your task\.

1. If the vendor meets your requirements, choose **Continue to subscribe**\.

1. Review the details of the subscription\. If you agree to the terms, choose **Subscribe** to complete your subscription to the service\.