# Collaborate with shared spaces<a name="domain-space"></a>

 A shared space consists of a shared JupyterServer application and a shared directory\. All user profiles in a Domain have access to all shared spaces in the Domain\. Amazon SageMaker automatically scopes resources in a shared space within the context of the Amazon SageMaker Studio application that you launch in that shared space\. Resources in a shared space include including notebooks, files, experiments, and models\. 

A shared space only supports Studio and KernelGateway applications\. A shared space only supports the use of a JupyterLab 3 image Amazon Resource Name \(ARN\)\. For more information, see [JupyterLab Versioning](studio-jl.md)\.

 Amazon SageMaker automatically tags all SageMaker resources that you create within the scope of a shared space\. You can use these tags to monitor costs and plan budgets using tools, such as AWS Budgets\. 

A shared space uses the same VPC settings as the Domain that it's created in\. 

**Note**  
Domains with AWS IAM Identity Center \(successor to AWS Single Sign\-On\) authentication do not currently support the use of shared spaces\. Shared spaces do not support the use of Amazon SageMaker Data Wrangler or Amazon EMR cross\-account clusters\. 

 **Automatic tagging** 

 All resources created in a shared space are automatically tagged with a Domain ARN tag and shared space ARN tag\. The Domain ARN tag is based on the Domain ID, while the shared space ARN tag is based on the shared space name\.  

 You can use these tags to monitor AWS CloudTrail usage\. For more information, see [Log Amazon SageMaker API Calls with AWS CloudTrail](https://docs.aws.amazon.com/sagemaker/latest/dg/logging-using-cloudtrail.html)\. 

 You can also use these tags to monitor costs with AWS Billing and Cost Management\. For more information, see [Using AWS cost allocation tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html)\. 

 **Real time co\-editing of notebooks** 

 A key benefit of a shared space is that it facilitates collaboration between members of the shared space in real time\. Users collaborating in a workspace get access to a shared Studio application where they can access, read, and edit their notebooks in real time\. Real time collaboration is only supported for JupyterServer applications within a shared space\. 

 Users with access to a shared space can simultaneously open, view, edit, and execute Jupyter notebooks in the shared Studio application in that space\. 

The notebook indicates each co\-editing user with a different cursor that shows the user profile name\. While multiple users can view the same notebook, co\-editing is best suited for small groups of two to five users\.

To track changes being made by multiple users, we strongly recommended using Studio's built\-in Git\-based version control\.

 **JupyterServer 2** 

To use shared spaces, Jupyter Server version 2 is required\. Certain JupyterLab extensions and packages can forcefully downgrade Jupyter Server to version 1\. This prevents the use of shared space\. Run the following from the command prompt to change the version number and continue using shared spaces\.

```
conda activate studio
pip install jupyter-server==2.0.0rc3
```