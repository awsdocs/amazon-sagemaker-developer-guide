# Connect to a Notebook Instance Through a VPC Interface Endpoint<a name="notebook-interface-endpoint"></a>

You can connect to your notebook instance from your VPC through an [interface endpoint](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpce-interface.html) in your Virtual Private Cloud \(VPC\) instead of connecting over the internet\. When you use a VPC interface endpoint, communication between your VPC and the notebook instance is conducted entirely and securely within the AWS network\.

SageMaker notebook instances support [Amazon Virtual Private Cloud](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Introduction.html) \(Amazon VPC\) interface endpoints that are powered by [AWS PrivateLink](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Introduction.html#what-is-privatelink)\. Each VPC endpoint is represented by one or more [Elastic Network Interfaces](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html) \(ENIs\) with private IP addresses in your VPC subnets\.

**Note**  
Before you create an interface VPC endpoint to connect to a notebook instance, create an interface VPC endpoint to connect to the SageMaker API\. That way, when users call [  `CreatePresignedNotebookInstanceUrl`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreatePresignedNotebookInstanceUrl.html) to get the URL to connect to the notebook instance, that call also goes through the interface VPC endpoint\. For information, see [Connect to SageMaker Through a VPC Interface Endpoint](interface-vpc-endpoint.md)\.

You can create an interface endpoint to connect to your notebook instance with either the AWS console or AWS Command Line Interface \(AWS CLI\) commands\. For instructions, see [Creating an Interface Endpoint](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpce-interface.html#create-interface-endpoint)\. Make sure that you create an interface endpoint for all of the subnets in your VPC from which you want to connect to the notebook instance\.

When you create the interface endpoint, specify **aws\.sagemaker\.*region*\.notebook** as the service name\. After you create a VPC endpoint, enable private DNS for your VPC endpoint\. Anyone using the SageMaker API, the AWS CLI, or the console to connect to the notebook instance from within the VPC will connect to the notebook instance through the VPC endpoint instead of the public internet\.

SageMaker notebook instances support VPC endpoints in all AWS Regions where both [Amazon VPC](https://docs.aws.amazon.com/general/latest/gr/rande.html#vpc_region) and [SageMaker](https://docs.aws.amazon.com/general/latest/gr/rande.html#sagemaker_region) are available\.

**Topics**
+ [Connect Your Private Network to Your VPC](#notebook-private-link-vpn)
+ [Create a VPC Endpoint Policy for SageMaker Notebook Instances](#nbi-private-link-policy)
+ [Restrict Access to Connections from Within Your VPC](#notebook-private-link-restrict)

## Connect Your Private Network to Your VPC<a name="notebook-private-link-vpn"></a>

To connect to your notebook instance through your VPC, you either have to connect from an instance that is inside the VPC, or connect your private network to your VPC by using an Amazon Virtual Private Network \(VPN\) or AWS Direct Connect\. For information about Amazon VPN, see [VPN Connections](https://docs.aws.amazon.com/vpc/latest/userguide/vpn-connections.html) in the *Amazon Virtual Private Cloud User Guide*\. For information about AWS Direct Connect, see [Creating a Connection](https://docs.aws.amazon.com/directconnect/latest/UserGuide/create-connection.html) in the *AWS Direct Connect User Guide*\.

## Create a VPC Endpoint Policy for SageMaker Notebook Instances<a name="nbi-private-link-policy"></a>

You can create a policy for Amazon VPC endpoints for SageMaker notebook instances to specify the following:
+ The principal that can perform actions\.
+ The actions that can be performed\.
+ The resources on which actions can be performed\.

For more information, see [Controlling Access to Services with VPC Endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-access.html) in the *Amazon VPC User Guide*\.

The following example of a VPC endpoint policy specifies that all users that have access to the endpoint are allowed to access the notebook instance named `myNotebookInstance`\.

```
{
  "Statement": [
      {
          "Action": "sagemaker:CreatePresignedNotebookInstanceUrl",
          "Effect": "Allow",
          "Resource": "arn:aws:sagemaker:us-west-2:123456789012:notebook-instance/myNotebookInstance",
          "Principal": "*"
      }
  ]
}
```

Access to other notebook instances is denied\.

## Restrict Access to Connections from Within Your VPC<a name="notebook-private-link-restrict"></a>

Even if you set up an interface endpoint in your VPC, individuals outside the VPC can connect to the notebook instance over the internet\.

**Important**  
If you apply an IAM policy similar to one of the following, users can't access the specified SageMaker APIs or the notebook instance through the console\.

To restrict access to only connections made from within your VPC, create an AWS Identity and Access Management policy that restricts access to only calls that come from within your VPC\. Then add that policy to every AWS Identity and Access Management user, group, or role used to access the notebook instance\.

**Note**  
This policy allows connections only to callers within a subnet where you created an interface endpoint\.

```
{
    "Id": "notebook-example-1",
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Enable Notebook Access",
            "Effect": "Allow",
            "Action": [
                "sagemaker:CreatePresignedNotebookInstanceUrl",
                "sagemaker:DescribeNotebookInstance"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "aws:SourceVpc": "vpc-111bbaaa"
                }
            }
        }
    ]
}
```

If you want to restrict access to the notebook instance to only connections made using the interface endpoint, use the `aws:SourceVpce` condition key instead of `aws:SourceVpc`:

```
{
    "Id": "notebook-example-1",
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Enable Notebook Access",
            "Effect": "Allow",
            "Action": [
                "sagemaker:CreatePresignedNotebookInstanceUrl",
                "sagemaker:DescribeNotebookInstance"
            ],
            "Resource": "*",
            "Condition": {
                "ForAnyValue:StringEquals": {
                    "aws:sourceVpce": [
                        "vpce-111bbccc",
                        "vpce-111bbddd"
                    ]
                }
            }
        }
    ]
}
```

Both of these policy examples assume that you have also created an interface endpoint for the SageMaker API\. For more information, see [Connect to SageMaker Through a VPC Interface Endpoint](interface-vpc-endpoint.md)\. In the second example, one of the values for `aws:SourceVpce` is the ID of the interface endpoint for the notebook instance\. The other is the ID of the interface endpoint for the SageMaker API\.

The policy examples here include [  `DescribeNotebookInstance`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeNotebookInstance.html) because typically you would call `DescribeNotebookInstance` to make sure that the `NotebookInstanceStatus` is `InService` before you try to connect to it\. For example:

```
aws sagemaker describe-notebook-instance \
                    --notebook-instance-name myNotebookInstance
                    
                    
{
   "NotebookInstanceArn":
   "arn:aws:sagemaker:us-west-2:1234567890ab:notebook-instance/mynotebookinstance",
   "NotebookInstanceName": "myNotebookInstance",
   "NotebookInstanceStatus": "InService",
   "Url": "mynotebookinstance.notebook.us-west-2.sagemaker.aws",
   "InstanceType": "ml.m4.xlarge",
   "RoleArn":
   "arn:aws:iam::1234567890ab:role/service-role/AmazonSageMaker-ExecutionRole-12345678T123456",
   "LastModifiedTime": 1540334777.501,
   "CreationTime": 1523050674.078,
   "DirectInternetAccess": "Disabled"
}
aws sagemaker create-presigned-notebook-instance-url --notebook-instance-name myNotebookInstance
                
                
{
   "AuthorizedUrl": "https://mynotebookinstance.notebook.us-west-2.sagemaker.aws?authToken=AuthToken
}
```

For both of these calls, if you did not enable private DNS hostnames for your VPC endpoint, or if you are using a version of the AWS SDK that was released before August 13, 2018, you must specify the endpoint URL in the call\. For example, the call to `create-presigned-notebook-instance-url` would be:

```
aws sagemaker create-presigned-notebook-instance-url
    --notebook-instance-name myNotebookInstance --endpoint-url
    VPC_Endpoint_ID.api.sagemaker.Region.vpce.amazonaws.com
```