# Limit Access to a Notebook Instance by IP Address<a name="nbi-ip-filter"></a>

To allow access to a notebook instance only from IP addresses in a list that you specify, attach an IAM policy that denies access to [CreatePresignedNotebookInstanceUrl](API_CreatePresignedNotebookInstanceUrl.md) unless the call comes from an IP address in the list to every AWS Identity and Access Management user, group, or role used to access the notebook instance\. For information about creating IAM policies, see [Creating IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html) in the *AWS Identity and Access Management User Guide*\. To specify the list of IP addresses that you want to have access to the notebook instance, use the `NotIpAddress` condition operator and the `aws:SourceIP` condition context key\. For information about IAM condition operators, see [IAM JSON Policy Elements: Condition Operators](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html) in the *AWS Identity and Access Management User Guide*\. For information about IAM condition context keys, see [AWS Global Condition Context Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html)\.

For example, the following policy allows access to a notebook instance only from IP addresses in the ranges `192.0.2.0`\-`192.0.2.255` and `203.0.113.0`\-`203.0.113.255`:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": "sagemaker:CreatePresignedNotebookInstanceUrl",
            "Resource": "*",
            "Condition": {
                "NotIpAddress": {
                    "aws:SourceIp": [
                        "192.0.2.0/24",
                        "203.0.113.0/24"
                    ]
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": "sagemaker:CreatePresignedNotebookInstanceUrl",
            "Resource": "*",
            "Condition": {
                "IpAddress": {
                    "aws:SourceIp": [
                        "192.0.2.0/24",
                        "203.0.113.0/24"
                    ]
                }
            }
        }
    ]
}
```

The policy restricts access to both the call to `CreatePresignedNotebookInstanceUrl` and to the URL that the call returns\. The policy also restricts access to opening a notebook instance in the console and is enforced for every HTTP request and WebSocket frame that attempts to connect to the notebook instance\.

**Note**  
Using this method to filter by IP address is incompatible when [connecting to Amazon SageMaker through a VPC interface endpoint\.](https://docs.aws.amazon.com/sagemaker/latest/dg/interface-vpc-endpoint.html)\. For information about restricting access to a notebook instance when connecting through a VPC interface endpoint, see [Connect to a Notebook Instance Through a VPC Interface Endpoint](notebook-interface-endpoint.md)\.