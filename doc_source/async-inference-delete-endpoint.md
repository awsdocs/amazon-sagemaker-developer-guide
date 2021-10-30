# Delete an Asynchronous Endpoint<a name="async-inference-delete-endpoint"></a>

Delete an asynchronous endpoint in a similar manner to how you would delete a SageMaker hosted endpoint with the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DeleteEndpoint.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DeleteEndpoint.html) API\. Specify the name of the asynchronous endpoint you want to delete\. When you delete an endpoint, SageMaker frees up all of the resources that were deployed when the endpoint was created\. Deleting a model does not delete model artifacts, inference code, or the IAM role that you specified when creating the model\.

Delete your SageMaker model with the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DeleteModel.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DeleteModel.html) API or with the SageMaker console\.

------
#### [ Boto3 ]

```
import boto3 

# Create a low-level SageMaker service client.
sagemaker_client = boto3.client('sagemaker', region_name=<aws_region>)
sagemaker_client.delete_endpoint(EndpointName='<endpoint-name>')
```

------
#### [ SageMaker console ]

1. Navigate to the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. Expand the **Inference** dropdown list\.

1. Select **Endpoints**\.

1. Search for endpoint in the **Search endpoints** search bar\.

1. Select your endpoint\.

1. Choose **Delete**\.

------

In addition to deleting the asynchronous endpoint, you might want to clear up other resources that were used to create the endpoint, such as the Amazon ECR repository \(if you created a custom inference image\), the SageMaker model, and the asynchronous endpoint configuration itself\. 