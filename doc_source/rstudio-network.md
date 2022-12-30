# Network and Storage<a name="rstudio-network"></a>

The following topic describes network access and data storage considerations for your RStudio instance\. For general information about network access and data storage when using Amazon SageMaker, see [Data Protection in Amazon SageMaker](data-protection.md)\.

 **Encryption**

 RStudio on Amazon SageMaker supports encryption at rest\.

 **Use RStudio in VPC\-only mode**

RStudio in Amazon SageMaker supports [AWS PrivateLink](https://docs.aws.amazon.com/https://docs.aws.amazon.com/vpc/latest/userguide/endpoint-services-overview.html) integration\. With this integration, you can use RStudio on SageMaker in VPC\-only mode without direct access to the internet\. When you use RStudio in VPC\-only mode, your security groups are automatically managed by the service\. This includes connectivity between your RServer and your RSessions\.

The following are required to use RStudio in VPC\-only mode\. For more information on selecting a VPC, see [Choose a VPC](onboard-vpc.md)\.
+ A private subnet with either access the internet to make a call to Amazon SageMaker & License Manager, or Amazon Virtual Private Cloud \(Amazon VPC\) endpoints for both Amazon SageMaker & License Manager\.
+ The Domain cannot have any more than two associated Security Groups\.
+ A Security Group ID for use with the Domain in Domain Settings\. This must allow all outbound access\.
+ A Security Group ID for use with the Amazon VPC endpoint\. This security group must allow inbound traffic from the Domain Security Group ID\.
+ Amazon VPC Endpoint for `sagemaker.api` and AWS License Manager\. This must be in the same Amazon VPC as the private subnet\. 