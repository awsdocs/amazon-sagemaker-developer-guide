# Detach Git Repos<a name="studio-git-detach"></a>

This guide shows how to detach Git repository URLs from an Amazon SageMaker Domain or user profile using the AWS CLI or Amazon SageMaker console\.

**Topics**
+ [Detach a Git repo using the AWS CLI](#studio-git-detach-cli)
+ [Detach the Git repo using the SageMaker console](#studio-git-detach-console)

## Detach a Git repo using the AWS CLI<a name="studio-git-detach-cli"></a>

To detach all Git repo URLs from a Domain or user profile, you must pass an empty list of code repositories\. This list is passed as part of the `JupyterServerAppSettings` parameter in an `update-domain` or `update-user-profile` command\. To detach only one Git repo URL, pass the code repositories list without the desired Git repo URL\. This section shows how to detach all Git repo URLs from your Domain or user profile using the AWS Command Line Interface \(AWS CLI\)\.

### Detach from a Domain<a name="studio-git-detach-cli-domain"></a>

The following command detaches all Git repo URLs from a Domain\.

```
aws sagemaker update-domain --region region --domain-name domain-name \
    --domain-settings JupyterServerAppSettings={CodeRepositories=[]}
```

### Detach from a user profile<a name="studio-git-detach-cli-userprofile"></a>

The following command detaches all Git repo URLs from a user profile\.

```
aws sagemaker update-user-profile --domain-name domain-name --user-profile-name user-name\
    --user-settings JupyterServerAppSettings={CodeRepositories=[]}
```

## Detach the Git repo using the SageMaker console<a name="studio-git-detach-console"></a>

The following sections show how to detach a Git repo URL from a Domain or user profile using the SageMaker console\.

### Detach from a Domain<a name="studio-git-detach-console-domain"></a>

Use the following steps to detach a Git repo URL from an existing Domain\.

**To detach a Git repo URL from an existing Domain**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. On the left navigation panel, choose **Domains**\.

1. Select the Domain with the Git repo URL that you want to detach\.

1. On the **Domain details** page, choose the **Environment** tab\.

1. On the **Suggested code repositories for the domain** tab, select the Git repository URL to detach\.

1. Choose **Detach**\.

1. From the new window, choose **Detach**\.

### Detach from a user profile<a name="studio-git-detach-console-userprofile"></a>

Use the following steps to detach a Git repo URL from a user profile\.

**To detach a Git repo URL from a user profile**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. On the left navigation panel, choose **Domains**\.

1. Select the Domain that includes the user profile with the Git repo URL that you want to detach\.

1. On the **Domain details** page, choose the **User profiles** tab\.

1. Select the user profile with the Git repo URL that you want to detach\.

1. On the **User details** page, choose **Edit**\.

1. On the **Studio settings** page, select the Git repo URL to detach from the **Suggested code repositories for the user** tab\.

1. Choose **Detach**\.

1. From the new window, choose **Detach**\.