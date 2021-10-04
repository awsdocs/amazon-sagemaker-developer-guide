# Connect to SageMaker Studio Through an Interface VPC Endpoint<a name="studio-interface-endpoint"></a>

You can connect to Amazon SageMaker Studio from your [Amazon Virtual Private Cloud](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html) \(Amazon VPC\) through an [interface endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html) in your VPC instead of connecting over the internet\. When you use an interface VPC endpoint \(interface endpoint\), communication between your VPC and Studio is conducted entirely and securely within the AWS network\.

SageMaker Studio supports interface endpoints that are powered by [AWS PrivateLink](https://docs.aws.amazon.com/vpc/latest/userguide/endpoint-services-overview.html)\. Each interface endpoint is represented by one or more [Elastic network interfaces](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html) with private IP addresses in your VPC subnets\.

You can create an interface endpoint to connect to Studio with either the AWS console or the AWS Command Line Interface \(AWS CLI\)\. For instructions, see [Creating an interface endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint)\. Make sure that you create interface endpoints for all of the subnets in your VPC from which you want to connect to Studio\. 

When you create an interface enpoint, ensure that the security groups on your endpoint allow inbound access for HTTPS traffic from the security groups associated with SageMaker Studio\. For more information, see [Control access to services with VPC endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints-access.html#vpc-endpoints-security-groups)\.

**Note**  
In addition to creating an interface endpoint to connect to SageMaker Studio, create an interface endpoint to connect to the Amazon SageMaker API\. When users call [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreatePresignedDomainUrl.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreatePresignedDomainUrl.html) to get the URL to connect to Studio, that call goes through the interface endpoint used to connect to the SageMaker API\.

When you create the interface endpoint, specify **aws\.sagemaker\.*Region*\.studio** as the service name\. After you create the interface endpoint, enable private DNS for your endpoint\. Anyone who uses the SageMaker API, the AWS CLI, or the console to connect to SageMaker Studio from within the VPC connects to Studio through the interface endpoint instead of the public internet\. You also need to set up a custom DNS with private hosted zones for the Amazon VPC endpoint so SageMaker Studio can access the SageMaker API using the `api.sagemaker.$region.amazonaws.com` endpoint rather than using the VPC endpoint URL\. For instructions on setting up a private hosted zone, see [Working with private hosted zones](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/hosted-zones-private.html)\.

Studio supports interface endpoints in all AWS Regions where both [Amazon SageMaker](http://aws.amazon.com/sagemaker/pricing/) and [Amazon VPC](http://aws.amazon.com/vpc/pricing/) are available\.

**Topics**
+ [Create a VPC Endpoint Policy for SageMaker Studio](#studio-private-link-policy)
+ [Allow Access Only from Within Your VPC](#studio-private-link-restrict)

## Create a VPC Endpoint Policy for SageMaker Studio<a name="studio-private-link-policy"></a>

You can attach an Amazon VPC endpoint policy to the interface VPC endpoints that you use to connect to SageMaker Studio\. The endpoint policy controls access to Studio\. You can specify the following:
+ The principal that can perform actions\.
+ The actions that can be performed\.
+ The resources on which actions can be performed\. 

To use a VPC endpoint with SageMaker Studio, your endpoint policy must allow the `CreateApp` operation on the KernelGateway app type\. This allows traffic that is routed to through the VPC endpoint to call the `CreateApp` API\. The following example VPC endpoint policy shows how to allow the `CreateApp` operation\.

```
{
 "Statement": [
   {
     "Action": "sagemaker:CreateApp",
     "Effect": "Allow",
     "Resource": "arn:aws:sagemaker:us-west-2:acct-id:user-profile/domain-id/*",
     "Principal": "*"
   }
 ]
}
```

For more information, see [Controlling access to services with VPC endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-access.html)\.

The following example of a VPC endpoint policy specifies that all users that have access to the endpoint are allowed to access the user profiles in the SageMaker Studio domain with the specified domain ID\. Access to other domains is denied\.

```
{
  "Statement": [
      {
          "Action": "sagemaker:CreatePresignedDomainUrl",
          "Effect": "Allow",
          "Resource": "arn:aws:sagemaker:us-west-2:acct-id:user-profile/domain-id/*",
          "Principal": "*"
      }
  ]
}
```

## Allow Access Only from Within Your VPC<a name="studio-private-link-restrict"></a>

Users outside your VPC can connect to SageMaker Studio over the internet even if you set up an interface endpoint in your VPC\.

To allow access to only connections made from within your VPC, create an AWS Identity and Access Management \(IAM\) policy to that effect\. Add that policy to every IAM user, group, or role used to access Studio\. This feature is only supported in IAM mode, and is not supported in SSO mode\. The following examples demonstrate how to create such policies\.

**Important**  
If you apply an IAM policy similar to one of the following examples, users can't access SageMaker Studio or the specified SageMaker APIs through the SageMaker console\. To access Studio, users must use a presigned URL or call the SageMaker APIs directly\.

**Example 1: Allow connections only within the subnet of an interface endpoint**

The following policy allows connections only to callers within the subnet where you created the interface endpoint\.

```
{
    "Id": "sagemaker-studio-example-1",
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Enable SageMaker Studio Access",
            "Effect": "Allow",
            "Action": [
                "sagemaker:CreatePresignedDomainUrl",
                "sagemaker:DescribeUserProfile"
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

**Example 2: Allow connections only through interface endpoints using `aws:sourceVpce`**

The following policy allows connections only to those made through the interface endpoints specified by the `aws:sourceVpce` condition key\. For example, the first interface endpoint could allow access through the SageMaker Studio Control Panel\. The second interface endpoint could allow access through the SageMaker API\.

```
{
    "Id": "sagemaker-studio-example-2",
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Enable SageMaker Studio Access",
            "Effect": "Allow",
            "Action": [
                "sagemaker:CreatePresignedDomainUrl",
                "sagemaker:DescribeUserProfile"
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

This policy includes the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeUserProfile.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeUserProfile.html) action\. Typically you call `DescribeUserProfile` to make sure that the status of the user profile is `InService` before you try to connect to the domain\. For example:

```
aws sagemaker describe-user-profile \
    --domain-id domain-id \
    --user-profile-name profile-name
```

Response:

```
{
    "DomainId": "domain-id",
    "UserProfileArn": "arn:aws:sagemaker:us-west-2:acct-id:user-profile/domain-id/profile-name",
    "UserProfileName": "profile-name",
    "HomeEfsFileSystemUid": "200001",
    "Status": "InService",
    "LastModifiedTime": 1605418785.555,
    "CreationTime": 1605418477.297
}
```

```
aws sagemaker create-presigned-domain-url
    --domain-id domain-id \
    --user-profile-name profile-name
```

Response:

```
{
    "AuthorizedUrl": "https://domain-id.studio.us-west-2.sagemaker.aws/auth?token=AuthToken"
}
```

For both of these calls, if you are using a version of the AWS SDK that was released before August 13, 2018, you must specify the endpoint URL in the call\. For example, the following example shows a call to `create-presigned-domain-url`:

```
aws sagemaker create-presigned-domain-url
    --domain-id domain-id \
    --user-profile-name profile-name \
    --endpoint-url vpc-endpoint-id.api.sagemaker.Region.vpce.amazonaws.com
```

**Example 3: Allow connections from IP addresses using `aws:SourceIp` **

The following policy allows connections only from the specified range of IP addresses using the `aws:SourceIp` condition key\.

```
{
    "Id": "sagemaker-studio-example-3",
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Enable SageMaker Studio Access",
            "Effect": "Allow",
            "Action": [
                "sagemaker:CreatePresignedDomainUrl",
                "sagemaker:DescribeUserProfile"
            ],
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

**Example 4: Allow connections from IP addresses through an interface endpoint using `aws:VpcSourceIp`** 

If you are accessing SageMaker Studio through an interface endpoint, you can use the `aws:VpcSourceIp` condition key to allow connections only from the specified range of IP addresses within the subnet where you created the interface endpoint as shown in the following policy:

```
{
    "Id": "sagemaker-studio-example-4",
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Enable SageMaker Studio Access",
            "Effect": "Allow",
            "Action": [
                "sagemaker:CreatePresignedDomainUrl",
                "sagemaker:DescribeUserProfile"
            ],
            "Resource": "*",
            "Condition": {
                "IpAddress": {
                    "aws:VpcSourceIp": [
                        "192.0.2.0/24",
                        "203.0.113.0/24"
                    ]
                },
                "StringEquals": {
                    "aws:SourceVpc": "vpc-111bbaaa"
                }
            }
        }
    ]
}
```