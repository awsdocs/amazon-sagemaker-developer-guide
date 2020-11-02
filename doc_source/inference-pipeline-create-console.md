# Create a Pipeline Model<a name="inference-pipeline-create-console"></a>

To create a pipeline model that can be deployed to an endpoint or used for a batch transform job, use the Amazon SageMaker console or the `CreateModel` operation\. 

**To create an inference pipeline \(console\)**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Models**, and then choose **Create models** from the **Inference** group\. 

1. On the **Create model** page, provide a model name, choose an IAM role, and, if you want to use a private VPC, specify VPC values\.   
![\[The page for creating a model for an Inference Pipeline.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/create-pipeline-model.png)

1. To add information about the containers in the inference pipeline, choose **Add container**, then choose **Next**\.

1. Complete the fields for each container in the order that you want to execute them, up to the maximum of five\. Complete the **Container input options**, , **Location of inference code image**, and, optionally, **Location of model artifacts**, **Container host name**, and **Environmental variables** fields\. \.  
![\[Creating a pipeline model with containers.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/create-pipeline-model-containers.png)

   The **MyInferencePipelineModel** page summarizes the settings for the containers that provide input for the model\. If you provided the environment variables in a corresponding container definition, SageMaker shows them in the **Environment variables** field\.  
![\[The summary of container settings for the pipeline model.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pipeline-MyInferencePipelinesModel-recap.png)