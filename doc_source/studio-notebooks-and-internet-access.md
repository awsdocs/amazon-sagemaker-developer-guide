# Connect SageMaker Studio Notebooks to Resources in a VPC<a name="studio-notebooks-and-internet-access"></a>

The following topic gives information on how to connect Studio Notebooks to resources in a VPC\.

## Default communication with the internet<a name="studio-notebooks-and-internet-access-default"></a>

By default, SageMaker Studio provides a network interface that allows communication with the internet through a VPC managed by SageMaker\. Traffic to AWS services like Amazon S3 and CloudWatch goes through an internet gateway, as does traffic that accesses the SageMaker API and SageMaker runtime\. Traffic between the domain and your Amazon EFS volume goes through the VPC that you specified when you onboarded to Studio or called the [CreateDomain](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateDomain.html) API\. The following diagram shows the default configuration\.

![\[Diagram of SageMaker Studio VPC using direct internet access\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-vpc-internet.png)

## `VPC only` communication with the internet<a name="studio-notebooks-and-internet-access-vpc"></a>

To prevent SageMaker from providing internet access to your Studio notebooks, you can disable internet access by specifying the `VPC only` network access type when you [onboard to Studio](https://docs.aws.amazon.com/sagemaker/latest/dg/onboard-vpc.html) or call the [CreateDomain](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateDomain.html) API\. As a result, you won't be able to run a Studio notebook unless your VPC has an interface endpoint to the SageMaker API and runtime, or a NAT gateway with internet access, and your security groups allow outbound connections\. The following diagram shows a configuration for using VPC\-only mode\.

![\[Diagram of SageMaker Studio VPC using VPC-only mode\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-vpc-private.png)

### Requirements to use `VPC only` mode<a name="studio-notebooks-and-internet-access-vpc-requirements"></a>

When you choose `VpcOnly`, follow these steps:

1. Ensure your subnets have one IP address for each instance\. For more information, see [VPC and subnet sizing for IPv4](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html#vpc-sizing-ipv4)\.
**Note**  
You can configure only subnets with a default tenancy VPC in which your instance runs on shared hardware\. For more information on the tenancy attribute for VPCs, see [Dedicated Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/dedicated-instance.html)\.

1. Set up one or more security groups with inbound and outbound rules that together allow the following traffic:
   + [NFS traffic over TCP on port 2049](https://docs.aws.amazon.com/efs/latest/ug/network-access.html) between the domain and the Amazon EFS volume\.
   + [TCP traffic within the security group](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/security-group-rules-reference.html#sg-rules-other-instances)\. This is required for connectivity between the JupyterServer app and the KernelGateway apps\. You must allow access to at least ports in the range `8192-65535`\.

1. If you want to allow internet access, you must use a [NAT gateway](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html#nat-gateway-working-with) with access to the internet, for example through an [internet gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html)\.

1. If you don't want to allow internet access, [create interface VPC endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/vpce-interface.html) \(AWS PrivateLink\) to allow Studio to access the following services with the corresponding service names\. You must also associate the security groups for your VPC with these endpoints\.
   + SageMaker API : `com.amazonaws.us-east-1.sagemaker.api` 
   + SageMaker runtime: `com.amazonaws.us-east-1.sagemaker.runtime`\. This is required to run Studio notebooks and to train and host models\. 
   + Amazon S3: `com.amazonaws.us-east-1.s3`\.
   + To use SageMaker Projects: `com.amazonaws.us-east-1.servicecatalog`\.
   + Any other AWS services you require\.

**Note**  
For a customer working within VPC mode, company firewalls can cause connection issues with SageMaker Studio or between JupyterServer and the KernelGateway\. Make the following checks if you encounter one of these issues when using SageMaker Studio from behind a firewall\.  
Check that the Studio URL is in your networks allowlist\.
Check that the websocket connections are not blocked\. Jupyter uses websocket under the hood\. If the KernelGateway application is InService, JupyterServer may not be able to connect to the KernelGateway\. You should see this problem when opening System Terminal as well\.

**For more information**
+ [Securing Amazon SageMaker Studio connectivity using a private VPC](http://aws.amazon.com/blogs/machine-learning/securing-amazon-sagemaker-studio-connectivity-using-a-private-vpc)\.
+ [Security groups for your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html)
+ [Connect to SageMaker Through a VPC Interface Endpoint](interface-vpc-endpoint.md)
+ [VPC with public and private subnets \(NAT\)](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Scenario2.html)