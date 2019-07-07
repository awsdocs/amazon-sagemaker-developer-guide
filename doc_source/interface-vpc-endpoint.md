# Connect to Amazon SageMaker Through a VPC Interface Endpoint<a name="interface-vpc-endpoint"></a>

You can connect directly to the Amazon SageMaker API or to the Amazon SageMaker Runtime through an [interface endpoint](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpce-interface.html) in your Virtual Private Cloud \(VPC\) instead of connecting over the internet\. When you use a VPC interface endpoint, communication between your VPC and the Amazon SageMaker API or Runtime is conducted entirely and securely within the AWS network\. 

**Note**  
PrivateLink for Amazon SageMaker is not supported in the `us-gov-west-1` region\.

The Amazon SageMaker API and Runtime support [Amazon Virtual Private Cloud](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Introduction.html) \(Amazon VPC\) interface endpoints that are powered by [AWS PrivateLink](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Introduction.html#what-is-privatelink)\. Each VPC endpoint is represented by one or more [Elastic Network Interfaces](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html) \(ENIs\) with private IP addresses in your VPC subnets\.

The VPC interface endpoint connects your VPC directly to the Amazon SageMaker API or Runtime without an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection\. The instances in your VPC don't need public IP addresses to communicate with the Amazon SageMaker API or Runtime\.

You can create an interface endpoint to connect to Amazon SageMaker or to Amazon SageMaker Runtime with either the AWS console or AWS Command Line Interface \(AWS CLI\) commands\. For instructions, see [Creating an Interface Endpoint](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpce-interface.html#create-interface-endpoint)\.

*After you have created a VPC endpoint*, you can use the following example CLI commands that use the `endpoint-url` parameter to specify interface endpoints to the Amazon SageMaker API or Runtime:

```
aws sagemaker list-notebook-instances --endpoint-url VPC_Endpoint_ID.api.sagemaker.Region.vpce.amazonaws.com

aws sagemaker list-training-jobs --endpoint-url VPC_Endpoint_ID.api.sagemaker.Region.vpce.amazonaws.com

aws sagemaker-runtime invoke-endpoint --endpoint-url VPC_Endpoint_ID.runtime.sagemaker.Region.vpce.amazonaws.com  \
    --endpoint-name Endpoint_Name \
    --body "Endpoint_Body" \
    --content-type "Content_Type" \
            Output_File
```

If you enable private DNS hostnames for your VPC endpoint, you don't need to specify the endpoint URL\. The Amazon SageMaker API DNS hostname that the CLI and Amazon SageMaker SDK use by default \(https://api\.sagemaker\.*Region*\.amazonaws\.com\) resolves to your VPC endpoint\. Similarly, the Amazon SageMaker Runtime DNS hostname that the CLI and Amazon SageMaker Runtime SDK use by default \(https://runtime\.sagemaker\.*Region*\.amazonaws\.com\) resolves to your VPC endpoint\.

The Amazon SageMaker API and Runtime support VPC endpoints in all AWS Regions where both [Amazon VPC](https://docs.aws.amazon.com/general/latest/gr/rande.html#vpc_region) and [Amazon SageMaker](https://docs.aws.amazon.com/general/latest/gr/rande.html#sagemaker_region) are available\. Amazon SageMaker supports making calls to all of its [Actions](API_Operations.md) inside your VPC\. The result `AuthorizedUrl` from the [CreatePresignedNotebookInstanceUrl](API_CreatePresignedNotebookInstanceUrl.md) is not supported by Private Link\. For information about how to enable PrivateLink for the authorized URL that users use to connect to a notebook instance, see [Connect to a Notebook Instance Through a VPC Interface Endpoint](notebook-interface-endpoint.md)\.

To learn more about AWS PrivateLink, see the [AWS PrivateLink documentation](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Introduction.html#what-is-privatelink) \. Refer to [VPC Pricing](https://aws.amazon.com/vpc/pricing/) for the price of VPC Endpoints\. To learn more about VPC and Endpoints, see [Amazon VPC](https://aws.amazon.com/vpc/)\. For information about how to use identity\-based AWS Identity and Access Management policies to restrict access to the Amazon SageMaker API and runtime, see [Control Access to the Amazon SageMaker API by Using Identity\-based Policies](using-identity-based-policies.md#api-access-policy)\.

## Create a VPC Endpoint Policy for Amazon SageMaker<a name="api-private-link-policy"></a>

You can create a policy for Amazon VPC endpoints for Amazon SageMaker to specify the following:
+ The principal that can perform actions\.
+ The actions that can be performed\.
+ The resources on which actions can be performed\.

For more information, see [Controlling Access to Services with VPC Endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-access.html) in the *Amazon VPC User Guide*\.

**Note**  
VPC endpoint policies aren't supported for Federal Information Processing Standard \(FIPS\) Amazon SageMaker runtime endpoints for [InvokeEndpoint](API_runtime_InvokeEndpoint.md)\.

The following example VPC endpoint policy specifies that all users who have access to the VPC interface endpoint are allowed to invoke the Amazon SageMaker hosted endpoint named `myEndpoint`\.

```
{
  "Statement": [
      {
          "Action": "sagemaker:InvokeEndpoint",
          "Effect": "Allow",
          "Resource": "arn:aws:sagemaker:us-west-2:123456789012:endpoint/myEndpoint",
          "Principal": "*"
      }
  ]
}
```

In this example, the following are denied:
+ Other Amazon SageMaker API actions, such as `sagemaker:CreateEndpoint` and `sagemaker:CreateTrainingJob`\.
+ Invoking Amazon SageMaker hosted endpoints other than `myEndpoint`\.

**Note**  
In this example, users can still take other Amazon SageMaker API actions from outside the VPC\. For information about how to restrict API calls to those from within the VPC, see [Control Access to the Amazon SageMaker API by Using Identity\-based Policies](using-identity-based-policies.md#api-access-policy)\.

## Connect Your Private Network to Your VPC<a name="notebook-private-link-vpn"></a>

To call the Amazon SageMaker API and runtime through your VPC, you have to connect from an instance that is inside the VPC or connect your private network to your VPC by using an Amazon Virtual Private Network \(VPN\) or AWS Direct Connect\. For information about Amazon VPN, see [VPN Connections](https://docs.aws.amazon.com/vpc/latest/userguide/vpn-connections.html) in the *Amazon Virtual Private Cloud User Guide*\. For information about AWS Direct Connect, see [Creating a Connection](https://docs.aws.amazon.com/directconnect/latest/UserGuide/create-connection.html) in the *AWS Direct Connect User Guide*\.