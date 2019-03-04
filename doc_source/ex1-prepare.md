# Step 2\.1: Create a Jupyter Notebook and Initialize Variables<a name="ex1-prepare"></a>

Create a Jupyter notebook in your Amazon SageMaker notebook instance and initialize variables\.

**To create a Jupyter notebook**

1. Create the notebook\.

   1. Sign in to the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\. 

   1. Open the notebook instance, by choosing **Open** next to its name\. The Jupyter notebook server page appears:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/sagemaker-jupyter-home-page-ex.png)

   1. To create a notebook, in the **Files** tab, choose **New**, and **conda\_python3**\. This pre\-installed environment includes the default Anaconda installation and Python 3\.

   1. In the Jupyter notebook, under **File**, choose **Save as**, and name the notebook\.

1. Copy the following Python code and paste it into your notebook\. Add the name of the S3 bucket that you created in [Set Up Amazon SageMaker](gs-set-up.md), and run the code\. The `get_execution_role` function retrieves the IAM role you created when you created your notebook instance\.

   ```
   from sagemaker import get_execution_role
   
   role = get_execution_role()
   bucket = 'bucket-name' # Use the name of your s3 bucket here
   ```

## <a name="ex1-prepare-2"></a>

**Next Step**  
[Step 2\.2: Download, Explore, and Transform the Training Data](ex1-preprocess-data.md)