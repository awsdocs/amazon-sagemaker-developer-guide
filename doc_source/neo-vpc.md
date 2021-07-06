# Give SageMaker Compilation Jobs Access to Resources in Your Amazon VPC<a name="neo-vpc"></a>

SageMaker Neo runs compilation jobs in an Amazon Virtual Private Cloud by default\. However, compilation jobs access AWS resources—such as the Amazon Simple Storage Service \(Amazon S3\) buckets where you store model artifacts—over the internet\.

To control access to your data, we recommend that you create a private VPC and configure it so that they aren't accessible over the internet\. For information about creating and configuring a VPC, see [Getting Started With Amazon VPC](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/getting-started-ipv4.html) in the *Amazon VPC User Guide*\. Using a VPC helps to protect your data because you can configure your VPC so that it is not connected to the internet\. For more information, see [VPC Flow Logs](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/flow-logs.html) in the *Amazon VPC User Guide*\.

You specify your private VPC configuration when you create compilation jobs by specifying subnets and security groups\. When you specify the subnets and security groups, SageMaker creates *elastic network interfaces* that are associated with your security groups in one of the subnets\. Network interfaces allow SageMaker Neo's compilation jobs to connect to resources in your VPC\. For information about network interfaces, see [Elastic Network Interfaces](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_ElasticNetworkInterfaces.html) in the *Amazon VPC User Guide*\.

**Note**  
For compilation jobs, you can configure only subnets with a default tenancy VPC in which your job runs on shared hardware\. For more information on the tenancy attribute for VPCs, see [Dedicated Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/dedicated-instance.html)\.

## Configure a Compilation Job for Amazon VPC Access<a name="neo-vpc-configure"></a>

