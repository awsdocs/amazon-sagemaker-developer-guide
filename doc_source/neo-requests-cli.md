# Request Inferences from a Deployed Service \(AWS CLI\)<a name="neo-requests-cli"></a>

 Inference requests can be made with the [ `sagemaker-runtime invoke-endpoint`](https://docs.aws.amazon.com/cli/latest/reference/sagemaker-runtime/invoke-endpoint.html) once you have an Amazon SageMaker endpoint `InService`\. You can make inference requests with the AWS Command Line Interface \(AWS CLI\)\. The following example shows how to send an image for inference: 

```
aws sagemaker-runtime invoke-endpoint --endpoint-name 'insert name of your endpoint here' --body fileb://image.jpg --content-type=application/x-image output_file.txt
```

An `output_file.txt` with information about your inference requests is made if the inference was successful\. 