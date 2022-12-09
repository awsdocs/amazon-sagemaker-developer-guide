# Use Amazon SageMaker Studio Notebooks<a name="notebooks"></a>

Amazon SageMaker Studio notebooks are collaborative notebooks that you can launch quickly because you don't need to set up compute instances and file storage beforehand\. A set of instance types, known as *Fast launch* types are designed to launch in under two minutes\. Studio notebooks provide persistent storage, which enables you to view and share notebooks even if the instances that the notebooks run on are shut down\.

You can share your notebooks with others, so that they can easily reproduce your results and collaborate while building models and exploring your data\. You provide access to a read\-only copy of the notebook through a secure URL\. Dependencies for your notebook are included in the notebook's metadata\. When your colleagues copy the notebook, it opens in the same environment as the original notebook\.

A Studio notebook runs in an environment defined by the following:
+ Amazon EC2 instance type – The hardware configuration the notebook runs on\. The configuration includes the number and type of processors \(vCPU and GPU\), and the amount and type of memory\. The instance type determines the pricing rate\.
+ SageMaker image – A container image that is compatible with SageMaker Studio\. The image consists of the kernels, language packages, and other files required to run a notebook in Studio\. There can be multiple images in an instance\. For more information, see [Bring your own SageMaker image](studio-byoi.md)\.
+ KernelGateway app – A SageMaker image runs as a KernelGateway app\. The app provides access to the kernels in the image\. There is a one\-to\-one correspondence between a SageMaker image and a SageMaker app\.
+ Kernel – The process that inspects and runs the code contained in the notebook\. A kernel is defined by a *kernel spec* in the image\. There can be multiple kernels in an image\.

You can change any of these resources from within the notebook\.

The following diagram outlines how a notebook kernel runs in relation to the KernelGateway App, User, and Domain\.

![\[How a notebook kernel runs in relation to the KernelGateway App, User, and Domain.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-components.png)

Sample SageMaker Studio notebooks are available in the [aws\_sagemaker\_studio](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/aws_sagemaker_studio) folder of the [Amazon SageMaker example GitHub repository](https://github.com/awslabs/amazon-sagemaker-examples)\. Each notebook comes with the necessary SageMaker image that opens the notebook with the appropriate kernel\.

We recommend that you familiarize yourself with the SageMaker Studio interface and the Studio notebook toolbar before creating or using a Studio notebook\. For more information, see [Amazon SageMaker Studio UI Overview](studio-ui.md) and [Use the Studio Notebook Toolbar](notebooks-menu.md)\.

**Topics**
+ [How Are Amazon SageMaker Studio Notebooks Different from Notebook Instances?](notebooks-comparison.md)
+ [Get Started](notebooks-get-started.md)
+ [Amazon SageMaker Studio Tour](gs-studio-end-to-end.md)
+ [Create or Open an Amazon SageMaker Studio Notebook](notebooks-create-open.md)
+ [Use the Studio Notebook Toolbar](notebooks-menu.md)
+ [Install External Libraries and Kernels in Amazon SageMaker Studio](studio-notebooks-add-external.md)
+ [Share and Use an Amazon SageMaker Studio Notebook](notebooks-sharing.md)
+ [Get Studio Notebook and App Metadata](notebooks-run-and-manage-metadata.md)
+ [Get Notebook Differences](notebooks-diff.md)
+ [Manage Resources](notebooks-run-and-manage.md)
+ [Usage Metering](notebooks-usage-metering.md)
+ [Available Resources](notebooks-resources.md)