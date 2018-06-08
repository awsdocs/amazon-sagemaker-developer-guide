# Step 3\.1: Create a Jupyter Notebook and Initialize Variables<a name="ex1-prepare"></a>

In this section, you create a Jupyter notebook in your Amazon SageMaker notebook instance and initialize variables\.

**To create a Jupyter notebook**

1. Create the notebook\.

   1. Sign in to the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\. 

   1. Open the notebook instance, by choosing **Open** next to its name\. The Jupyter notebook server page appears:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/ironman-jupyter-home-page-ex.png)

   1. To create a notebook, in the **Files** tab, choose **New**, and **conda\_python3**\. This pre\-installed environment includes the default Anaconda installation and Python 3\.

   1. Name the notebook\.

1. Copy the following Python code and paste it into your notebook\. Add the name of the S3 bucket that you created in [Step 1: Setting Up](gs-set-up.md), and run the code\. The `get_execution_role` function retrieves the IAM role you created at the time of creating your notebook instance\.

   ```
   from sagemaker import get_execution_role
   
   role = get_execution_role()
   bucket='bucket-name'
   ```

## <a name="ex1-prepare-2"></a>

**Next Step**  
[Step 3\.2: Download, Explore, and Transform the Training Data](ex1-preprocess-data.md)