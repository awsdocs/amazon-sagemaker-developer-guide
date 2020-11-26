# Connect a Notebook Instance to Resources in a VPC<a name="appendix-notebook-and-internet-access"></a>

SageMaker notebook instances are internet\-enabled by default\. This allows you to download popular packages and notebooks, customize your development environment, and work efficiently\. However, this could provide an additional avenue for unauthorized access to your data\. For example, a malicious user or code that you accidentally install on the computer \(in the form of a publicly available notebook or a publicly available source code library\) could access your data\. You can choose to launch your notebook instance in your Virtual Private Cloud \(VPC\) to restrict which traffic can go through the public Internet\. When launched with your VPC attached, the notebook instance can either be configured with or without direct internet access\.

When your notebook allows *direct internet access*, SageMaker provides a network interface that allows the notebook to communicate with the internet through a VPC managed by SageMaker\. Traffic within your VPC's CIDR goes through elastic network interface created in your VPC\. All the other traffic goes through the network interface created by SageMaker, which is essentially through the public internet\. Traffic to gateway VPC endpoints like Amazon S3 and DynamoDB goes through the public internet, while traffic to interface VPC endpoints \(interface endpoints\) still goes through your VPC\. If you want to use gateway VPC endpoints, you might want to disable direct internet access\. 

To disable direct internet access, you can specify a VPC for your notebook instance\. By doing so, you prevent SageMaker from providing internet access to your notebook instance\. As a result, the notebook instance won't be able to train or host models unless your VPC has an interface endpoint \(PrivateLink\) or a NAT gateway, and your security groups allow outbound connections\. 

For information about creating a VPC interface endpoint to use AWS PrivateLink for your notebook instance, see [Connect to a Notebook Instance Through a VPC Interface Endpoint](notebook-interface-endpoint.md)\. For information about setting up a NAT gateway for your VPC, see [VPC with Public and Private Subnets \(NAT\)](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Scenario2.html) in the *Amazon Virtual Private Cloud User Guide*\. For information about security groups, see [Security Groups for Your VPC](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_SecurityGroups.html)\. For more information about networking configurations in each networking mode and configuring network on premise, see [Understanding Amazon SageMaker notebook instance networking configurations and advanced routing options](http://aws.amazon.com/blogs/machine-learning/understanding-amazon-sagemaker-notebook-instance-networking-configurations-and-advanced-routing-options/)\. 

## Security and Shared Notebook Instances<a name="appendix-notebook-and-single-user"></a>

A SageMaker notebook instance is designed to work best for an individual user\. It is designed to give data scientists and other users the most power for managing their development environment\.

A notebook instance user has root access for installing packages and other pertinent software\. We recommend that you exercise judgement when granting individuals access to notebook instances that are attached to a VPC that contains sensitive information\. For example, you might grant a user access to a notebook instance with an IAM policy, as shown in the following example:

```
{
  "Version": "2012-10-17",
  "Statement": [
   {
      "Effect": "Allow",
      "Action": "sagemaker:CreatePresignedNotebookInstanceUrl",
      "Resource": "arn:aws:sagemaker:region:account-id:notebook-instance/myNotebookInstance"
   }
  ]
}
```