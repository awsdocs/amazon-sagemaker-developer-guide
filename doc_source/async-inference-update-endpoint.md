# Update an Asynchronous Endpoint<a name="async-inference-update-endpoint"></a>

Update an asynchronous endpoint with the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateEndpoint.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateEndpoint.html) API\. When you update an endpoint, SageMaker first switches to the new endpoint you specify before it deletes the resources that were provisioned in the previous endpoint configuration\. Do not delete an `EndpointConfig` with an endpoint that is live or while the `UpdateEndpoint` or `CreateEndpoint` operations are being performed on the endpoint\. 

```
# The name of the endpoint. The name must be unique within an AWS Region in your AWS account.
endpoint_name='<endpoint-name>'

# The name of the endpoint configuration associated with this endpoint.
endpoint_config_name='<endpoint-config-name>'

sagemaker_client.update_endpoint(
                                EndpointConfigName=endpoint_config_name,
                                EndpointName=endpoint_name
                                )
```

When Amazon SageMaker receives the request, it sets the endpoint status to **Updating**\. After updating the asynchronous endpoint, it sets the status to **InService**\. To check the status of an endpoint, use the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeEndpoint.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeEndpoint.html) API\. For a full list of parameters you can specify when updating an endpoint, see the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateEndpoint.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateEndpoint.html) API\.