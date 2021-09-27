# Use Lifecycle Configurations with Amazon SageMaker Studio<a name="studio-lcc"></a>

Lifecycle Configurations are shell scripts triggered by Amazon SageMaker Studio lifecycle events, such as starting a new Studio notebook\. You can use Lifecycle Configurations to automate customization for your Studio environment\. This customization includes installing custom packages, configuring notebook extensions, preloading datasets, and setting up source code repositories\. 

Using Lifecycle Configurations gives you flexibility and control to configure Studio to meet your specific needs\. For example, you can create a minimal set of base container images with the most commonly used packages and libraries, then use Lifecycle Configurations to install additional packages for specific use cases across your data science and machine learning teams\. 

For example Lifecycle Configuration scripts, see the [Studio Lifecycle Configuration examples repo](https://github.com/aws-samples/sagemaker-studio-lifecycle-config-examples)\. For a blog on implementing Lifecycle Configurations, see [Customize Amazon SageMaker Studio using Lifecycle Configurations](http://aws.amazon.com/blogs/machine-learning/customize-amazon-sagemaker-studio-using-lifecycle-configurations/)\.

**Topics**
+ [Creating and Associating a Lifecycle Configuration](studio-lcc-create.md)
+ [Setting Default Lifecycle Configurations](studio-lcc-defaults.md)
+ [Debugging Lifecycle Configurations](studio-lcc-debug.md)
+ [Updating and deleting Lifecycle Configurations](studio-lcc-delete.md)