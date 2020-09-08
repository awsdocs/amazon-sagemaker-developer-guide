# Onboard to Amazon SageMaker Studio<a name="gs-studio-onboard"></a>

**Note**  
Amazon SageMaker Studio is available in the following AWS Regions:  
US East \(Ohio\), us\-east\-2
US East \(N\. Virginia\), us\-east\-1
US West \(N\. Oregon\), us\-west\-2
China \(Beijing\), cn\-north\-1
China \(Ningxia\), cn\-northwest\-1
EU \(Ireland\), eu\-west\-1
To change the Region in the SageMaker console, use the Region selector at the upper\-right corner of the console\.

To use Amazon SageMaker Studio and Amazon SageMaker Studio Notebooks, you must complete the Studio onboarding process using the SageMaker console\.

When onboarding, you can choose to use either AWS Single Sign\-On \(AWS SSO\) or AWS Identity and Access Management \(IAM\) for authentication methods\. When you use IAM authentication, you can choose either the **Quick start** or the **Standard setup** procedure\.

**Note**  
If you onboard using IAM authentication and want to switch to AWS SSO authentication later, you must delete the domain created for you by SageMaker Studio\. Then, you need to manually re\-import all notebooks and other user data that you created\. For more information, see [Delete an Amazon SageMaker Studio Domain](gs-studio-delete-domain.md)\.

The simplest way to create a Amazon SageMaker Studio account is to follow the **Quick start** procedure\.

For more control, including the option of using AWS SSO authentication, use the **Standard setup** procedures\.

**AWS SSO Authentication**

To use AWS SSO authentication with Amazon SageMaker Studio, you must onboard to an AWS SSO organization\.

**Note**  
The SSO organization account must be in the same AWS Region as Amazon SageMaker Studio\.

AWS SSO authentication provides the following benefits over IAM authentication:
+ Members given access to Studio have a unique sign\-in URL that directly opens Studio, and they sign in with their SSO credentials\. When you use IAM authentication, you must sign in through the SageMaker console\.
+ Organizations manage their members in AWS SSO instead of Studio\. You can assign multiple members access to Studio at the same time\. When you use IAM authentication, you must add and manage members manually one at time using the Studio Control Panel\. 

**Topics**
+ [Onboard to Amazon SageMaker Studio Using Quick Start](onboard-quick-start.md)
+ [Onboard to Amazon SageMaker Studio Using AWS SSO](onboard-sso-users.md)
+ [Onboard to Amazon SageMaker Studio Using IAM](onboard-iam.md)
+ [Delete an Amazon SageMaker Studio Domain](gs-studio-delete-domain.md)