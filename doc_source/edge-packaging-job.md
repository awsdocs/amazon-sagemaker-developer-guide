# Package Model<a name="edge-packaging-job"></a>

SageMaker Edge Manager packaging jobs take Amazon SageMaker Neo–compiled models and make any changes necessary to deploy the model with the inference engine, Edge Manager agent\.

**Topics**
+ [Prerequisites](#edge-packaging-job-prerequisites)
+ [Package a Model \(Amazon SageMaker Console\)](edge-packaging-job-console.md)
+ [Package a Model \(Boto3\)](edge-packaging-job-boto3.md)

## Prerequisites<a name="edge-packaging-job-prerequisites"></a>

To package a model, you must do the following:

1. **Compile your machine learning model with SageMaker Neo\.**

   If you have not already done so, compile your model with SageMaker Neo\. For more information on how to compile your model, see [Compile and Deploy Models with Neo](https://docs.aws.amazon.com/sagemaker/latest/dg/neo.html)\. If you are first\-time user of SageMaker Neo, go through [Getting Started with Neo Edge Devices](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-getting-started-edge.html)\.

1. **Get the name of your compilation job\.**

   Provide the name of the compilation job name you used when you compiled your model with SageMaker Neo\. Open the [Amazon SageMaker console](https://console.aws.amazon.com/sagemaker/) and choose **Compilation jobs** to find a list of compilations that have been submitted to your AWS account\. The names of submitted compilation jobs are in the **Name** column\.

1. **Get your IAM ARN\.**

   You need an Amazon Resource Name \(ARN\) of an IAM role that you can use to download and upload the model and contact SageMaker Neo\.

   Use one of the following methods to get your IAM ARN:
   + **Programmatically with the SageMaker Python SDK**

     ```
     import sagemaker
     
     # Initialize SageMaker Session object so you can interact with AWS resources
     sess = sagemaker.Session()
     
     # Get the role ARN 
     role = sagemaker.get_execution_role()
     
     print(role)
     >> arn:aws:iam::<your-aws-account-id>:role/<your-role-name>
     ```

     For more information about using the SageMaker Python SDK, see the [SageMaker Python SDK API](https://sagemaker.readthedocs.io/en/stable/index.html)\.
   + **Using the AWS Identity and Access Management \(IAM\) console**

     Sign in to the [AWS Management Console](https://console.aws.amazon.com/sagemaker/) and open the [IAM console](https://console.aws.amazon.com/iam/)\. In the IAM **Resources** section, choose **Roles** to view a list of roles in your AWS account\. Select or create a role that has `AmazonSageMakerFullAccess`, `AWSIoTFullAccess`, and `AmazonS3FullAccess`\.

     For more information on IAM, see [What is IAM?](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)

1. **Have an S3 bucket URI\.**

   You need to have at least one Amazon Simple Storage Service \(Amazon S3\) bucket URI to store your Neo\-compiled model, the output of the Edge Manager packaging job, and sample data from your device fleet\.

   Use one of the following methods to create an Amazon S3 bucket:
   + **Programmatically with the SageMaker Python SDK**

     You can use the default Amazon S3 bucket during a session\. A default bucket is created based on the following format: `sagemaker-{region}-{aws-account-id}`\. To create a default bucket with the SageMaker Python SDK, use the following:

     ```
     import sagemaker
     
     session=sagemaker.create_session()
     
     bucket=session.default_bucket()
     ```
   + **Using the Amazon S3 console**

     Open the [Amazon S3 console](https://console.aws.amazon.com/s3/) and see [How do I create an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-bucket.html) for step\-by\-step instructions\.