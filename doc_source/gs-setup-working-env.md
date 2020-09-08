# Step 2: Create an Amazon SageMaker Notebook Instance<a name="gs-setup-working-env"></a>

An Amazon SageMaker notebook instance is a fully managed machine learning \(ML\) Amazon Elastic Compute Cloud \(Amazon EC2\) compute instance that runs the Jupyter Notebook App\. You use the notebook instance to create and manage Jupyter notebooks that you can use to prepare and process data and to train and deploy machine learning models\. For more information, see [Explore, Analyze, and Process Data](how-it-works-notebooks-instances.md)\. 

**Note**  
If necessary, you can change the notebook instance settings, including the ML compute instance type, later\.

**To create a SageMaker notebook instance**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Notebook instances**, then choose **Create notebook instance**\.

1. On the **Create notebook instance** page, provide the following information \(if a field is not mentioned, leave the default values\):

   1. For **Notebook instance name**, type a name for your notebook instance\.

   1. For **Instance type**, choose `ml.t2.medium`\. This is the least expensive instance type that notebook instances support, and it suffices for this exercise\.

   1. For **IAM role**, choose **Create a new role**, then choose **Create role**\.

   1. Choose **Create notebook instance**\. 

      In a few minutes, Amazon SageMaker launches an ML compute instance—in this case, a notebook instance—and attaches an ML storage volume to it\. The notebook instance has a preconfigured Jupyter notebook server and a set of Anaconda libraries\.

## Next Step<a name="gs-setup-working-env-nextstep"></a>

 [Step 3: Create a Jupyter Notebook](ex1-prepare.md)\.