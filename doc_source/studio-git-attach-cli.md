# Attach a Git Repository from the AWS CLI<a name="studio-git-attach-cli"></a>

The following topic shows how to attach a Git repository URL using the AWS CLI, so that Amazon SageMaker Studio automatically suggests it for cloning\. After you attach the Git repository URL, you can clone it by following the steps in [Clone a Git Repository in SageMaker Studio](studio-tasks-git.md)\.

## Prerequisites<a name="studio-git-attach-cli-prerequisites"></a>

Before you begin, complete the following prerequisites: 
+ Update the AWS CLI by following the steps in [Installing the current AWS CLI Version](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv1.html#install-tool-bundled)\.
+ From your local machine, run `aws configure` and provide your AWS credentials\. For information about AWS credentials, see [Understanding and getting your AWS credentials](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html)\. 
+ Onboard to Amazon SageMaker Domain\. For more information, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.

## Attach the Git repo to a Domain or user profile<a name="studio-git-attach-cli-attach"></a>

Git repo URLs associated at the Domain level are inherited by all users\. However, Git repo URLs that are associated at the user profile level are scoped to a specific user\. You can attach multiple Git repo URLs to a Domain or user profile by passing a list of repository URLs\.

The following sections show how to attach a Git repo URL to your Domain and user profile\.

### Attach to a Domain<a name="studio-git-attach-cli-attach-domain"></a>

The following command attaches a Git repo URL to an existing Domain\.

```
aws sagemaker update-domain --region region --domain-name domain-name \
    --domain-settings JupyterServerAppSettings={CodeRepositories=[{RepositoryUrl="repository"}]}
```

### Attach to a user profile<a name="studio-git-attach-cli-attach-userprofile"></a>

The following shows how to attach a Git repo URL to an existing user profile\.

```
aws sagemaker update-user-profile --domain-name domain-name --user-profile-name user-name\
    --user-settings JupyterServerAppSettings={CodeRepositories=[{RepositoryUrl="repository"}]}
```