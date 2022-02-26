# Onboard to Amazon SageMaker Domain<a name="gs-studio-onboard"></a>

An Amazon SageMaker Domain consists of an associated Amazon Elastic File System \(Amazon EFS\) volume; a list of authorized users; and a variety of security, application, policy, and Amazon Virtual Private Cloud \(Amazon VPC\) configurations\. To use Amazon SageMaker Studio, Amazon SageMaker Studio Notebooks, and RStudio, you must complete the Amazon SageMaker Domain onboarding process using the SageMaker console\. For more information about Amazon SageMaker Domains, see [Amazon SageMaker Machine Learning Environments](domain.md)\.

When onboarding, you can choose to use either AWS Single Sign\-On \(AWS SSO\) or AWS Identity and Access Management \(IAM\) for authentication methods\. When you use IAM authentication, you can choose either the **Quick setup** or the **Standard setup** procedure\. RStudio setup is only available when using the **Standard setup** procedure\.

**Note**  
If you onboard using IAM authentication and want to switch to AWS SSO authentication later, you must delete the Domain that you created\. Then, you need to manually re\-import all notebooks and other user data that you created\. For more information, see [Delete an Amazon SageMaker Domain](gs-studio-delete-domain.md)\.

The simplest way to create a Amazon SageMaker Domain is to follow the **Quick setup** procedure\. Quick setup uses the same default settings as the **Standard setup** procedures\. These settings include shareable notebooks and public internet access\. For more control, including the option of using AWS SSO authentication and RStudio, use the **Standard setup** procedures\.

**AWS SSO Authentication**

To use AWS SSO authentication with Amazon SageMaker Studio and RStudio, you must onboard to an AWS SSO organization\.

**Note**  
The SSO organization account must be in the same AWS Region as Amazon SageMaker Studio and RStudio\.

AWS SSO authentication provides the following benefits over IAM authentication:
+ Members given access to Studio have a unique sign\-in URL that directly opens Studio, and they sign in with their SSO credentials\. When you use IAM authentication, you must sign in through the SageMaker console\.
+ Organizations manage their members in AWS SSO instead of the Domain\. You can assign multiple members access to the Domain at the same time\. When you use IAM authentication, you must add and manage members manually, one at time, using the Domain Control Panel\. 

**Topics**
+ [Onboard to Amazon SageMaker Domain Using Quick setup](onboard-quick-start.md)
+ [Onboard to Amazon SageMaker Domain Using AWS SSO](onboard-sso-users.md)
+ [Onboard to Amazon SageMaker Domain Using IAM](onboard-iam.md)
+ [Choose a VPC](onboard-vpc.md)
+ [Delete an Amazon SageMaker Domain](gs-studio-delete-domain.md)