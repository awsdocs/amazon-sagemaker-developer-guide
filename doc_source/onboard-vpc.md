# Choose a VPC<a name="onboard-vpc"></a>

This topic provides detailed information about choosing an Amazon Virtual Private Cloud \(VPC\) when you onboard to Amazon SageMaker Domain\. For more information about onboarding to SageMaker Domain, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.

By default, SageMaker Domain uses two VPCs\. One VPC is managed by Amazon SageMaker and provides direct internet access\. You specify the other VPC, which provides encrypted traffic between the Domain and your Amazon Elastic File System \(EFS\) volume\.

You can change this behavior so that SageMaker sends all traffic over your specified VPC\. When you choose this option, you must provide the subnets, security groups, and interface endpoints that are necessary to communicate with the SageMaker API and SageMaker runtime, and various AWS services, such as Amazon Simple Storage Service \(Amazon S3\) and Amazon CloudWatch, that are used by Amazon SageMaker Studio and your Studio notebooks\.

When you onboard to SageMaker Domain, you tell SageMaker to send all traffic over your VPC by setting the network access type to **VPC only**\.

**To specify the VPC information**

When you specify the VPC entities \(that is, the VPC, subnet, or security group\) in the following procedure, one of three options is presented based on the number of entities you have in the current AWS Region\. The behavior is as follows:
+ One entity – SageMaker uses that entity\. This can't be changed\.
+ Multiple entities – You must choose the entities from the dropdown list\.
+ No entities – You must create one or more entities in order to use Domain\. Choose **Create <entity>** to open the VPC console in a new browser tab\. After you create the entities, return to the Domain **Get started** page to continue the onboarding process\.

This procedure is part of the Amazon SageMaker Domain onboarding process when you choose **Standard setup**\. Your VPC information is specified under the **Network** section\.

1. Choose the VPC\.

1. Choose one or more subnets\. If you don't choose any subnets, SageMaker uses all the subnets in the VPC\.

1. Select the network access type\.
   + **Public internet only** – Non\-EFS traffic goes through a SageMaker managed VPC, which allows internet access\. Traffic between the Domain and your Amazon EFS volume is through the specified VPC\.
   + **VPC only** – All SageMaker traffic is through the specified VPC and subnets\. Internet access is disabled by default\.

1. Choose the security groups\. If you chose **Public internet only**, this step is optional\. If you chose **VPC only**, this step is required\.
**Note**  
For the maximum number of allowed security groups, see [UserSettings](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UserSettings.html)\.

For VPC requirements in **VPC only** mode, see [Connect SageMaker Studio Notebooks in a VPC to External Resources](studio-notebooks-and-internet-access.md)\.