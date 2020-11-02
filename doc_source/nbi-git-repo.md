# Associate Git Repositories with SageMaker Notebook Instances<a name="nbi-git-repo"></a>

Associate Git repositories with your notebook instance to save your notebooks in a source control environment that persists even if you stop or delete your notebook instance\. You can associate one default repository and up to three additional repositories with a notebook instance\. The repositories can be hosted in AWS CodeCommit, GitHub, or on any other Git server\. Associating Git repositories with your notebook instance can be useful for:
+ Persistence \- Notebooks in a notebook instance are stored on durable Amazon EBS volumes, but they do not persist beyond the life of your notebook instance\. Storing notebooks in a Git repository enables you to store and use notebooks even if you stop or delete your notebook instance\.
+ Collaboration \- Peers on a team often work on machine learning projects together\. Storing your notebooks in Git repositories allows peers working in different notebook instances to share notebooks and collaborate on them in a source\-control environment\.
+ Learning \- Many Jupyter notebooks that demonstrate machine learning techniques are available in publicly hosted Git repositories, such as on GitHub\. You can associate your notebook instance with a repository to easily load Jupyter notebooks contained in that repository\.

There are two ways to associate a Git repository with a notebook instance:
+ Add a Git repository as a resource in your Amazon SageMaker account\. Then, to access the repository, you can specify an AWS Secrets Manager secret that contains credentials\. That way, you can access repositories that require authentication\.
+ Associate a public Git repository that is not a resource in your account\. If you do this, you cannot specify credentials to access the repository\.

**Topics**
+ [Add a Git Repository to Your Amazon SageMaker Account](nbi-git-resource.md)
+ [Create a Notebook Instance with an Associated Git Repository](nbi-git-create.md)
+ [Associate a CodeCommit Repository in a Different AWS Account with a Notebook Instance](nbi-git-cross.md)
+ [Use Git Repositories in a Notebook Instance](git-nbi-use.md)