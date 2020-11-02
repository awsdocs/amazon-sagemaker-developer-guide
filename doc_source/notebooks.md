# Use Amazon SageMaker Studio Notebooks<a name="notebooks"></a>

Amazon SageMaker Studio notebooks are collaborative notebooks that you can launch quickly because you don't need to set up compute instances and file storage beforehand\. A set of instance types, known as *Fast launch* types are designed to launch in under two minutes\. SageMaker Studio notebooks provide persistent storage, which enables you to view and share notebooks even if the instances that the notebooks run on are shut down\.

You can share your notebooks with others, so that they can easily reproduce your results and collaborate while building models and exploring your data\. You provide access to a read\-only copy of the notebook through a secure URL\. Dependencies for your notebook are included in the notebook's metadata\. When your colleagues copy the notebook, it opens in the same environment as the original notebook\.

A SageMaker Studio notebook runs in an environment defined by the following:
+ Instance type – The hardware configuration the notebook runs on\. The configuration includes the number and type of processors \(vCPU and GPU\), and the amount and type of memory\. The instance type determines the pricing rate\.
+ SageMaker image – A container image that is compatible with SageMaker Studio\. The image consists of the kernels, language packages, and other files required to run a notebook in Studio\.
+ Kernel – The process that runs the code contained in the notebook\. A kernel is defined by a Conda environment\.

You can change any of these resources from within the notebook\.

Sample SageMaker Studio notebooks are available in the [aws\_sagemaker\_studio](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/aws_sagemaker_studio) folder of the [Amazon SageMaker example GitHub repository](https://github.com/awslabs/amazon-sagemaker-examples)\. Each notebook comes with the necessary SageMaker image that opens the notebook with the appropriate kernel\.

We recommend that you familiarize yourself with the Amazon SageMaker Studio interface before creating or using a SageMaker Studio notebook\. For more information, see [Amazon SageMaker Studio UI Overview](studio-ui.md)\.

**Topics**
+ [How Are Amazon SageMaker Studio Notebooks Different from Notebook Instances?](notebooks-comparison.md)
+ [Get Started](notebooks-get-started.md)
+ [Create or Open an Amazon SageMaker Studio Notebook](notebooks-create-open.md)
+ [Use the Amazon SageMaker Studio Notebook Toolbar](notebooks-menu.md)
+ [Share and Use a Amazon SageMaker Studio Notebook](notebooks-sharing.md)
+ [Get Notebook and App Metadata](notebooks-run-and-manage-metadata.md)
+ [Get Notebook Differences](notebooks-diff.md)
+ [Manage Resources](notebooks-run-and-manage.md)
+ [Usage Metering](notebooks-usage-metering.md)
+ [Available Resources](notebooks-resources.md)