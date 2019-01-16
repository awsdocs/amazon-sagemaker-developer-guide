# Managing Vendor Workforces<a name="sms-workforce-management-vendor"></a>

You can use a vendor\-managed workforce to label your data using Amazon SageMaker Ground Truth\. Vendors have extensive experience in providing data labeling services for the purpose of machine learning\. 

Vendors make their services available via the AWS Marketplace\. You can find details of the vendor's services on their detail page, such as the number of workers and the hours that they work\. You can use these details to make estimates of how much the labeling job will cost and the amount of time that you can expect the job to take\. Once you have chosen a vendor you subscribe to their services using the AWS Marketplace\.

A subscription is an agreement between you and the vendor\. The agreement spells out the details of the agreement, such as price, schedule, or refund policy\. You work directly with the vendor if there are any issues with your labeling job\.

You can subscribe to any number of vendors to meet your data annotation needs\. When you create a labeling job you can specify that the job be routed to a specific vendor\.

Before you send sensitive data to a vendor, check the vendor's security practices on their detail page and review the end user license agreement \(EULA\) that is part of your subscription agreement\. 

You must use the console to subscribe to a vendor workforce\. Once you have a subscription, you can use the [ListSubscribedWorkteams](API_ListSubscribedWorkteams.md) operation to list your subscribed vendors\.

**To subscribe to a vendor workforce**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Labeling workforces**, choose **Vendor**, and then choose **Find data labeling services**\.

1. The console opens the AWS Marketplace with the data labeling services category selected\. You see a list of the data labeling services available\.

1. Choose a vendor\. The AWS Marketplace shows detailed information about the data labeling service\. Use this information to determine if the vendor meets your data labeling requirements\.

1. If the vendor meets your requirements, choose **Continue to subscribe**\.

1. Review the details of the subscription\. If you agree to the terms, choose **Subscribe** to complete your subscription to the service\.