# Protect Data in Batch Transform Jobs by Using an Amazon Virtual Private Cloud<a name="batch-vpc"></a>

Amazon SageMaker runs batch transform jobs in an Amazon Virtual Private Cloud by default\. However, model containers access AWS resources—such as the Amazon S3 buckets where you store your data and model artifacts—over the internet\.

To control access to your model containers and data, we recommend creating a private VPC and configuring it so that it is not connected to the internet\. For information about creating and configuring a VPC, see [Getting Started With Amazon VPC](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/getting-started-ipv4.html) in the *Amazon VPC User Guide*\.

With a VPC, you can monitor network traffic to and from your model containers by using VPC flow logs\. For more information, see [VPC Flow Logs](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/flow-logs.html) in the *Amazon VPC User Guide*\.

When you specify subnets and security groups, Amazon SageMaker creates elastic network interfaces \(ENIs\) and associates them with your security groups in one of the subnets\. *ENIs* allow model containers to connect to resources in your VPC\. For information about ENIs, see [Elastic Network Interfaces](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_ElasticNetworkInterfaces.html) in the *Amazon VPC User Guide*\.

## Configure a Batch Transform Job for Amazon VPC Access<a name="batch-vpc-configure"></a>

To specify subnets and security groups in your private VPC, use the `VpcConfig` request parameter of the [CreateModel](API_CreateModel.md) API and provide this model when you create a batch transform job in the Amazon SageMaker console\. Amazon SageMaker uses this information to create ENIs and attach them to your model containers\. The ENIs provide your model containers with a network connection within your VPC that is not connected to the internet\. They also enable your batch transform job to connect to resources in your private VPC\.

The following is an example of the `VpcConfig` parameter that you include in your call to `CreateModel`:

```
VpcConfig: {
      "Subnets": [
          "subnet-0123456789abcdef0",
          "subnet-0123456789abcdef1",
          "subnet-0123456789abcdef2"
          ],
          "SecurityGroupIds": [
              "sg-0123456789abcdef0"
              ]
            }
```

**Note**  
For training jobs, you can only configure subnets with a default tenancy VPC in which your instance runs on shared hardware\. For more information on the tenancy attribute for VPCs, see [Dedicated Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/dedicated-instance.html)\.

## Configure Your Private VPC for Amazon SageMaker Batch Transform<a name="batch-vpc-vpc"></a>

When configuring the private VPC for your Amazon SageMaker batch transform jobs, use the following guidelines\. For information about setting up a VPC, see [Working with VPCs and Subnets](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/working-with-vpcs.html) in the *Amazon VPC User Guide*\.

### Ensure That Subnets Have Enough IP Addresses<a name="batch-vpc-ip"></a>

Your VPC subnets should have at least two private IP addresses for each instance in a batch transform job\. For more information, see [VPC and Subnet Sizing for IPv4](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Subnets.html#vpc-sizing-ipv4) in the *Amazon VPC User Guide*\.

### Create an Amazon S3 VPC Endpoint<a name="batch-vpc-s3"></a>

If you configure your VPC so that model containers don't have access to the internet, they can't connect to the Amazon S3 buckets that contain your data unless you create a VPC endpoint that allows access\. By creating a VPC endpoint, you allow your model containers to access the buckets where you store your data and model artifacts\. We recommend that you also create a custom policy that allows only requests from your private VPC to access to your S3 buckets\. For more information, see [Endpoints for Amazon S3](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-endpoints-s3.html)\.

The following policy allows access to S3 buckets\. Edit this policy to allow access only the resources that your batch transform job needs\.

```
{
    "Version": "2008-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject",
                "s3:PutObject",
                "s3:ListBucket",
                "s3:GetBucketLocation",
                "s3:DeleteObject",
                "s3:ListMultipartUploadParts",
                "s3:AbortMultipartUpload"
            ],
            "Resource": "*"
        }
    ]
}
```

Use default DNS settings for your endpoint route table, so that standard Amazon S3 URLs \(for example, `http://s3-aws-region.amazonaws.com/MyBucket`\) resolve\. If you don't use default DNS settings, ensure that the URLs that you use to specify the locations of the data in your batch transform jobs resolve by configuring the endpoint route tables\. For information about VPC endpoint route tables, see [Routing for Gateway Endpoints](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpce-gateway.html#vpc-endpoints-routing) in the *Amazon VPC User Guide*\.

The default endpoint policy allows users to install packages from the Amazon Linux and Amazon Linux 2 repositories on the model container\. If you don't want users to install packages from them, create a custom endpoint policy that explicitly denies access to the Amazon Linux and Amazon Linux 2 repositories\. The following is an example of a policy that denies access to these repositories:

```
{ 
    "Statement": [ 
      { 
        "Sid": "AmazonLinuxAMIRepositoryAccess",
        "Principal": "*",
        "Action": [ 
            "s3:GetObject" 
        ],
        "Effect": "Deny",
        "Resource": [
            "arn:aws:s3:::packages.*.amazonaws.com/*",
            "arn:aws:s3:::repo.*.amazonaws.com/*"
        ] 
      } 
    ] 
} 

{ 
    "Statement": [ 
        { "Sid": "AmazonLinux2AMIRepositoryAccess",
          "Principal": "*",
          "Action": [ 
              "s3:GetObject" 
              ],
          "Effect": "Deny",
          "Resource": [
              "arn:aws:s3:::amazonlinux.*.amazonaws.com/*" 
              ] 
         } 
    ] 
}
```

### Configure the VPC Security Group<a name="batch-vpc-groups"></a>

In distributed batch transform, you must allow communication between the different containers in the same batch transform job\. To do that, configure a rule for your security group that allows inbound connections between members of the same security group\. For information, see [Security Group Rules](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_SecurityGroups.html#SecurityGroupRules)\.

### Connect to Resources Outside Your VPC<a name="batch-vpc-nat"></a>

If you configure your VPC so that it doesn't have internet access, batch transform jobs that use that VPC do not have access to resources outside your VPC\. If your batch transform jobs needs access to resources outside your VPC, provide access with one of the following options:
+ If your batch transform job needs access to an AWS service that supports interface VPC endpoints, create an endpoint to connect to that service\. For a list of services that support interface endpoints, see [VPC Endpoints](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-endpoints.html) in the *Amazon VPC User Guide*\. For information about creating an interface VPC endpoint, see [Interface VPC Endpoints \(AWS PrivateLink\)](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpce-interface.html) in the *Amazon VPC User Guide*\.
+ If your batch transform job needs access to an AWS service that doesn't support interface VPC endpoints or to a resource outside of AWS, create a NAT gateway and configure your security groups to allow outbound connections\. For information about setting up a NAT gateway for your VPC, see [Scenario 2: VPC with Public and Private Subnets \(NAT\)](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Scenario2.html) in the *Amazon Virtual Private Cloud User Guide*\.