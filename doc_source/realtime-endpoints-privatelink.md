# Low latency real\-time inference with AWS PrivateLink<a name="realtime-endpoints-privatelink"></a>

 Amazon SageMaker provides low latency for real\-time inferences while maintaining high availability and resiliency using multi\-AZ deployment\. The application latency is made up of two primary components: infrastructure or overhead latency and model inference latency\. Reduction of overhead latency opens up new possibilities such as deploying more complex, deep, and accurate models or splitting monolithic applications into scalable and maintainable microservice modules\. You can reduce the latency for real\-time inferences with SageMaker using an AWS PrivateLink deployment\. With AWS PrivateLink, you can privately access all SageMaker API operations from your Virtual Private Cloud \(VPC\) in a scalable manner by using interface VPC endpoints\. An interface VPC endpoint is an elastic network interface in your subnet with private IP addresses that serves as an entry point for all SageMaker API calls\.

By default, a SageMaker endpoint with 2 or more instances is deployed in at least 2 AWS Availability Zones \(AZs\) and instances in any AZ can process invocations\. This results in one or more AZ “hops” that contribute to the overhead latency\. An AWS PrivateLink deployment with the `privateDNSEnabled` option set as `true` alleviates this by achieving two objectives:
+ It keeps all inference traffic within your VPC\.
+ It keeps invocation traffic in the same AZ as the client that originated it when using SageMaker\. This avoids the “hops” between AZs reducing the overhead latency\.

The following sections of this guide demonstrate how you can reduce the latency for real\-time inferences with AWS PrivateLink deployment\.

**Topics**
+ [Deploy AWS PrivateLink](#deploy-privatelink)
+ [Deploy SageMaker endpoint in a VPC](#deploy-sagemaker-inference-endpoint)
+ [Invoke the SageMaker endpoint](#invoke-sagemaker-inference-endpoint)

## Deploy AWS PrivateLink<a name="deploy-privatelink"></a>

To deploy AWS PrivateLink, first create an interface endpoint for the VPC from which you connect to the SageMaker endpoints\. Please follow the steps in [ Access an AWS service using an interface VPC endpoint](https://docs.aws.amazon.com/vpc/latest/privatelink/create-interface-endpoint.html) to create the interface endpoint\. While creating the endpoint, select the following settings in the console interface:
+ Select the **Enable DNS name** checkbox under **Additional Settings**
+ Select the appropriate security groups and the subnets to be used with the SageMaker endpoints\.

Also make sure that the VPC has DNS hostnames turned on\. For more information on how to change DNS attributes for your VPC, see [ View and update DNS attributes for your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-dns.html#vpc-dns-updating)\.

## Deploy SageMaker endpoint in a VPC<a name="deploy-sagemaker-inference-endpoint"></a>

To achieve low overhead latency, create a SageMaker endpoint using the same subnets that you specified when deploying AWS PrivateLink\. These subnets should match the AZs of your client application, as shown in the following code snippet\.

```
model_name = '<the-name-of-your-model>'

vpc = 'vpc-0123456789abcdef0'
subnet_a = 'subnet-0123456789abcdef0'
subnet_b = 'subnet-0123456789abcdef1'
security_group = 'sg-0123456789abcdef0'

create_model_response = sagemaker_client.create_model(
    ModelName = model_name,
    ExecutionRoleArn = sagemaker_role,
    PrimaryContainer = {
        'Image': container,
        'ModelDataUrl': model_url
    },
    VpcConfig = {
        'SecurityGroupIds': [security_group],
        'Subnets': [subnet_a, subnet_b],
    },
)
```

The aforementioned code snippet assumes that you have followed the steps in [Before you begin](realtime-endpoints-deployment.md#realtime-endpoints-deployment-setup)\.

## Invoke the SageMaker endpoint<a name="invoke-sagemaker-inference-endpoint"></a>

Finally, specify the SageMaker client and invoke the SageMaker endpoint as shown in the following code snippet\.

```
endpoint_name = '<endpoint-name>'
  
runtime_client = boto3.client('sagemaker-runtime')
response = runtime_client.invoke_endpoint(EndpointName=endpoint_name, 
                                          ContentType='text/csv', 
                                          Body=payload)
```

For more information on endpoint configuration, see [Create an Endpoint Configuration](realtime-endpoints-deployment.md#realtime-endpoints-deployment-create-endpoint-config)\.