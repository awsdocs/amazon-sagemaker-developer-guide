# Associate a CodeCommit Repository in a Different AWS Account with a Notebook Instance<a name="nbi-git-cross"></a>

To associate a CodeCommit repository in a different AWS account with your notebook instance, set up cross\-account access for the CodeCommit repository\.

**To set up cross\-account access for a CodeCommit repository and associate it with a notebook instance:**

1. In the AWS account that contains the CodeCommit repository, create an IAM policy that allows access to the repository from users in the account that contains your notebook instance\. For information, see [Step 1: Create a Policy for Repository Access in AccountA](https://docs.aws.amazon.com/codecommit/latest/userguide/cross-account-administrator-a.html#cross-account-create-policy-a) in the *CodeCommit User Guide*\.

1. In the AWS account that contains the CodeCommit repository, create an IAM role, and attach the policy that you created in the previous step to that role\. For information, see [Step 2: Create a Role for Repository Access in AccountA](https://docs.aws.amazon.com/codecommit/latest/userguide/cross-account-administrator-a.html#cross-account-create-role-a) in the *CodeCommit User Guide*\.

1. Create a profile in the notebook instance that uses the role that you created in the previous step:

   1. Open the notebook instance\.

   1. Open a terminal in the notebook instance\.

   1. Edit a new profile by typing the following in the terminal:

      ```
      vi /home/ec2-user/.aws/config
      ```

   1. Edit the file with the following profile information:

      ```
      [profile CrossAccountAccessProfile]
      region = us-west-2
      role_arn = arn:aws:iam::CodeCommitAccount:role/CrossAccountRepositoryContributorRole
      credential_source=Ec2InstanceMetadata
      output = json
      ```

      Where *CodeCommitAccount* is the account that contains the CodeCommit repository, *CrossAccountAccessProfile* is the name of the new profile, and *CrossAccountRepositoryContributorRole* is the name of the role you created in the previous step\.

1. On the notebook instance, configure git to use the profile you created in the previous step:

   1. Open the notebook instance\.

   1. Open a terminal in the notebook instance\.

   1. Edit the Git configuration file typing the following in the terminal:

      ```
      vi /home/ec2-user/.gitconfig
      ```

   1. Edit the file with the following profile information:

      ```
      [credential]
              helper = !aws codecommit credential-helper --profile CrossAccountAccessProfile $@
              UseHttpPath = true
      ```

      Where *CrossAccountAccessProfile* is the name of the profile that you created in the previous step\.