# Give SageMaker Training Jobs Access to Resources in Your Amazon VPC<a name="train-vpc"></a>

SageMaker runs training jobs in an Amazon Virtual Private Cloud by default\. However, training containers access AWS resources—such as the Amazon S3 buckets where you store training data and model artifacts—over the internet\.

To control access to your data and training containers, we recommend that you create a private VPC and configure it so that they aren't accessible over the internet\. For information about creating and configuring a VPC, see [Getting Started With Amazon VPC](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/getting-started-ipv4.html) in the *Amazon VPC User Guide*\. Using a VPC helps to protect your training containers and data because you can configure your VPC so that it is not connected to the internet\. Using a VPC also allows you to monitor all network traffic in and out of your training containers by using VPC flow logs\. For more information, see [VPC Flow Logs](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/flow-logs.html) in the *Amazon VPC User Guide*\.

You specify your private VPC configuration when you create training jobs by specifying subnets and security groups\. When you specify the subnets and security groups, SageMaker creates *elastic network interfaces* \(ENIs\) that are associated with your security groups in one of the subnets\. ENIs allow your training containers to connect to resources in your VPC\. For information about ENIs, see [Elastic Network Interfaces](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_ElasticNetworkInterfaces.html) in the *Amazon VPC User Guide*\.

**Note**  
For training jobs, you can configure only subnets with a default tenancy VPC in which your instance runs on shared hardware\. For more information on the tenancy attribute for VPCs, see [Dedicated Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/dedicated-instance.html)\.

## Configure a Training Job for Amazon VPC Access<a name="train-vpc-configure"></a>

To specify subnets and security groups in your private VPC, use the `VpcConfig` request parameter of the [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) API, or provide this information when you create a training job in the SageMaker console\. SageMaker uses this information to create ENIs and attach them to your training containers\. The ENIs provide your training containers with a network connection within your VPC that is not connected to the internet\. They also enable your training job to connect to resources in your private VPC\.

The following is an example of the `VpcConfig` parameter that you include in your call to `CreateTrainingJob`:

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

## Configure Your Private VPC for SageMaker Training<a name="train-vpc-vpc"></a>

When configuring the private VPC for your SageMaker training jobs, use the following guidelines\. For information about setting up a VPC, see [Working with VPCs and Subnets](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/working-with-vpcs.html) in the *Amazon VPC User Guide*\.

**Topics**
+ [Ensure That Subnets Have Enough IP Addresses](#train-vpc-ip)
+ [Create an Amazon S3 VPC Endpoint](#train-vpc-s3)
+ [Use a Custom Endpoint Policy to Restrict Access to S3](#train-vpc-policy)
+ [Configure Route Tables](#train-vpc-route-table)
+ [Configure the VPC Security Group](#train-vpc-groups)
+ [Connect to Resources Outside Your VPC](#train-vpc-nat)

### Ensure That Subnets Have Enough IP Addresses<a name="train-vpc-ip"></a>

Your VPC subnets should have at least two private IP addresses for each instance in a training job\. For more information, see [VPC and Subnet Sizing for IPv4](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Subnets.html#vpc-sizing-ipv4) in the *Amazon VPC User Guide*\.

### Create an Amazon S3 VPC Endpoint<a name="train-vpc-s3"></a>

If you configure your VPC so that training containers don't have access to the internet, they can't connect to the Amazon S3 buckets that contain your training data unless you create a VPC endpoint that allows access\. By creating a VPC endpoint, you allow your training containers to access the buckets where you store your data and model artifacts \. We recommend that you also create a custom policy that allows only requests from your private VPC to access to your S3 buckets\. For more information, see [Endpoints for Amazon S3](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-endpoints-s3.html)\.

**To create an S3 VPC endpoint:**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Endpoints**, then choose **Create Endpoint**

1. For **Service Name**, choose **com\.amazonaws\.*region*\.s3**, where *region* is the name of the region where your VPC resides\.

1. For **VPC**, choose the VPC you want to use for this endpoint\.

1. For **Configure route tables**, select the route tables to be used by the endpoint\. The VPC service automatically adds a route to each route table you select that points any S3 traffic to the new endpoint\.

1. For **Policy**, choose **Full Access** to allow full access to the S3 service by any user or service within the VPC\. Choose **Custom** to restrict access further\. For information, see [Use a Custom Endpoint Policy to Restrict Access to S3](#train-vpc-policy)\.

### Use a Custom Endpoint Policy to Restrict Access to S3<a name="train-vpc-policy"></a>

The default endpoint policy allows full access to S3 for any user or service in your VPC\. To further restrict access to S3, create a custom endpoint policy\. For more information, see [Using Endpoint Policies for Amazon S3](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-s3.html#vpc-endpoints-policies-s3)\. You can also use a bucket policy to restrict access to your S3 buckets to only traffic that comes from your Amazon VPC\. For information, see [Using Amazon S3 Bucket Policies](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-s3.html#vpc-endpoints-s3-bucket-policies)\.

#### Restrict Package Installation on the Training Container<a name="train-vpc-policy-repos"></a>

The default endpoint policy allows users to install packages from the Amazon Linux and Amazon Linux 2 repositories on the training container\. If you don't want users to install packages from that repository, create a custom endpoint policy that explicitly denies access to the Amazon Linux and Amazon Linux 2 repositories\. The following is an example of a policy that denies access to these repositories:

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

### Configure Route Tables<a name="train-vpc-route-table"></a>

Use default DNS settings for your endpoint route table, so that standard Amazon S3 URLs \(for example, `http://s3-aws-region.amazonaws.com/MyBucket`\) resolve\. If you don't use default DNS settings, ensure that the URLs that you use to specify the locations of the data in your training jobs resolve by configuring the endpoint route tables\. For information about VPC endpoint route tables, see [Routing for Gateway Endpoints](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpce-gateway.html#vpc-endpoints-routing) in the *Amazon VPC User Guide*\.

### Configure the VPC Security Group<a name="train-vpc-groups"></a>

In distributed training, you must allow communication between the different containers in the same training job\. To do that, configure a rule for your security group that allows inbound connections between members of the same security group For information, see [Security Group Rules](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_SecurityGroups.html#SecurityGroupRules)\.

### Connect to Resources Outside Your VPC<a name="train-vpc-nat"></a>

If you configure your VPC so that it doesn't have internet access, training jobs that use that VPC do not have access to resources outside your VPC\. If your training job needs access to resources outside your VPC, provide access with one of the following options:
+ If your training job needs access to an AWS service that supports interface VPC endpoints, create an endpoint to connect to that service\. For a list of services that support interface endpoints, see [VPC Endpoints](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-endpoints.html) in the *Amazon VPC User Guide*\. For information about creating an interface VPC endpoint, see [Interface VPC Endpoints \(AWS PrivateLink\)](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpce-interface.html) in the *Amazon VPC User Guide*\.
+ If your training job needs access to an AWS service that doesn't support interface VPC endpoints or to a resource outside of AWS, create a NAT gateway and configure your security groups to allow outbound connections\. For information about setting up a NAT gateway for your VPC, see [Scenario 2: VPC with Public and Private Subnets \(NAT\)](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Scenario2.html) in the *Amazon Virtual Private Cloud User Guide*\.