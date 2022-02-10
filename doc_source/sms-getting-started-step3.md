# Step 3: Select Workers<a name="sms-getting-started-step3"></a>

In this step you choose a workforce for labeling your dataset\. It is recommended that you create a private workforce to test Amazon SageMaker Ground Truth\. Use email addresses to invite the members of your workforce\. If you create a private workforce in this step you won't be able to import your Amazon Cognito user pool later\. If you want to create a private workforce using an Amazon Cognito user pool, see [Manage a Private Workforce \(Amazon Cognito\)](sms-workforce-management-private.md) and use the Mechanical Turk workforce instead in this tutorial\.

**Tip**  
To learn about the other workforce options you can use with Ground Truth, see [Create and Manage Workforces](sms-workforce-management.md)\. 

**To create a private workforce:**

1. In the **Workers** section, choose **Private**\.

1. If this is your first time using a private workforce, in the **Email addresses** field, enter up to 100 email addresses\. The addresses must be separated by a comma\. You should include your own email address so that you are part of the workforce and can see data object labeling tasks\.

1. In the **Organization name** field, enter the name of your organization\. This information is used to customize the email sent to invite a person to your private workforce\.

1. In the **Contact email** field enter an email address that members of the workforce use to report problems with the task\.

If you add yourself to the private workforce, you will receive an email that looks similar to the following\. **Amazon, Inc\.** is replaced by the organization you enter in step 3 of the preceding procedure\. Select the link in the email to log in using the temporary password provided\. If prompted, change your password\. When you successfully log in, you see the worker portal where your labeling tasks appear\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sms/worker_portal_invite.png)

**Tip**  
You can find the link to your private workforce's worker portal in the **Labeling workforces** section of the Ground Truth area of the SageMaker console\. To see the link, select the **Private** tab\. The link is under the **Labeling portal sign\-in URL** header in **Private workforce summary**\.

If you choose to use the Amazon Mechanical Turk workforce to label the dataset, you are charged for labeling tasks completed on the dataset\.

**To use the Amazon Mechanical Turk workforce:**

1. In the **Workers** section, choose **Public**\.

1. Set a **Price per task**\.

1. If applicable, choose **The dataset does not contain adult content** to acknowledge that the sample dataset has no adult content\. This information enables Amazon SageMaker Ground Truth to warn external workers on Mechanical Turk that they might encounter potentially offensive content in your dataset\.

1. Choose the check box next to the following statement to acknowledge that the sample dataset does not contain any personally identifiable information \(PII\)\. This is a requirement to use Mechanical Turk with Ground Truth\. If your input data does contain PII, use the private workforce for this tutorial\. 

   **You understand and agree that the Amazon Mechanical Turk workforce consists of independent contractors located worldwide and that you should not share confidential information, personal information or protected health information with this workforce\.**

## Next<a name="step3-next"></a>

[Step 4: Configure the Bounding Box Tool](sms-getting-started-step4.md)
