# Step 3: Create a Jupyter Notebook<a name="ex1-prepare"></a>

Create a Jupyter notebook in the notebook instance you created in [Step 2: Create an Amazon SageMaker Notebook Instance](gs-setup-working-env.md), and create a cell that gets the IAM role that your notebook needs to run Amazon SageMaker APIs and specifies the name of the Amazon S3 bucket that you will use to store the datasets that you use for your training data and the model artifacts that a Amazon SageMaker training job outputs\.

**To create a Jupyter notebook**

1. Open the notebook instance\.

   1. Sign in to the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

   1. Open the notebook instance, by choosing either **Open Jupyter** for classic Juypter view or **Open JupyterLab** for JupyterLab view next to the name of the notebook instance\. The Jupyter notebook server page appears:

1. Create a notebook\. 

   1. If you opened the notebook in Jupyter classic view, on the **Files** tab, choose **New**, and **conda\_python3**\. This preinstalled environment includes the default Anaconda installation and Python 3\.

   1. If you opened the notebook in JupyterLab view, on the **File** menu, choose **New**, and then choose **Notebook\.**\. For **Select Kernel**, choose **conda\_python3**\. This preinstalled environment includes the default Anaconda installation and Python 3\.

1. In the Jupyter notebook, choose **File** and **Save as**, and name the notebook\.

1. Copy the following Python code and paste it into the first cell in your notebook\. Add the name of the S3 bucket that you created in [Set Up Amazon SageMaker](gs-set-up.md), and run the code\. The `get_execution_role` function retrieves the IAM role you created when you created your notebook instance\.

   ```
   import os
   import boto3
   import re
   import copy
   import time
   from time import gmtime, strftime
   from sagemaker import get_execution_role
   
   role = get_execution_role()
   
   region = boto3.Session().region_name
   
   bucket='bucket-name' # Put your s3 bucket name here
   prefix = 'sagemaker/xgboost-mnist2' # Used as part of the path in the bucket where you store data
   # customize to your bucket where you will store data
   bucket_path = 'https://s3-{}.amazonaws.com/{}'.format(region,bucket)
   ```

## <a name="ex1-prepare-2"></a>

**Next Step**  
[Step 4: Download, Explore, and Transform the Training Data](ex1-preprocess-data.md)