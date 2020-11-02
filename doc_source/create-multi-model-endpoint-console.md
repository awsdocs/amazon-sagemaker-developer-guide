# Create a Multi\-Model Endpoint \(Console\)<a name="create-multi-model-endpoint-console"></a>

**To create a multi\-model endpoint \(console\)**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Model**, and then from the **Inference** group, choose **Create model**\. 

1. For **Model name**, enter a name\.

1. For **IAM role**\. choose or create an IAM role that has the AmazonSageMakerFullAccess IAM policy attached\. 

1.  In the **Container definition** section, for **Provide model artifacts and inference image options**choose **Use multiple models**\.  
![\[The section of the Create model page where you can choose Use multiple models to host multiple models on a single endpoint.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/mme-create-model-ux-2.PNG)

1. Choose **Create model**\.

1. Deploy your multi\-model endpoint as you would a single model endpoint\. For instructions, see [Step 6\.1: Deploy the Model to SageMaker Hosting Services](ex1-deploy-model.md)\.