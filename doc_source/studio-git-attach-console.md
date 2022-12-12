# Attach a Git Repository from the SageMaker Console<a name="studio-git-attach-console"></a>

The following topic shows how to associate a Git repository URL from the Amazon SageMaker console to clone it in your Studio environment\. After you associate the Git repository URL, you can clone it by following the steps in [Clone a Git Repository in SageMaker Studio](studio-tasks-git.md)\.

## Prerequisites<a name="studio-git-attach-console-prerequisites"></a>

Before you can begin this tutorial, you must onboard to Amazon SageMaker Domain\. For more information, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.

## Attach the Git repo to a Domain or user profile<a name="studio-git-attach-console-attach"></a>

Git repo URLs associated at the Domain level are inherited by all users\. However, Git repo URL that are associated at the user profile level are scoped to a specific user\. 

The following sections show how to attach a Git repo URL to a Domain and user profile\.

### Attach to a Domain<a name="studio-git-attach-console-attach-domain"></a>

**To attach a Git repo URL to an existing Domain**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. On the left navigation panel, choose **Domains**\.

1. Select the Domain to attach the Git repo to\.

1. On the **Domain details** page, choose the **Environment** tab\.

1. On the **Suggested code repositories for the domain** tab, choose **Attach**\.

1. Under **Source**, enter the Git repository URL\.

1. Select **Attach to domain**\.

### Attach to a user profile<a name="studio-git-attach-console-attach-userprofile"></a>

The following shows how to attach a Git repository URL to an existing user profile\.

**To attach a Git repository URL to a user profile**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. On the left navigation panel, choose **Domains**\.

1. Select the Domain that includes the user profile to attach the Git repo to\.

1. On the **Domain details** page, choose the **User profiles** tab\.

1. Select the user profile to attach the Git repo URL to\.

1. On the **User details** page, choose **Edit**\.

1. On the **Studio settings** page, choose **Attach** from the **Suggested code repositories for the user** section\.

1. Under **Source**, enter the Git repository URL\.

1. Choose **Attach to user**\.