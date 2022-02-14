# Set Up AWS SSO for Use with Amazon SageMaker Domain<a name="onboard-sso-setup"></a>

To use AWS SSO authentication, you must belong to an AWS SSO organization\. If you don't belong to an AWS SSO organization, you can create one with the following procedure\.

**To create an AWS SSO organization**

1. Sign in to the AWS Management Console and open the AWS Single Sign\-On console at [https://console\.aws\.amazon\.com/singlesignon/](https://console.aws.amazon.com/singlesignon/)\.

1. Choose **Enable AWS SSO**, and then choose **Create AWS organization**\.

   When your organization has been created, the AWS SSO dashboard opens\. AWS also sends you email to verify the email address associated with the organization\.

1. To add a user to your AWS SSO organization, in the navigation pane, choose **Users**\.

1. On the **Users** page, choose **Add user**\. Under **User details**, specify all required fields\. For **Password**, choose **Send an email to the user**\.

1. Choose **Next: Groups** and then **Add user**\. AWS sends an email to the user inviting them to create a password and activate their AWS SSO account\.

1. To add more users, repeat steps 4 through 6\.

Return to the **SageMaker Domain** Control Panel to continue to onboard using AWS SSO authentication\.