# Create a Pipeline Model<a name="inference-pipeline-create-console"></a>

You can create a pipeline model that can be deployed to an endpoint or for a transform job with the Amazon SageMaker console\. 

Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)

Choose **Models**, and then choose **Create models** from the **Inference** group\. On the **Create model** page, complete the **Model name**, **IAM role**, and, if needed, **VPC** fields\. 

![\[An image of the Create model UI for an Inference Pipeline.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/create-pipeline-model.png)

To add information about the remaining containers in the inference pipeline, choose **Add container**, then choose **Next**\. Specify each of the containers to be added to the Amazon SageMaker inference pipeline\. We support up to five containers in a pipeline\. They must be listed in the order of execution\. Complete the **Container input options**, , **Location of inference code image**, and, optionally, **Location of model artifacts**, **Container host name**, and **Environmental variables** fields\. Confirm that the information for the containers is accurate, and then choose **Create model**\.

![\[An image of the Create model UI for Containers of an Inference Pipeline.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/create-pipeline-model-containers.png)

The **MyInferencePipelineModel** page summarizes the model input\. If you provide the environment variables in a corresponding container definition, Amazon SageMaker shows them in **Environment variables**\.

![\[An image of the pipeline model inputs summary UI.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/pipeline-MyInferencePipelinesModel-recap.png)