# Cloud Instances<a name="neo-cloud-instances"></a>

Amazon SageMaker Neo provides compilation support for popular machine learning frameworks such as TensorFlow, PyTorch, MXNet, and more\. You can deploy your compiled model to cloud instances and AWS Inferentia instances\. For a full list of supported frameworks and instances types, see [Supported Instances Types and Frameworks](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-supported-cloud.html)\. 

You can compile your model in one of three ways: through the AWS CLI, the SageMaker Console, or the SageMaker SDK for Python\. See, [Use Neo to Compile a Model](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-job-compilation.html) for more information\. Once compiled, your model artifacts are stored in the Amazon S3 bucket URI you specified during the compilation job\. You can deploy your compiled model to cloud instances and AWS Inferentia instances using the SageMaker SDK for Python, AWS SDK for Python \(Boto3\), AWS CLI, or the AWS console\. 

If you deploy your model using AWS CLI, the console, or Boto3, you must select a Docker image Amazon ECR URI for your primary container\. See [Neo Inference Container Images](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-deployment-hosting-services-container-images.html) for a list of Amazon ECR URIs\.

**Topics**
+ [Supported Instance Types and Frameworks](neo-supported-cloud.md)
+ [Deploy a Model](neo-deployment-hosting-services.md)
+ [Request Inferences from a Deployed Service](neo-requests.md)
+ [Inference Container Images](neo-deployment-hosting-services-container-images.md)