To specify subnets and security groups in your private VPC, use the `VpcConfig` request parameter of the [ `CreateCompilationJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateCompilationJob.html) API, or provide this information when you create a compilation job in the SageMaker console\. SageMaker Neo uses this information to create network interfaces and attach them to your compilation jobs\. The network interfaces provide compilation jobs with a network connection within your VPC that is not connected to the internet\. They also enable your compilation job to connect to resources in your private VPC\. The following is an example of the `VpcConfig` parameter that you include in your call to `CreateCompilationJob`:

```
VpcConfig: {"Subnets": [
          "subnet-0123456789abcdef0",
          "subnet-0123456789abcdef1",
          "subnet-0123456789abcdef2"
          ],
      "SecurityGroupIds": [
          "sg-0123456789abcdef0"
          ]
        }
```

## Configure Your Private VPC for SageMaker Compilation<a name="neo-vpc-vpc"></a>

When configuring the private VPC for your SageMaker compilation jobs, use the following guidelines\. For information about setting up a VPC, see [Working with VPCs and Subnets](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/working-with-vpcs.html) in the *Amazon VPC User Guide*\.

**Topics**
+ [Ensure That Subnets Have Enough IP Addresses](#neo-vpc-ip)
+ [Create an Amazon S3 VPC Endpoint](#neo-vpc-s3)
+ [Use a Custom Endpoint Policy to Restrict Access to S3](#neo-vpc-policy)
+ [Configure Route Tables](#neo-vpc-route-table)
+ [Configure the VPC Security Group](#neo-vpc-groups)

### Ensure That Subnets Have Enough IP Addresses<a name="neo-vpc-ip"></a>

Your VPC subnets should have at least two private IP addresses for each instance in a compilation job\. For more information, see [VPC and Subnet Sizing for IPv4](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Subnets.html#vpc-sizing-ipv4) in the *Amazon VPC User Guide*\.

### Create an Amazon S3 VPC Endpoint<a name="neo-vpc-s3"></a>

If you configure your VPC to block access to the internet, SageMaker Neo can't connect to the Amazon S3 buckets that contain your models unless you create a VPC endpoint that allows access\. By creating a VPC endpoint, you allow your SageMaker Neo compilation jobs to access the buckets where you store your data and model artifacts \. We recommend that you also create a custom policy that allows only requests from your private VPC to access to your S3 buckets\. For more information, see [Endpoints for Amazon S3](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-endpoints-s3.html)\.

**To create an S3 VPC endpoint:**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Endpoints**, then choose **Create Endpoint**

1. For **Service Name**, search for **com\.amazonaws\.*region*\.s3**, where *region* is the name of the region where your VPC resides\.

1. Choose the **Gateway** type\.

1. For **VPC**, choose the VPC you want to use for this endpoint\.

1. For **Configure route tables**, select the route tables to be used by the endpoint\. The VPC service automatically adds a route to each route table you select that points any S3 traffic to the new endpoint\.

1. For **Policy**, choose **Full Access** to allow full access to the S3 service by any user or service within the VPC\. Choose **Custom** to restrict access further\. For information, see [Use a Custom Endpoint Policy to Restrict Access to S3](train-vpc.md#train-vpc-policy)\.

### Use a Custom Endpoint Policy to Restrict Access to S3<a name="neo-vpc-policy"></a>

The default endpoint policy allows full access to S3 for any user or service in your VPC\. To further restrict access to S3, create a custom endpoint policy\. For more information, see [Using Endpoint Policies for Amazon S3](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-s3.html#vpc-endpoints-policies-s3)\. You can also use a bucket policy to restrict access to your S3 buckets to only traffic that comes from your Amazon VPC\. For information, see [Using Amazon S3 Bucket Policies](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-s3.html#vpc-endpoints-s3-bucket-policies)\. The following is a sample customized policy:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Principal": {
                "AWS": "*"
            },
            "Action": "s3:GetObject",
            "Resource": [
                "arn:aws:s3:::your-sample-bucket",
                "arn:aws:s3:::your-sample-bucket/*"
            ],
            "Condition": {
                "StringNotEquals": {
                    "aws:SourceVpce": [
                        "vpce-01234567890123456"
                    ]
                }
            }
        }
    ]
}
```

#### Add Permissions for Compilation Job Running in a Amazon VPC to Custom IAM Policies<a name="neo-vpc-custom-iam"></a>

The `SageMakerFullAccess` managed policy includes the permissions that you need to use models configured for Amazon VPC access with an endpoint\. These permissions allow SageMaker Neo to create an elastic network interface and attach it to compilation job running in a Amazon VPC\. If you use your own IAM policy, you must add the following permissions to that policy to use models configured for Amazon VPC access\.

```
{"Version": "2012-10-17",
    "Statement": [
        {"Effect": "Allow",
            "Action": [
                "ec2:DescribeVpcEndpoints",
                "ec2:DescribeDhcpOptions",
                "ec2:DescribeVpcs",
                "ec2:DescribeSubnets",
                "ec2:DescribeSecurityGroups",
                "ec2:DescribeNetworkInterfaces",
                "ec2:DeleteNetworkInterfacePermission",
                "ec2:DeleteNetworkInterface",
                "ec2:CreateNetworkInterfacePermission",
                "ec2:CreateNetworkInterface",
                "ec2:ModifyNetworkInterfaceAttribute"
            ],
            "Resource": "*"
        }
    ]
}
```

For more information about the `SageMakerFullAccess` managed policy, see [`AmazonSageMakerFullAccess`](security-iam-awsmanpol.md#security-iam-awsmanpol-AmazonSageMakerFullAccess)\.

### Configure Route Tables<a name="neo-vpc-route-table"></a>

Use default DNS settings for your endpoint route table, so that standard Amazon S3 URLs \(for example, `http://s3-aws-region.amazonaws.com/MyBucket`\) resolve\. If you don't use default DNS settings, ensure that the URLs that you use to specify the locations of the data in your compilation jobs resolve by configuring the endpoint route tables\. For information about VPC endpoint route tables, see [Routing for Gateway Endpoints](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpce-gateway.html#vpc-endpoints-routing) in the *Amazon VPC User Guide*\.

### Configure the VPC Security Group<a name="neo-vpc-groups"></a>

In your security group for the compilation job, you must allow outbound communication to your Amazon S3 Amazon VPC endpoints and the subnet CIDR ranges used for the compilation job\. For information, see [Security Group Rules](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_SecurityGroups.html#SecurityGroupRules) and [Control access to services with Amazon VPC endpoints](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-endpoints-access.html)\.