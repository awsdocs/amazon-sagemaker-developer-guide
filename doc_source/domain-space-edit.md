# Edit a shared space<a name="domain-space-edit"></a>

 You can only edit the details for a shared space using the AWS CLI\. This is not currently supported from the Amazon SageMaker console\. You can only update workspace attributes when there are no running applications in the shared space\. 

To edit the details of a shared space from the AWS CLI, run the following command from the terminal of your local machine\. shared spaces only support the use of JupyterLab 3 image ARNs\. For more information, see [JupyterLab Versioning](studio-jl.md)\.

```
aws --region region \
sagemaker update-space \
--domain-id domain-id \
--space-name space-name \
--query SpaceArn --output text \
--space-settings '{
  "JupyterServerAppSettings": {
    "DefaultResourceSpec": {
      "SageMakerImageArn": "sagemaker-image-arn",
      "InstanceType": "system"
    }
  }
}'
```