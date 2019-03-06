# Protect Endpoints by Using an Amazon Virtual Private Cloud<a name="host-vpc"></a>

Amazon SageMaker hosts models in an Amazon Virtual Private Cloud by default\. However, models access AWS resources—such as the Amazon S3 buckets that you use to store model artifacts—over the internet\.

To avoid making your data and model containers accessible over internet, we recommend that you create a private VPC and configure it to control access to them\. For information about creating and configuring a VPC, see [Getting Started With Amazon VPC](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/getting-started-ipv4.html) in the *Amazon VPC User Guide*\. Using a VPC helps to protect your model containers and data because you can configure the VPC so that it isn't connected to the internet\. Using a VPC also allows you to monitor all network traffic in and out of your model containers by using VPC flow logs\. For more information, see [VPC Flow Logs](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/flow-logs.html) in the *Amazon VPC User Guide*\.

You specify your VPC configuration when you create a model by specifying subnets and security groups\. When you specify your subnets and security groups, Amazon SageMaker creates *elastic network interfaces* \(ENIs\) that are associated with your security groups in one of the specified subnets\. ENIs allow your model containers to connect to resources in your VPC\. For information about ENIs, see [Elastic Network Interfaces](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_ElasticNetworkInterfaces.html) in the *Amazon VPC User Guide*\.

## Configure a Model for Amazon VPC Access<a name="host-vpc-configure"></a>

To specify subnets and security groups in your private VPC, use the `VpcConfig` request parameter of the [CreateModel](API_CreateModel.md) API or when you create a model in the Amazon SageMaker console\. Amazon SageMaker uses this information to create ENIs and then attaches them to your model containers\. The ENIs provide your model containers with a network connection within your VPC that is not connected to the internet\. It also enables your model to connect to resources in your private VPC\.

**Note**  
You must create at least two subnets in different availability zones in your private VPC, even if you have only one hosting instance\.

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

## Configure Your Private VPC for Amazon SageMaker Hosting<a name="host-vpc-vpc"></a>

Use the following guidelines to configure a private VPC for your Amazon SageMaker hosting jobs \. For information about setting up a VPC, see [Working with VPCs and Subnets](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/working-with-vpcs.html) in the *Amazon VPC User Guide*\.

### Ensure That Subnets Have Enough IP Addresses<a name="host-vpc-ip"></a>

Your VPC subnets should have at least two available private IP addresses for each instance for a hosted model\. For more information, see [VPC and Subnet Sizing for IPv4](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Subnets.html#vpc-sizing-ipv4) in the *Amazon VPC User Guide*\.

### Create an Amazon S3 VPC Endpoint<a name="host-vpc-s3"></a>

If you configure your VPC so that model containers don't have access to the internet, models that use that VPC can't connect to the Amazon S3 buckets that contain your data unless you create a VPC endpoint for access to the buckets\. We recommend that you create a VPC endpoint with a custom policy that restricts access to your S3 buckets to only requests from your VPC\. For more information, see [Endpoints for Amazon S3](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-endpoints-s3.html) in the *Amazon VPC User Guide*\.

The following policy allows access to S3 buckets\. Edit this policy to allow access only the resources that your model needs\.

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

Use default DNS settings for your endpoint route table, so that standard Amazon S3 URLs \(for example, `http://s3-aws-region.amazonaws.com/MyBucket`\) resolve\. If you don't use default DNS settings, ensure that the URLs that you use to specify the locations of the data in your models resolve by configuring the endpoint route tables\. For information about VPC endpoint route tables, see [Routing for Gateway Endpoints](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpce-gateway.html#vpc-endpoints-routing) in the *Amazon VPC User Guide*\.

**Note**  
The default endpoint policy allows users to install packages from the Amazon Linux and Amazon Linux 2 repositories on the model container\. If you don't want users to install packages from that repository, create a custom endpoint policy that explicitly denies access to the Amazon Linux and Amazon Linux 2 repositories\. The following is an example of a policy that denies access to these repositories:  

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

### Connect Resources Outside Your VPC<a name="host-vpc-nat"></a>

If you configure your VPC so that it doesn't have internet access, models that use that VPC do not have access to resources outside your VPC\. If your model needs access to resources on the internet, provide access with one of the following options:
+ If your model needs access to an AWS service that supports interface VPC endpoints, create an endpoint to connect to that service\. For a list of services that support interface endpoints, see [VPC Endpoints](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-endpoints.html) in the *Amazon VPC User Guide*\. For information about creating an interface VPC endpoint, see [Interface VPC Endpoints \(AWS PrivateLink\)](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpce-interface.html) in the *Amazon VPC User Guide*\.
+ If your model needs access to an AWS service that doesn't support interface VPC endpoints or to a resource outside of AWS, create a NAT gateway and configure your security groups to allow outbound connections\. For information about setting up a NAT gateway for your VPC, see [Scenario 2: VPC with Public and Private Subnets \(NAT\)](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Scenario2.html) in the *Amazon Virtual Private Cloud User Guide*\.