# Use Git Repositories in a Notebook Instance<a name="git-nbi-use"></a>

When you open a notebook instance that has Git repositories associated with it, it opens in the default repository, which is installed in your notebook instance directly under `/home/ec2-user/SageMaker`\. You can open and create notebooks, and you can manually run Git commands in a notebook cell\. For example:

```
!git pull origin master
```

To open any of the additional repositories, navigate up one folder\. The additional repositories are also installed as directories under `/home/ec2-user/SageMaker`\.

If you open the notebook instance with a JupyterLab interface, the jupyter\-git extension is installed and available to use\. For information about the jupyter\-git extension for JupyterLab, see [https://github\.com/jupyterlab/jupyterlab\-git](https://github.com/jupyterlab/jupyterlab-git)\.

When you open a notebook instance in JupyterLab, you see the git repositories associated with it on the left menu:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/git-notebook.png)

You can use the jupyter\-git extension to manage git visually, instead of using the command line:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jupyterlab-git.png)