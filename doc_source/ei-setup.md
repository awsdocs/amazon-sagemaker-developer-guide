# Set Up to Use EI<a name="ei-setup"></a>

Use the instructions in this topic only if one of the following applies to you:
+ You want to use a customized role or permission policy\.
+ You want to use a VPC for your hosted model or notebook instance\.

**Note**  
If you already have an execution role that has the `AmazonSageMakerFullAccess` managed policy attached \(this is true for any IAM role that you create when you create a notebook instance, training job, or model in the console\) and you are not connecting to an EI model or notebook instance in a VPC, you do not need to make any of these changes to use EI in Amazon SageMaker\.

**Topics**
+ [Set Up Required Permissions](#ei-setup-permissions)
+ [Use a Custom VPC to Connect to EI](#ei-setup-custom-vpc)

## Set Up Required Permissions<a name="ei-setup-permissions"></a>

To use EI in SageMaker, the role that you use to open a notebook instance or create a deployable model must have a policy with the required permissions attached\. You can attach the `AmazonSageMakerFullAccess` managed policy, which contains the required permissions, to the role, or you can add a custom policy that has the required permissions\. For information about creating an IAM role, see [Creating a Role for an AWS Service \(Console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html#roles-creatingrole-service-console) in the *AWS Identity and Access Management User Guide*\. For information about attaching a policy to a role, see [Adding and Removing IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html) \.

Add these permissions specifically for connecting EI in an IAM policy\.

```
{
    "Effect": "Allow",
    "Action": [
        "elastic-inference:Connect",
        "ec2:DescribeVpcEndpoints"
    ],
    "Resource": "*"
}
```

The following IAM policy is the complete list of required permissions to use EI in SageMaker\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "elastic-inference:Connect",
                "ec2:DescribeVpcEndpoints"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "sagemaker:*"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ecr:GetAuthorizationToken",
                "ecr:GetDownloadUrlForLayer",
                "ecr:BatchGetImage",
                "ecr:BatchCheckLayerAvailability",
                "cloudwatch:PutMetricData",
                "cloudwatch:PutMetricAlarm",
                "cloudwatch:DescribeAlarms",
                "cloudwatch:DeleteAlarms",
                "ec2:CreateNetworkInterface",
                "ec2:CreateNetworkInterfacePermission",
                "ec2:DeleteNetworkInterface",
                "ec2:DeleteNetworkInterfacePermission",
                "ec2:DescribeNetworkInterfaces",
                "ec2:DescribeVpcs",
                "ec2:DescribeDhcpOptions",
                "ec2:DescribeSubnets",
                "ec2:DescribeSecurityGroups",
                "application-autoscaling:DeleteScalingPolicy",
                "application-autoscaling:DeleteScheduledAction",
                "application-autoscaling:DeregisterScalableTarget",
                "application-autoscaling:DescribeScalableTargets",
                "application-autoscaling:DescribeScalingActivities",
                "application-autoscaling:DescribeScalingPolicies",
                "application-autoscaling:DescribeScheduledActions",
                "application-autoscaling:PutScalingPolicy",
                "application-autoscaling:PutScheduledAction",
                "application-autoscaling:RegisterScalableTarget",
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:DescribeLogStreams",
                "logs:GetLogEvents",
                "logs:PutLogEvents"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject",
                "s3:DeleteObject"
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
                "s3:CreateBucket",
                "s3:GetBucketLocation",
                "s3:ListBucket",
                "s3:ListAllMyBuckets"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": "*",
            "Condition": {
                "StringEqualsIgnoreCase": {
                    "s3:ExistingObjectTag/SageMaker": "true"
                }
            }
        },
        {
            "Action": "iam:CreateServiceLinkedRole",
            "Effect": "Allow",
            "Resource": "arn:aws:iam::*:role/aws-service-role/sagemaker.application-autoscaling.amazonaws.com/AWSServiceRoleForApplicationAutoScaling_SageMakerEndpoint",
            "Condition": {
                "StringLike": {
                    "iam:AWSServiceName": "sagemaker.application-autoscaling.amazonaws.com"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "iam:PassRole"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "iam:PassedToService": "sagemaker.amazonaws.com"
                }
            }
        }
    ]
}
```

## Use a Custom VPC to Connect to EI<a name="ei-setup-custom-vpc"></a>

To use EI with SageMaker in a VPC, you need to create and configure two security groups, and set up a PrivateLink VPC interface endpoint\. EI uses VPC interface endpoint to communicate with SageMaker endpoints in your VPC\. The security groups you create are used to connect to the VPC interface endpoint\.

### Set up Security Groups to Connect to EI<a name="ei-setup-security-groups"></a>

To use EI within a VPC, you need to create two security groups:
+ A security group to control access to the VPC interface endpoint that you will set up for EI\.
+ A security group that allows SageMaker to call into the first security group\.

**To configure the two security groups**

1. Create a security group with no outbound connections\. You will attach this to the VPC endpoint interface you create in the next section\.

1. Create a second security group with no inbound connections, but with an outbound connection to the first security group\.

1. Edit the first security group to allow inbound connections only to the second security group an all outbound connections\.

For more information about VPC security groups, see [Security Groups for Your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html) in the *Amazon Virtual Private Cloud User Guide*\.

### Set up a VPC Interface Endpoint to Connect to EI<a name="ei-setup-privatelink"></a>

To use EI with SageMaker in a custom VPC, you need to set up a VPC interface endpoint \(PrivateLink\) for the EI service\.
+ Set up a VPC interface endpoint \(PrivateLink\) for the EI\. Follow the instructions at [Creating an Interface Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint)\. In the list of services, choose **com\.amazonaws\.<region>\.elastic\-inference\.runtime**\. For **Security group**, make sure you select the first security group you created in the previous section to the endpoint\.
+ When you set up the interface endpoint, choose all of the Availability Zones where EI is available\. EI fails if you do not set up at least two Availability Zones\. For information about VPC subnets, see [VPCs and Subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html)\.