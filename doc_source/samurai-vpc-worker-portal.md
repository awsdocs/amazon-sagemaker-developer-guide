# Use Amazon VPC mode from a Private Worker Portal<a name="samurai-vpc-worker-portal"></a>

To restrict worker portal access to labelers working inside of your Amazon VPC, you can add a VPC configuration when you create a Ground Truth private workforce\. You can also add a VPC configuration to an existing private workforce\. Ground Truth automatically creates VPC interface endpoints in your VPC and sets up VPC PrivateLinks between your VPC endpoint and the Ground Truth services\. The worker portal URL associated with the workforce can be accessed from your VPC\. The worker portal URL can also be accessed from public internet until you set the restriction on the public internet\. When you delete the workforce or remove the VPC configuration from your workforce, Ground Truth automatically deletes the VPC endpoints associated with the workforce\.

**Note**  
There can be only one VPC supported for a workforce\.

[Point Cloud](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-point-cloud.html) and [Video](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-video.html) tasks do not support loading through a VPC\.

The guide demonstrates how to complete the necessary steps to add and delete an Amazon VPC configuration to your workforce, and satisfy the prerequisites\.

## Prerequisites<a name="samurai-vpc-getting-started-prerequisites"></a>
+ You have an Amazon VPC configured that you can use\. If you have not configured a VPC, follow these instructions for [creating a VPC](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)\.
+ Depending on how a [Worker Task Template](https://docs.aws.amazon.com/sagemaker/latest/dg/a2i-instructions-overview.html) is written, labeling data stored in an Amazon S3 bucket may be accessed directly from S3 during labeling tasks\. In these cases, the VPC network must be configured to allow traffic from the device used by the human labeler to the S3 bucket containing labeling data\.
+ Follow [View and update DNS attributes for your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-dns.html#vpc-dns-updating) to enable DNS hostnames and DNS resolution for your VPC\.

**Note**  
There are two ways for configuring your VPC for your workforce\. You can do this through the [Console](https://console.aws.amazon.com/sagemaker) or the AWS SageMaker [CLI](http://aws.amazon.com/cli/)\.