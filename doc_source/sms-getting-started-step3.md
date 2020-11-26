# Step 3: Select Workers<a name="sms-getting-started-step3"></a>

In this step you choose a workforce for labeling your dataset\. You can create your own private workforce or you can use the Amazon Mechanical Turk workforce\. If you create a private workforce in this step you won't be able to import your Amazon Cognito user pool later\. For more information, see [Manage a Private Workforce \(Amazon Cognito\)](sms-workforce-management-private.md)\. Use the Amazon Mechanical Turk workforce for this exercise instead\.

You can create a private workforce to test Amazon SageMaker Ground Truth\. Use email addresses to invite the members of your workforce\.

**To create a private workforce**

1. In the **Workers** section, choose **Private**\.

1. If this is your first time using a private workforce, in the **Email addresses** field, enter up to 100 email addresses\. The addresses must be separated by a comma\. You should include your own email address so that you are part of the workforce and can see data object labeling tasks\.

1. In the **Organization name** field, enter the name of your organization\. This information is used to customize the email sent to invite a person to your private workforce\.

1. In the **Contact email** field enter an email address that members of the workforce use to report problems with the task\.

If you choose to use the Amazon Mechanical Turk workforce to label the dataset, you are charged for labeling tasks completed on the dataset\.

**To use the Amazon Mechanical Turk workforce**

1. In the **Workers** section, choose **Public**\.

1. Choose **The dataset does not contain PII** to acknowledge that the dataset does not contain any personally identifiable information\.

1. Choose **The dataset does not contain adult content\.** to acknowledge that the sample dataset has no adult content\.

1. Review and accept the statement that the dataset will be viewed by the public workforce\.

## Next<a name="step3-next"></a>

[Step 4: Configure the Bounding Box Tool](sms-getting-started-step4.md)