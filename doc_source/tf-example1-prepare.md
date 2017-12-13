# Step 1: Create a Notebook and Initialize Variables<a name="tf-example1-prepare"></a>

In this section, you create a Jupyter notebook in your Amazon SageMaker notebook instance and initialize variables\.

1. Follow the instructions in [Step 1: Setting Up](gs-set-up.md) to create the S3 bucket and IAM role\. 

   For simplicity, we suggest that you use one S3 bucket to store both the training code and the results of model training\. 

1. Create the notebook\.

   1. If you haven't already done so, sign in to the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

   1. Open the notebook instance by choosing `Open` next to its name\. The Jupyter notebook server page appears\.

   1. To create a notebook, choose **Files**, **New**, and **conda\_tensorflow\_p27**\. This pre\-installed environment includes the default Anaconda installation, Python 3, and TensorFlow\.

   1. Name the notebook\.

1. To initialize variables, copy, paste, and run the following code in your notebook\. Provide the name of the S3 bucket that contains your custom code\. The `get_execution_role function` retrieves the IAM role you created at the time of creating your notebook instance\. You can use the bucket that you created in [Step 1: Setting Up](gs-set-up.md), or create a new bucket\.

   ```
   from sagemaker import get_execution_role
   
   #Bucket location to save your custom code in tar.gz format.
   custom_code_upload_location = 's3://yourbucket/customcode/tensorflow_iris'
   
   #Bucket location where results of model training are saved.
   model_artifacts_location = 's3://yourbucket/artifacts'
   
   #IAM execution role that gives SageMaker access to resources in your AWS account.
   role = get_execution_role()
   ```

## Next Step<a name="tf-example1-prepare-nexttopic"></a>

 [Step 2: Train a Model on Amazon SageMaker Using TensorFlow Custom Code](tf-example1-train.md) 