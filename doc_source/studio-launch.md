# Launch Amazon SageMaker Studio<a name="studio-launch"></a>

After you have onboarded to an Amazon SageMaker Domain, you can launch an Amazon SageMaker Studio application from either the SageMaker console or the AWS CLI\. For more information about onboarding to a Domain, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.

**Topics**
+ [Launch Studio Using the Amazon SageMaker Console](#studio-launch-console)
+ [Launch Studio Using the AWS CLI](#studio-launch-cli)

## Launch Studio Using the Amazon SageMaker Console<a name="studio-launch-console"></a>

When launching an Amazon SageMaker Studio application from the SageMaker console, you can use the Studio landing page or the Amazon SageMaker Domain details page\. The following sections demonstrate how to launch the Studio application from the SageMaker console\.

**Topics**
+ [Prerequisite](#studio-launch-console-prerequisites)
+ [Launch Studio from the Domain details page](#studio-launch-console-domain)
+ [Launch Studio from the Studio landing page](#studio-launch-console-domain)

### Prerequisite<a name="studio-launch-console-prerequisites"></a>

 To complete this procedure, you must onboard to a Domain by following the steps in [Onboard to Amazon SageMaker Domain](https://docs.aws.amazon.com/sagemaker/latest/dg/gs-studio-onboard.html)\. 

### Launch Studio from the Domain details page<a name="studio-launch-console-domain"></a>

The following sections describe how to launch a Studio application from the Domain details page\. The steps to launch the Studio application after you have navigated to the Domain details page differ depending on if you’re launching a personal application or a shared space\. 

 **Navigate to the Domain details page** 

 The following procedure shows how to navigate to the Domain details page\. 

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1.  On the left navigation pane, choose **Domains**\. 

1. From the list of Domain, select the Domain that you want to launch the Studio application in\.

 **Launch a user profile app** 

 The following procedure shows how to launch a Studio application that is scoped to a user profile\. 

1.  On the Domain details page, choose the **User profiles** tab\. 

1.  Identify the user profile that you want to launch the Studio application for\. 

1.  Choose **Launch** for your selected user profile, then choose **Studio**\. 

 **Launch a shared space app** 

 The following procedure shows how to launch a Studio application that is scoped to a shared space\. 

1.  On the Domain details page, choose the **Space management** tab\. 

1.  Identify the shared space that you want to launch the Studio application for\. 

1.  Choose **Launch Studio** for your selected shared space\. 

### Launch Studio from the Studio landing page<a name="studio-launch-console-domain"></a>

 The following procedure describes how to launch a Studio application from the Studio landing page\. 

 **Launch Studio** 

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. On the left navigation pane, choose Studio\.

1.  Under **Get started**, select the Domain that you want to launch the Studio application in\. If your user profile only belongs to one Domain, you do not see the option for selecting a Domain\.

1.  Select the user profile that you want to launch the Studio application for\. If there is no user profile in the Domain, choose **Create user profile**\. For more information, see [Add and Remove User Profiles](domain-user-profile-add-remove.md)\.

1.  Choose **Launch Studio**\. If the user profile belongs to a shared space, choose **Open Spaces**\. 

1.  To launch a Studio application scoped to a user profile, choose **Launch personal Studio**\. 

1.  To launch a shared Studio application, choose the **Launch shared Studio** button next to the shared space that you want to launch into\. 

## Launch Studio Using the AWS CLI<a name="studio-launch-cli"></a>

You can use the AWS Command Line Interface \(AWS CLI\) to launch Amazon SageMaker Studio by creating a presigned Domain URL\.

 **Prerequisites** 

 Before you begin, complete the following prerequisites: 
+  Onboard to Amazon SageMaker Domain\. For more information, see [Onboard to Amazon SageMaker Domain](https://docs.aws.amazon.com/sagemaker/latest/dg/gs-studio-onboard.html)\. 
+  Update the AWS CLI by following the steps in [Installing the current AWS CLI Version](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv1.html#install-tool-bundled)\. 
+  From your local machine, run `aws configure` and provide your AWS credentials\. For information about AWS credentials, see [Understanding and getting your AWS credentials](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html)\. 

The following code snippet demonstrates how to launch Amazon SageMaker Studio from the AWS CLI using a presigned Domain URL\. For more information, see [create\-presigned\-domain\-url](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/sagemaker/create-presigned-domain-url.html)\.

```
aws sagemaker create-presigned-domain-url \
--region region \
--domain-id domain-id \
--space-name space-name \
--user-profile-name user-profile-name \
--session-expiration-duration-in-seconds 43200
```