# Set Up IAM Identity Center for use with Amazon SageMaker Domain<a name="onboard-sso-setup"></a>

To use authentication in IAM Identity Center, you must belong to an AWS Organizations\. If you don't belong to an AWS Organizations, you can create one following the steps in [Tutorial: Creating and configuring an organization](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_tutorials_basic.html)\.

After you have created your organization and user, you can create a SageMaker Studio user profile for that user in IAM Identity Center as follows\.

1. **From the Amazon SageMaker Console:** – You can use the Amazon SageMaker Console to create a user profile for the user in IAM Identity Center\. If the user in IAM Identity Center hasn’t already been associated with Studio, it is automatically associated\.

1. **Using the AWS CLI or AWS CloudFormation** – A user in IAM Identity Center assigned to Studio can create a Studio user profile using the SageMaker console, AWS CLI or AWS CloudFormation\.
   + The user in IAM Identity Center, or a group in IAM Identity Center containing that user, must first be assigned to the Studio application from the IAM Identity Center Console\. For more information about application assignment, see [Assign user access](https://docs.aws.amazon.com/singlesignon/latest/userguide/assignuserstoapp.html)\.
   + A user profile can then be created for the user in IAM Identity Center with the AWS CLI or AWS CloudFormation\.

**Note**  
To simplify administration of access permissions, we recommend assigning groups in IAM Identity Center to the SageMaker Studio application instead of assigning users in IAM Identity Center\. Groups allow permissions to be granted or denied to multiple users at once\. A user can be moved out of a group or to a different group if needed\. When assigning user access to applications, IAM Identity Center does not currently support users being added to nested groups\. If a user is added to a nested group, they may receive a "You do not have any applications" error message during sign\-in\. Assignments must be made to the immediate group the user is a member of\. 

Return to the **Control Panel** to continue to onboard using authentication using IAM Identity Center\.