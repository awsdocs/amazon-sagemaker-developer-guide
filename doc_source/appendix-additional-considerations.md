# Notebook Instance Security<a name="appendix-additional-considerations"></a>

Note the following security considerations for notebook instances\.


+ [Notebook Instances Are Enabled with Internet Access](#appendix-notebook-and-internet-access)
+ [Notebook Instances Provide the Best Experience for a Single User](#appendix-notebook-and-single-user)

## Notebook Instances Are Enabled with Internet Access<a name="appendix-notebook-and-internet-access"></a>

Amazon SageMaker notebook instances are Internet\-enabled\. This allows data scientists to download popular packages and notebooks, customize their development environment, and work efficiently\. However, if you connect a notebook instance to your VPC, the notebook instance provides an additional avenue for unauthorized access to your data\. For example, a malicious user or code that you accidentally install on the computer \(in the form of a publicly available notebook or a publicly available source code library\) could access your data\.

## Notebook Instances Provide the Best Experience for a Single User<a name="appendix-notebook-and-single-user"></a>

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