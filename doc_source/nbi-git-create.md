# Create a Notebook Instance with an Associated Git Repository<a name="nbi-git-create"></a>

You can associate Git repositories with a notebook instance when you create the notebook instance by using the AWS Management Console, or the AWS CLI\. If you want to use a CodeCommit repository that is in a different AWS account than the notebook instance, set up cross\-account access for the repository\. For information, see [Associate a CodeCommit Repository in a Different AWS Account with a Notebook Instance](nbi-git-cross.md)\.

**Topics**
+ [Create a Notebook Instance with an Associated Git Repository \(Console\)](#nbi-git-create-console)
+ [Create a Notebook Instance with an Associated Git Repository \(CLI\)](nbi-git-create-cli.md)

## Create a Notebook Instance with an Associated Git Repository \(Console\)<a name="nbi-git-create-console"></a>

**To create a notebook instance and associate Git repositories in the Amazon SageMaker console**

1. Follow the instructions at [Step 2: Create an Amazon SageMaker Notebook Instance](gs-setup-working-env.md)\.

1. For **Git repositories**, choose Git repositories to associate with the notebook instance\.

   1. For **Default repository**, choose a repository that you want to use as your default repository\. SageMaker clones this repository as a subdirectory in the Jupyter startup directory at `/home/ec2-user/SageMaker`\. When you open your notebook instance, it opens in this repository\. To choose a repository that is stored as a resource in your account, choose its name from the list\. To add a new repository as a resource in your account, choose **Add a repository to SageMaker \(opens the Add repository flow in a new window\)** and then follow the instructions at [Create a Notebook Instance with an Associated Git Repository \(Console\)](#nbi-git-create-console)\. To clone a public repository that is not stored in your account, choose **Clone a public Git repository to this notebook instance only**, and then specify the URL for that repository\.

   1. For **Additional repository 1**, choose a repository that you want to add as an additional directory\. SageMaker clones this repository as a subdirectory in the Jupyter startup directory at `/home/ec2-user/SageMaker`\. To choose a repository that is stored as a resource in your account, choose its name from the list\. To add a new repository as a resource in your account, choose **Add a repository to SageMaker \(opens the Add repository flow in a new window\)** and then follow the instructions at [Create a Notebook Instance with an Associated Git Repository \(Console\)](#nbi-git-create-console)\. To clone a repository that is not stored in your account, choose **Clone a public Git repository to this notebook instance only**, and then specify the URL for that repository\.

      Repeat this step up to three times to add up to three additional repositories to your notebook instance\.