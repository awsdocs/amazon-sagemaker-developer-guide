# Notebook Instance Security<a name="appendix-additional-considerations"></a>

Note the following security considerations for notebook instances\.

**Topics**
+ [Notebook Instances Are Internet\-Enabled by Default](#appendix-notebook-and-internet-access)

## Notebook Instances Are Internet\-Enabled by Default<a name="appendix-notebook-and-internet-access"></a>

Amazon SageMaker notebook instances are internet\-enabled\. This allows you to download popular packages and notebooks, customize your development environment, and work efficiently\. However, if you connect a notebook instance to your VPC, the notebook instance provides an additional avenue for unauthorized access to your data\. For example, a malicious user or code that you accidentally install on the computer \(in the form of a publicly available notebook or a publicly available source code library\) could access your data\. If you do not want Amazon SageMaker to provide internet access to your notebook instance, you can disable direct internet access when you specify a VPC for your notebook instance\. If you disable direct internet access, the notebook instance won't be able to train or host models unless your VPC has a NAT gateway and your security groups allow outbound connections\. For information about creating a VPC interface endpoint for your notebook instance, see [Connect to a Notebook Instance Through a VPC Interface Endpoint](notebook-interface-endpoint.md)\. For information about setting up a NAT gateway for your VPC, see [Scenario 2: VPC with Public and Private Subnets \(NAT\)](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Scenario2.html) in the in the *Amazon Virtual Private Cloud User Guide*\. For information about security groups, see [Security Groups for Your VPC](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_SecurityGroups.html)\. 

### Notebook Instances Provide the Best Experience for a Single User<a name="appendix-notebook-and-single-user"></a>

An Amazon SageMaker notebook instance is designed to work best for an individual user\. It is designed to give data scientists and other users the most power for managing their development environment\. A notebook instance user has root access for installing packages and other pertinent software\. We recommend that you exercise judgement when granting individuals access to notebook instances that are attached to a VPC that contains sensitive information\. For example, you might grant a user access to a notebook instance with an IAM policy, as in the following example:

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