# Amazon SageMaker Studio Lab<a name="studio-lab"></a>

 Amazon SageMaker Studio Lab is a free service that gives customers access to AWS compute resources, in an environment based on open\-source JupyterLab\. It is based on the same architecture and user interface as Amazon SageMaker Studio, but with a subset of Studio capabilities\.

With Studio Lab, you can use AWS compute resources to create and run your Jupyter notebooks without signing up for an AWS account\. Because Studio Lab is based on open\-source JupyterLab, you can take advantage of open\-source Jupyter extensions to run your Jupyter notebooks\.

 **Studio Lab compared to Amazon SageMaker Studio**

While Studio Lab provides free access to AWS compute resources, Amazon SageMaker Studio provides the following advanced machine learning capabilities that Studio Lab does not support\.
+ Continuous integration and continuous delivery \(SageMaker Pipelines\)
+ Real\-time predictions
+ Large\-scale distributed training
+ Data preparation \(Amazon SageMaker Data Wrangler\)
+ Data labeling \(Amazon SageMaker Ground Truth\)
+ Feature Store
+ Bias analysis \(Clarify\)
+ Model deployment
+ Model monitoring

Studio also supports fine\-grained access control and security by using AWS Identity and Access Management \(IAM\), Amazon Virtual Private Cloud \(Amazon VPC\), and AWS Key Management Service \(AWS KMS\)\. Studio Lab does not support these Studio features, nor does it support the use of estimators and built\-in SageMaker algorithms\.

To export your Studio Lab projects for use with Studio, see [Export Amazon SageMaker Studio Lab environment to Amazon SageMaker Studio](studio-lab-use-migrate.md)\.

The following topics give information about Studio Lab and how to use it

**Topics**
+ [Amazon SageMaker Studio Lab components overview](studio-lab-overview.md)
+ [Onboard to Amazon SageMaker Studio Lab](studio-lab-onboard.md)
+ [Manage your account](studio-lab-manage-account.md)
+ [Launch your Amazon SageMaker Studio Lab project runtime](studio-lab-manage-runtime.md)
+ [Use Amazon SageMaker Studio Lab starter assets](studio-lab-integrated-resources.md)
+ [Use the Amazon SageMaker Studio Lab project runtime](studio-lab-use.md)
+ [Troubleshooting](studio-lab-troubleshooting.md)