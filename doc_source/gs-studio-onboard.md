# Onboard to Amazon SageMaker Studio<a name="gs-studio-onboard"></a>


****  

|  | 
| --- |
| Amazon SageMaker Studio Notebooks is in preview release and is subject to change\. | 

**Note**  
Amazon SageMaker Studio is available only in the following AWS Regions:  
US East \(Ohio\), us\-east\-2
To change the Region in the Amazon SageMaker console, use the Region selector at the top right of the page, next to your AWS account number\.

To use Amazon SageMaker Studio and Amazon SageMaker Studio Notebooks, you must complete the Studio onboarding process using the Amazon SageMaker console\.

When onboarding, you can choose to use either AWS Single Sign\-On \(AWS SSO\) or AWS Identity and Access Management \(IAM\) for authentication methods\. When you use IAM authentication, you can choose either the **Quick start** or the **Standard setup** procedure\.

The simplest way to create an Amazon SageMaker Studio account is to follow the **Quick start** procedure\. For more control, including the option of using AWS SSO authentication, use the **Standard setup** procedures\.

**AWS SSO Authentication**

To use AWS SSO authentication with Amazon SageMaker Studio, you must onboard to an AWS SSO organization\. The SSO organization account must be in an AWS Region supported by Studio\. In addition, an AWS SSO account can't be in multiple Regions\.

AWS SSO authentication provides the following benefits over IAM authentication:
+ Members given access to Studio have a unique sign\-on URL that directly opens Studio, and they sign\-on with their SSO credentials\. When you use IAM authentication, you must sign on through the Amazon SageMaker console\.
+ Organizations manage their members in AWS SSO instead of Studio\. You can assign multiple members access to Studio at the same time\. When you use IAM authentication, you must add and manage members manually one at time using the Studio Control Panel\. 

**Note**  
If you onboard using IAM authentication and want to switch to AWS SSO authentication later, you must delete the domain created for you by Amazon SageMaker Studio\. Then, you need to manually re\-import all notebooks and other user data that you created\. For more information, see [Delete an Amazon SageMaker Studio Domain](gs-studio-delete-domain.md)\.

**Topics**
+ [Onboard to Amazon SageMaker Studio Using Quick Start](onboard-quick-start.md)
+ [Onboard to Amazon SageMaker Studio Using AWS SSO](onboard-sso-users.md)
+ [Onboard to Amazon SageMaker Studio Using IAM](onboard-iam.md)
+ [Delete an Amazon SageMaker Studio Domain](gs-studio-delete-domain.md)