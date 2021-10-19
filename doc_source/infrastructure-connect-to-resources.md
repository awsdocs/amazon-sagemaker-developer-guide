# Connect to Resources in a VPC<a name="infrastructure-connect-to-resources"></a>

Amazon SageMaker Studio and SageMaker notebook instances allow direct internet access by default\. This allows you to download popular packages and notebooks, customize your development environment, and work efficiently\. However, this could provide an additional avenue for unauthorized access to your data\. For example, if you install malicious code on your computer in the form of a publicly available notebook or a publicly available source code library, it could access your data\. You can choose to restrict which traffic can access the internet by launching your Amazon SageMaker Studio and SageMaker notebook instances in a [Amazon Virtual Private Cloud \(Amazon VPC\)](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html) of your choosing\. 

An Amazon Virtual Private Cloud is a virtual network dedicated to your AWS account\. With an Amazon VPC, you can control the network access and internet connectivity of your Amazon SageMaker Studio and notebook instances\. You can disable direct internet access to add an additional layer of security\.

The following topics describe how to connect your SageMaker Studio instances and notebook instances to resources in a VPC\.

**Topics**
+ [Connect SageMaker Studio Notebooks to Resources in a VPC](studio-notebooks-and-internet-access.md)
+ [Connect a Notebook Instance to Resources in a VPC](appendix-notebook-and-internet-access.md)