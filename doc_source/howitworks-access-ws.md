# Accessing Notebook Instances<a name="howitworks-access-ws"></a>

To access your Amazon SageMaker notebook instances, choose one of the following options: 
+ Use the console\.Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.Choose **Notebook instances**\. The console displays a list of notebook instances in your account\. To open a notebook instance, choose the **Open** action for the instance\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/ws-notebook-10.png)

  The console uses your sign\-in credentials to send a [CreatePresignedNotebookInstanceUrl](API_CreatePresignedNotebookInstanceUrl.md) API request to Amazon SageMaker\. Amazon SageMaker returns the URL for your notebook instance, and the console opens the URL in another browser tab and displays the Jupyter notebook dashboard\. 
+ Use the API\.

  To get the URL for the notebook instance, call the [CreatePresignedNotebookInstanceUrl](API_CreatePresignedNotebookInstanceUrl.md) API and use the URL that the API returns to open the notebook instance\.

Use the Jupyter notebook dashboard to create and manage notebooks and to write code\. For more information about Jupyter notebooks, see [http://jupyter\.org/documentation\.html](http://jupyter.org/documentation.html)\.

## Limit Access to a Notebook Instance by IP Address<a name="nbi-ip-filter"></a>

To allow access to a notebook instance only from IP addresses in a list that you specify, attach an IAM policy that denies access to [CreatePresignedNotebookInstanceUrl](API_CreatePresignedNotebookInstanceUrl.md) unless the call comes from an IP address in the list to every AWS Identity and Access Management user, group, or role used to access the notebook instance\. For information about creating IAM policies, see [Creating IAM Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html) in the *AWS Identity and Access Management User Guide*\. Use the `NotIpAddress` condition operator and the `aws:SourceIP` condition context key to specify the list of IP addresses that you want to have access to the notebook instance\. For information about IAM condition operators, see [IAM JSON Policy Elements: Condition Operators](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html) in the *AWS Identity and Access Management User Guide*\. For information about IAM condition context keys, see [AWS Global Condition Context Keys](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html)\.

For example, the following policy allows access to a notebook instance only from IP addresses in the ranges `192.0.2.0`\-`192.0.2.255` and `203.0.113.0`\-`203.0.113.255`:

```
{
    "Version": "2012-10-17",
    "Statement": {
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
    }
}
```

The policy restricts access to both the call to `CreatePresignedNotebookInstanceUrl` and to the URL that the call returns\. The policy also restricts access to opening a notebook instance in the console\.

**Note**  
Using this method to filter by IP address is incompatible when [connecting to Amazon SageMaker through a VPC interface endpoint\.](http://docs.aws.amazon.com/sagemaker/latest/dg/interface-vpc-endpoint.html)\.