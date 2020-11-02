# Add a Git Repository to Your Amazon SageMaker Account \(CLI\)<a name="nbi-git-resource-cli"></a>

Use the `create-code-repository` AWS CLI command\. Specify a name for the repository as the value of the `code-repository-name` argument\. The name must be 1 to 63 characters\. Valid characters are a\-z, A\-Z, 0\-9, and \- \(hyphen\)\. Also specify the following:
+ The default branch
+ The URL of the Git repository
**Note**  
Do not provide a user name in the URL\. Add the username and password in AWS Secrets Manager as described in the next step\.
+ The Amazon Resource Name \(ARN\) of an AWS Secrets Manager secret that contains the credentials to use to authenticate the repository as the value of the `git-config` argument

For information about creating and storing a secret, see [Creating a Basic Secret](https://docs.aws.amazon.com/secretsmanager/latest/userguide/manage_create-basic-secret.html) in the *AWS Secrets Manager User Guide*\. The following command creates a new repository named `MyRespository` in your Amazon SageMaker account that points to a Git repository hosted at `https://github.com/myprofile/my-repo"`\.

For Linux, OS X, or Unix:

```
aws sagemaker create-code-repository \
                    --code-repository-name "MyRepository" \
                    --git-config '{"Branch":"master", "RepositoryUrl" :
                    "https://github.com/myprofile/my-repo", "SecretArn" : "arn:aws:secretsmanager:us-east-2:012345678901:secret:my-secret-ABc0DE"}'
```

For Windows:

```
aws sagemaker create-code-repository ^
                    --code-repository-name "MyRepository" ^
                    --git-config "{\"Branch\":\"master\", \"RepositoryUrl\" :
                    \"https://github.com/myprofile/my-repo\", \"SecretArn\" : \"arn:aws:secretsmanager:us-east-2:012345678901:secret:my-secret-ABc0DE\"}"
```

**Note**  
The secret must have a staging label of `AWSCURRENT` and must be in the following format:  
`{"username": UserName, "password": Password}`  
For GitHub repositories, we recommend using a personal access token instead of your account password\.