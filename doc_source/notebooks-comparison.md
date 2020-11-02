# How Are Amazon SageMaker Studio Notebooks Different from Notebook Instances?<a name="notebooks-comparison"></a>

When you're starting a new notebook, we recommend that you create the notebook in Amazon SageMaker Studio instead of launching a notebook instance from the Amazon SageMaker console\. There are many benefits to using a SageMaker Studio notebook, including the following:
+ Starting a Studio notebook is faster than launching an instance\-based notebook\. Typically, it is 5\-10 times faster than instance\-based notebooks\.
+ Notebook sharing is an integrated feature in SageMaker Studio\. Users can generate a shareable link that reproduces the notebook code and also the SageMaker image required to execute it, in just a few clicks\.
+ SageMaker Studio notebooks come pre\-installed with the latest [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\.
+ SageMaker Studio notebooks are accessed from within Studio\. This enables you to build, train, debug, track, and monitor your models without leaving Studio\.
+ Each member of a Studio team gets their own home directory to store their notebooks and other files\. The directory is automatically mounted onto all instances and kernels as they're started, so their notebooks and other files are always available\. The home directories are stored in Amazon Elastic File System \(Amazon EFS\) so that you can access them from other services\.
+ When using AWS SSO, you use your SSO credentials through a unique URL to directly access SageMaker Studio\. You don't have to interact with the AWS Management Console to run your notebooks\.
+ Studio notebooks are equipped with a set of predefined SageMaker image settings to get you started faster\.

**Note**  
Studio notebooks don't support *local mode*\. However, you can use a notebook instance to train a sample of your dataset locally, and then use the same code in a Studio notebook to train on the full dataset\.

When you open a notebook in SageMaker Studio, the view is an extension of the JupyterLab interface\. The primary features are the same, so you'll find the typical features of a Jupyter notebook and JupyterLab\. For more information about the Studio interface, see [Amazon SageMaker Studio UI Overview](studio-ui.md)\.