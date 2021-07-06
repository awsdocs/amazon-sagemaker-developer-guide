# Connect SageMaker Studio Notebooks to Resources in a VPC<a name="studio-notebooks-and-internet-access"></a>

Amazon SageMaker Studio allows direct internet access by default\. This could provide an avenue for unauthorized access to your data\. For example, if you install malicious code on your computer \(in the form of a publicly available notebook or a publicly available source code library\), it could access your data\. You can choose to restrict which traffic can access the internet by launching Studio in a Virtual Private Cloud \(VPC\) of your choosing\. This allows you fine\-grained control of the network access and internet connectivity of your SageMaker Studio notebooks\. You can disable direct internet access to add an additional layer of security\.

By default, SageMaker Studio provides a network interface that allows communication with the internet through a VPC managed by SageMaker\. Traffic to AWS services like Amazon S3 and CloudWatch goes through an internet gateway as does traffic that accesses the SageMaker API and SageMaker runtime\. Traffic between the domain and your Amazon EFS volume goes through the VPC that you specified when you onboarded to Studio or called the [CreateDomain](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateDomain.html) API\. The following diagram shows the default configuration\.

![\[Diagram of SageMaker Studio VPC using direct internet access\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-vpc-internet.png)

To disable direct internet access, you can specify the `VPC only` network access type when you onboard to Studio or call the [CreateDomain](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateDomain.html) API\. Doing so prevents SageMaker from providing internet access to your Studio notebooks\. As a result, you won't be able to run a Studio notebook unless your VPC has an interface endpoint to the SageMaker API and runtime, or a NAT gateway with internet access, and your security groups allow outbound connections\. The following diagram shows a configuration for using VPC\-only mode\.

![\[Diagram of SageMaker Studio VPC using VPC-only mode\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-vpc-private.png)

**VPC requirements** in `VpcOnly`

When you choose `VpcOnly`, your VPC must contain the following:
+ Subnets with one IP address for each instance\. For more information, see [VPC and subnet sizing for IPv4](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html#vpc-sizing-ipv4)\.
**Note**  
You can configure only subnets with a default tenancy VPC in which your instance runs on shared hardware\. For more information on the tenancy attribute for VPCs, see [Dedicated Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/dedicated-instance.html)\.
+ One or more security groups with inbound and outbound rules that together allow the following traffic:
  + NFS traffic over TCP on port 2049 between the domain and the Amazon EFS volume\.
  + TCP traffic within the security group\. This is required for connectivity between the JupyterServer app and the KernelGateway apps\.
+ If you want to allow internet access, you must use NAT gateway with access to internet \(for example, via an internet gateway\)\.
+ If you don't want to allow internet access, you must create interface VPC endpoints \(AWS PrivateLink\) to access the following:
  + The SageMaker API and SageMaker runtime\. This is required to run Studio notebooks and to train and host models\.
  + Amazon S3 and other AWS services you require\.
  + If you're using SageMaker Projects in SageMaker Studio without internet access, you need a VPC endpoint for Service Catalog\.

  You must associate the security groups for your VPC with these endpoints\.

**Note**  
For a customer working within VPC mode, company firewalls can cause connection issues with SageMaker Studio or between JupyterServer and the KernelGateway\. Make the following checks if you encounter one of these issues when using SageMaker Studio from behind a firewall\.  
Check that the Studio URL is in your networks allowlist\.
Check that the websocket connections are not blocked\. Jupyter uses websocket under the hood\. If the KernelGateway application is InService, JupyterServer may not be able to connect to the KernelGateway\. You should see this problem when opening System Terminal as well\.

**For more information**
+ [Securing Amazon SageMaker Studio connectivity using a private VPC](http://aws.amazon.com/blogs/machine-learning/securing-amazon-sagemaker-studio-connectivity-using-a-private-vpc)\.
+ [Security groups for your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html)
+ [Connect to SageMaker Through a VPC Interface Endpoint](interface-vpc-endpoint.md)
+ [VPC with public and private subnets \(NAT\)](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Scenario2.html)