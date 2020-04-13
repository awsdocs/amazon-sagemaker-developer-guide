# How Are These Notebooks Different from Instance\-based Amazon SageMaker Notebooks?<a name="notebooks-comparison"></a>


****  

|  | 
| --- |
| Amazon SageMaker Studio Notebooks is in preview release and is subject to change\. | 

 When starting a new notebook, we recommend that you use the notebook feature in Amazon SageMaker Studio instead of launching a notebook instance from the console\. There are many benefits to using an Amazon SageMaker Studio notebook: 
+ Starting an Amazon SageMaker Studio notebook is faster than launching an instance\-based notebook\. Typically, it is 5\-10 times faster than instance\-based notebooks\. 
+ Notebook sharing is an integrated feature in Amazon SageMaker Studio\. Users can generate a shareable link that reproduces not only the notebook code, but also the environment required to execute it, in just a few clicks\.  You can also share the environment that hosts\.  
+ Amazon SageMaker Studio notebooks come pre\-installed with the latest Amazon SageMaker SDK and can be accessed within the studio’s IDE, allowing you to build, train, debug, track, and monitor your models\. 
+ As a member of an Amazon SageMaker Studio team, you get a home directory independent of a particular instance\. This directory is automatically mounted into all notebook servers and kernels as they’re started, so you always have your notebooks and other files\. The home directories are in your account in EFS so that you can access them from other services\. 
+ When using AWS SSO, you use your AWS domain credentials and have a unique URL for your team\. You never have to interact with the AWS Management Console to run your notebooks\. 
+ Amazon SageMaker Studio Notebooks are equipped with a set of predefined environments to get your organization started on data science projects faster\. 

 When you open a notebook in Amazon SageMaker Studio, the default view is a JuypterLab interface\. The primary features are the same, however, so what you can expect to find the typical features of a Jupyter notebook and JupyterLab, you will find here as well\.  

 If you’re unfamiliar with JupyterLab features, it is recommended that you take a tour of the [JupyterLab user interface features in the JupyterLab documentation](https://jupyterlab.readthedocs.io/en/latest/user/interface.html)\. 