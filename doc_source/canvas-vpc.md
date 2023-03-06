# Configure Amazon SageMaker Canvas in a VPC without internet access<a name="canvas-vpc"></a>

The Amazon SageMaker Canvas application runs in a container in an AWS managed Amazon Virtual Private Cloud \(VPC\)\. If you want to further control access to your resources or run SageMaker Canvas without public internet access, you can configure your Amazon SageMaker Domain and VPC settings\. Within your own VPC, you can configure settings such as security groups \(virtual firewalls that control inbound and outbound traffic from Amazon EC2 instances\) and subnets \(ranges of IP addresses in your VPC\)\. To learn more about VPCs, see [How Amazon VPC works](https://docs.aws.amazon.com/vpc/latest/userguide/how-it-works.html)\.

When the SageMaker Canvas application is running in the AWS managed VPC, it can interact with other AWS services using either an internet connection or through VPC endpoints created in a customer\-managed VPC \(without public internet access\)\. SageMaker Canvas applications can access these VPC endpoints through a Studio\-created network interface that provides connectivity to the customer\-managed VPC\. The default behavior of the SageMaker Canvas application is to have internet access\. When using an internet connection, the containers for the preceding jobs access AWS resources over the internet, such as the Amazon S3 buckets where you store training data and model artifacts\.

However, if you have security requirements to control access to your data and job containers, we recommend that you configure SageMaker Canvas and your VPC so that your data and containers aren’t accessible over the internet\. SageMaker uses the VPC configuration settings you specify when setting up your Domain for SageMaker Canvas\.

If you want to configure your SageMaker Canvas application without internet access, you must configure your VPC settings when you onboard to [Amazon SageMaker Domain](gs-studio-onboard.md), set up VPC endpoints, and grant the necessary AWS Identity and Access Management permissions\. For information about configuring a VPC in Amazon SageMaker, see [Choose an Amazon VPC](onboard-vpc.md)\. The following sections describe how to run SageMaker Canvas in a VPC without public internet access\.

## Configure Amazon SageMaker Canvas in a VPC without internet access<a name="canvas-vpc-configure"></a>

You can send traffic from SageMaker Canvas to other AWS services through your own VPC\. If your own VPC doesn't have public internet access and you've set up your Domain in **VPC only** mode, then SageMaker Canvas won't have public internet access as well\. This includes all requests, such as accessing datasets in Amazon S3 or training jobs for standard builds, and the requests go through VPC endpoints in your VPC instead of the public internet\. When you onboard to Domain and [Choose an Amazon VPC](onboard-vpc.md), you can specify your own VPC as the default VPC for the Domain, along with your desired security group and subnet settings\. Then, SageMaker creates a network interface in your VPC that SageMaker Canvas uses to access VPC endpoints in your VPC\. Note that the security group and subnet settings are set after you finish onboarding to Domain\.

When onboarding to Domain, if you choose **Public internet only** as the network access type, the VPC is SageMaker managed and allows internet access\.

You can change this behavior by choosing **VPC only** so that SageMaker sends all traffic to a network interface that SageMaker creates in your specified VPC\. When you choose this option, you must provide the subnets, security groups, and VPC endpoints that are necessary to communicate with the SageMaker API and SageMaker Runtime, and various AWS services, such as Amazon S3 and Amazon CloudWatch, that are used by SageMaker Canvas\. Note that you can only import data from Amazon S3 buckets located in the same Region as your VPC\.

The following procedures show how you can configure these settings to use SageMaker Canvas without the internet\.

### Step 1: Onboard to Amazon SageMaker Domain<a name="canvas-vpc-configure-onboard"></a>

To send SageMaker Canvas traffic to a network interface in your own VPC instead of over the internet, specify the VPC you want to use when onboarding to [Amazon SageMaker Domain](gs-studio-onboard.md)\. You must also specify at least two subnets in your VPC that SageMaker can use\. Choose **Standard setup** and do the following procedure when configuring the **Network and Storage Section** for the Domain\.

1. Select your desired **VPC**\.

1. Choose two or more **Subnets**\. If you don’t specify the subnets, SageMaker uses all of the subnets in the VPC\.

1. Choose one or more **Security group\(s\)**\.

1. Choose **VPC Only** to turn off direct internet access in the AWS managed VPC where SageMaker Canvas is hosted\.

After disabling internet access, finish the onboarding process to set up your Domain\. For more information about the VPC settings for Amazon SageMaker Domain, see [Choose an Amazon VPC](onboard-vpc.md)\.

### Step 2: Configure VPC endpoints and access<a name="canvas-vpc-configure-endpoints"></a>

**Note**  
In order to configure Canvas in your own VPC, you must enable private DNS hostnames for your VPC endpoints\. For more information, see [Connect to SageMaker Through a VPC Interface Endpoint](https://docs.aws.amazon.com/sagemaker/latest/dg/interface-vpc-endpoint.html)\.

SageMaker Canvas only accesses other AWS services to manage and store data for its functionality\. For example, it connects to Amazon Redshift if your users access an Amazon Redshift database\. It can connect to an AWS service such as Amazon Redshift using an internet connection or a VPC endpoint\. Use VPC endpoints if you want to set up connections from your VPC to AWS services that don't use the public internet\.

A VPC endpoint creates a private connection to an AWS service that uses a networking path that is isolated from the public internet\. For example, if you set up access to Amazon S3 using a VPC endpoint from your own VPC, then the SageMaker Canvas application can access Amazon S3 by going through the network interface in your VPC and then through the VPC endpoint that connects to Amazon S3\. The communication between SageMaker Canvas and Amazon S3 is private\.

For more information about configuring VPC endpoints for your VPC, see [AWS PrivateLink](https://docs.aws.amazon.com/vpc/latest/privatelink/what-is-privatelink.html)\.

The following are the VPC endpoints for each service you can use with SageMaker Canvas:


| Service | Endpoint | Endpoint type | 
| --- | --- | --- | 
|  Amazon Athena  |  com\.amazonaws\.*Region*\.athena  | Interface | 
|  Amazon SageMaker  |  com\.amazonaws\.*Region*\.sagemaker\.api com\.amazonaws\.*Region*\.sagemaker\.runtime com\.amazonaws\.*Region*\.notebook  | Interface | 
|  AWS Security Token Service  |  com\.amazonaws\.*Region*\.sts  | Interface | 
|  Amazon Elastic Container Registry \(Amazon ECR\)  |  com\.amazonaws\.*Region*\.ecr\.api com\.amazonaws\.*Region*\.ecr\.dkr  | Interface | 
|  Amazon Elastic Compute Cloud \(Amazon EC2\)  |  com\.amazonaws\.*Region*\.ec2  | Interface | 
|  Amazon Simple Storage Service \(Amazon S3\)  |  com\.amazonaws\.*Region*\.s3  | Gateway | 
|  Amazon Redshift  |  com\.amazonaws\.*Region*\.redshift\-data  | Interface | 
|  AWS Secrets Manager  |  com\.amazonaws\.*Region*\.secretsmanager  | Interface | 
|  AWS Systems Manager  |  com\.amazonaws\.*Region*\.ssm  | Interface | 
|  Amazon CloudWatch  |  com\.amazonaws\.*Region*\.monitoring  | Interface | 
|  Amazon CloudWatch Logs  |  com\.amazonaws\.*Region*\.logs  | Interface | 
|  Amazon Forecast  |  com\.amazonaws\.*Region*\.forecast com\.amazonaws\.*Region*\.forecastquery  | Interface | 

You must also add the following endpoint policy for Amazon S3 to control AWS principal access to your VPC endpoint\. For information about how to update your VPC endpoint policy, see [Control access to VPC endpoints using endpoint policies](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints-access.html)\.

```
{
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject",
                "s3:DeleteObject",
                "s3:CreateBucket",
                "s3:GetBucketCors",
                "s3:GetBucketLocation"
            ],
            "Resource": [
                "arn:aws:s3:::*SageMaker*",
                "arn:aws:s3:::*Sagemaker*",
                "arn:aws:s3:::*sagemaker*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket",
                "s3:ListAllMyBuckets"
            ],
            "Resource": "*"
        }
```

### Step 3: Grant IAM permissions<a name="canvas-vpc-configure-permissions"></a>

The SageMaker Canvas user must have the necessary AWS Identity and Access Management permissions to allow connection to the VPC endpoints\. The IAM role to which you give permissions must be the same one you used when onboarding to Amazon SageMaker Domain\. You can attach the SageMaker managed `AmazonSageMakerFullAccess` policy to the IAM role for the user to give the user the required permissions\. If you require more restrictive IAM permissions and use custom policies instead, then give the user’s role the `ec2:DescribeVpcEndpointServices` permission\. SageMaker Canvas requires these permissions to verify the existence of the required VPC endpoints for standard build jobs\. If it detects these VPC endpoints, then standard build jobs run by default in your VPC\. Otherwise, they will run in the default AWS managed VPC\.

For instructions on how to attach the `AmazonSageMakerFullAccess` IAM policy to your user’s IAM role, see [Adding and removing IAM identity permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html)\.

To grant your user’s IAM role the granular `ec2:DescribeVpcEndpointServices` permission, use the following procedure\.

1. Sign in to the AWS Management Console and open the [IAM console](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\.

1. In the list, choose the name of the role to which you want to grant permissions\.

1. Choose the **Permissions** tab\.

1. Choose **Add permissions** and then choose **Create inline policy**\.

1. Choose the **JSON** tab and enter the following policy, which grants the `ec2:DescribeVpcEndpointServices` permission:

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "VisualEditor0",
               "Effect": "Allow",
               "Action": "ec2:DescribeVpcEndpointServices",
               "Resource": "*"
           }
       ]
   }
   ```

1. Choose **Review policy**, and then enter a **Name** for the policy \(for example, `VPCEndpointPermissions`\)\.

1. Choose **Create policy**\.

The user’s IAM role should now have permissions to access the VPC endpoints configured in your VPC\.

### \(Optional\) Step 4: Override security group settings for specific users<a name="canvas-vpc-configure-override"></a>

If you are an administrator, you might want different users to have different VPC settings, or user\-specific VPC settings\. When you override the default VPC’s security group settings for a specific user, these settings are passed on to the SageMaker Canvas application for that user\.

You can override the security groups that a specific user has access to in your VPC when you set up a new user profile in Studio\. You can use the [CreateUserProfile](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateUserProfile.html) SageMaker API call \(or [create\_user\_profile](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_user_profile) with the [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html)\), and then in the `UserSettings`, you can specify the `SecurityGroups` for the user\.