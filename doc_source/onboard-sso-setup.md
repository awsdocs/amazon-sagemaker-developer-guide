# Set Up AWS SSO for Use with Amazon SageMaker Domain<a name="onboard-sso-setup"></a>

To use AWS SSO authentication, you must belong to an AWS SSO organization\. If you don't belong to an AWS SSO organization, you can create one with the following procedure\.

**To create an AWS SSO organization**

1. Sign in to the AWS Management Console and open the AWS Single Sign\-On console at [https://console\.aws\.amazon\.com/singlesignon/](https://console.aws.amazon.com/singlesignon/)\.

1. Choose **Enable AWS SSO**, and then choose **Create AWS organization**\.

   When your organization has been created, the AWS SSO dashboard opens\. AWS also sends you email to verify the email address associated with the organization\.

**To add a user to an AWS SSO organization**

1. To add a user to your AWS SSO organization, in the navigation pane, choose **Users**\.

1. On the **Users** page, choose **Add user**\. Under **User details**, specify all required fields\. For **Password**, choose **Send an email to the user**\.

1. Choose **Next: Groups** and then **Add user**\. AWS sends an email to the user inviting them to create a password and activate their AWS SSO account\.

1. To add more users, repeat steps 2 through 4\.

After you have created your organization and user, you can create a SageMaker Studio user profile for that AWS SSO user as follows\.

1. **From the Amazon SageMaker Console:** – You can use the Amazon SageMaker Console to create a user profile for the AWS SSO user\. If the AWS SSO user hasn’t already been associated with Studio, it is automatically associated\.

1. **Using the AWS CLI or AWS CloudFormation** – An AWS SSO user assigned to Studio can create a Studio user profile using the SageMaker console, AWS CLI or AWS CloudFormation\.
   + The AWS SSO user, or an AWS SSO group containing that user, must first be assigned to the Studio application from the AWS SSO Console\. For more information about application assignment, see [Assign user access](https://docs.aws.amazon.com/singlesignon/latest/userguide/assignuserstoapp.html)\.
   + A user profile can then be created for the AWS SSO user with the AWS CLI or AWS CloudFormation\.

**Note**  
To simplify administration of access permissions, we recommend assigning AWS SSO groups to the SageMaker Studio application instead of assigning AWS SSO users\. Groups allow permissions to be granted or denied to multiple users at once\. A user can be moved out of a group or to a different group if needed\. When assigning user access to applications, AWS SSO does not currently support users being added to nested groups\. If a user is added to a nested group, they may receive a "You do not have any applications" error message during sign\-in\. Assignments must be made to the immediate group the user is a member of\. 

Return to the **Control Panel** to continue to onboard using AWS SSO authentication\.