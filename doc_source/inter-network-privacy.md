# Internetwork Traffic Privacy<a name="inter-network-privacy"></a>

This topic describes how Amazon SageMaker secures connections from the service to other locations\.

Internetwork communications support TLS 1\.2 encryption between all components and clients\.

Instances can be connected to Customer VPC, providing access to S3 VPC endpoints or customer repositories\. Internet egress can be managed through this interface by the customer if service platform internet egress is disabled for notebooks\. For training and hosting, egress through the service platform is not available when connected to the customer's VPC\.

By default, API calls made to published endpoints traverse the public network to the request router\. SageMaker supports Amazon Virtual Private Cloud interface endpoints powered by AWS PrivateLink for private connectivity between the customer's VPC and the request router to access hosted model endpoints\. For information about Amazon VPC, see [Connect to SageMaker Through a VPC Interface Endpoint](interface-vpc-endpoint.md)