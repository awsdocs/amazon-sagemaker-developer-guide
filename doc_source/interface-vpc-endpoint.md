# Connect to Amazon SageMaker Runtime Through a VPC Interface Endpoint<a name="interface-vpc-endpoint"></a>

You can connect directly to Amazon SageMaker Runtime through an [interface endpoint](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpce-interface.html) in your Virtual Private Cloud \(VPC\) instead of connecting over the internet\. When you use a VPC interface endpoint, communication between your VPC and Amazon SageMaker Runtime is conducted entirely and securely within the AWS network\.

Amazon SageMaker Runtime supports [Amazon Virtual Private Cloud](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Introduction.html) \(Amazon VPC\) interface endpoints that are powered by [AWS PrivateLink](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Introduction.html#what-is-privatelink)\. Each VPC endpoint is represented by one or more [Elastic Network Interfaces](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html) \(ENIs\) with private IP addresses in your VPC subnets\.

The VPC interface endpoint connects your VPC directly to Amazon SageMaker Runtime without an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection\. The instances in your VPC don't need public IP addresses to communicate with Amazon SageMaker Runtime\.

You can create an interface endpoint to connect to Amazon SageMaker Runtime with either the AWS console or AWS Command Line Interface \(AWS CLI\) commands\. For instructions, see [Creating an Interface Endpoint](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpce-interface.html#create-interface-endpoint)\.

*After you have created a VPC endpoint*, you can use the following example CLI commands that use the `endpoint-url` parameter to specify interface endpoints to Amazon SageMaker Runtime:

```
aws sagemaker-runtime invoke-endpoint –-endpoint-url VPC_Endpoint_ID.runtime.sagemaker.Region.vpce.amazonaws.com  \
    --endpoint-name Endpoint_Name \
    --body "Endpoint_Body" \
    --content-type "Content_Type" \
            Output_File
```

If you enable private DNS hostnames for your VPC endpoint, you don't need to specify the endpoint URL\. The Amazon SageMaker Runtime DNS hostname that the CLI and Amazon SageMaker Runtime SDKs use by default \(https://runtime\.sagemaker\.*Region*\.amazonaws\.com\) resolves to your VPC endpoint\.

Amazon SageMaker Runtime supports VPC endpoints in all AWS Regions where both [Amazon VPC](http://docs.aws.amazon.com/general/latest/gr/rande.html#vpc_region) and [Amazon SageMaker](http://docs.aws.amazon.com/general/latest/gr/rande.html#sagemaker_region) are available\.

To learn more about AWS PrivateLink, see the [AWS PrivateLink documentation](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Introduction.html#what-is-privatelink) and visit the AWS Blog\. Refer to [VPC Pricing](https://aws.amazon.com/vpc/pricing/) for the price of VPC Endpoints\. To learn more about VPC and Endpoints, see [Amazon VPC](https://aws.amazon.com/vpc/)\.