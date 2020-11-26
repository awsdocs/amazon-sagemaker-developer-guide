# Create a Notebook Instance with an Associated Git Repository \(CLI\)<a name="nbi-git-create-cli"></a>

To create a notebook instance and associate Git repositories by using the AWS CLI, use the `create-notebook-instance` command as follows:
+ Specify the repository that you want to use as your default repository as the value of the `default-code-repository` argument\. Amazon SageMaker clones this repository as a subdirectory in the Jupyter startup directory at `/home/ec2-user/SageMaker`\. When you open your notebook instance, it opens in this repository\. To use a repository that is stored as a resource in your SageMaker account, specify the name of the repository as the value of the `default-code-repository` argument\. To use a repository that is not stored in your account, specify the URL of the repository as the value of the `default-code-repository` argument\.
+ Specify up to three additional repositories as the value of the `additional-code-repositories` argument\. SageMaker clones this repository as a subdirectory in the Jupyter startup directory at `/home/ec2-user/SageMaker`, and the repository is excluded from the default repository by adding it to the `.git/info/exclude` directory of the default repository\. To use repositories that are stored as resources in your SageMaker account, specify the names of the repositories as the value of the `additional-code-repositories` argument\. To use repositories that are not stored in your account, specify the URLs of the repositories as the value of the `additional-code-repositories` argument\.

For example, the following command creates a notebook instance that has a repository named `MyGitRepo`, that is stored as a resource in your SageMaker account, as a default repository, and an additional repository that is hosted on GitHub:

```
aws sagemaker create-notebook-instance \
                    --notebook-instance-name "MyNotebookInstance" \
                    --instance-type "ml.t2.medium" \
                    --role-arn "arn:aws:iam::012345678901:role/service-role/AmazonSageMaker-ExecutionRole-20181129T121390" \
                    --default-code-repository "MyGitRepo" \
                    --additional-code-repositories "https://github.com/myprofile/my-other-repo"
```

**Note**  
If you use an AWS CodeCommit repository that does not contain "SageMaker" in its name, add the `codecommit:GitPull` and `codecommit:GitPush` permissions to the role that you pass as the `role-arn` argument to the `create-notebook-instance` command\. For information about how to add permissions to a role, see [Adding and Removing IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html) in the *AWS Identity and Access Management User Guide*\. 