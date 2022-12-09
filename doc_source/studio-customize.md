# Customize Amazon SageMaker Studio<a name="studio-customize"></a>

There are three options for customizing your Amazon SageMaker Studio environment\. You bring your own SageMaker image, use a Lifecycle Configuration script, or attach suggested Git repos to Studio \. These three options can be used individually or together\. 
+ **Bring your own SageMaker image:** A SageMaker image is a file that identifies the kernels, language packages, and other dependencies required to run a Jupyter notebook in Amazon SageMaker Studio\. Amazon SageMaker provides many built\-in images for you to use\. If you need different functionality, you can bring your own custom images to Studio\.
+ **Use Lifecycle Configurations with Amazon SageMaker Studio:** Lifecycle Configurations are shell scripts triggered by Amazon SageMaker Studio lifecycle events, such as starting a new Studio notebook\. You can use Lifecycle Configurations to automate customization for your Studio environment\. For example, you can install custom packages, configure notebook extensions, preload datasets, and set up source code repositories\.
+ **Attach suggested Git repos to Studio:**You can attach suggested Git repository URLs at the Amazon SageMaker Domain or user profile level\. Then, you can select the repo URL from the list of suggestions and clone that into your environment using the Git extension in Studio\. 

The following topics show how to use these three options to customize your Amazon SageMaker Studio environment\.

**Topics**
+ [Bring your own SageMaker image](studio-byoi.md)
+ [Use Lifecycle Configurations with Amazon SageMaker Studio](studio-lcc.md)
+ [Attach Suggested Git Repos to Studio](studio-git-attach.md)