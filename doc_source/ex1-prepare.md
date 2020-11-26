# Step 3: Create a Jupyter Notebook<a name="ex1-prepare"></a>

You can create a Jupyter notebook in the notebook instance you created in [Step 2: Create an Amazon SageMaker Notebook Instance](gs-setup-working-env.md), and create a cell that gets the IAM role that your notebook needs to run Amazon SageMaker APIs and specifies the name of the Amazon S3 bucket that you will use to store the datasets that you use for your training data and the model artifacts that a SageMaker training job outputs\.

**To create a Jupyter notebook**

1. Open the notebook instance\.

   1. Sign in to the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

   1. Open the Notebook Instances, and then open the notebook instance you created by choosing either **Open Jupyter** for classic Juypter view or **Open JupyterLab** for JupyterLab view next to the name of the notebook instance\.
**Note**  
If you see **Pending** to the right of the notebook instance in the **Status** column, your notebook is still being created\. The status will change to **InService** when the notebook is ready for use\. 

1. Create a notebook\. 

   1. If you opened the notebook in Jupyter classic view, on the **Files** tab, choose **New**, and **conda\_python3**\. This preinstalled environment includes the default Anaconda installation and Python 3\.

   1. If you opened the notebook in JupyterLab view, on the **File** menu, choose **New**, and then choose **Notebook**\. For **Select Kernel**, choose **conda\_python3**\. This preinstalled environment includes the default Anaconda installation and Python 3\.

1. In the Jupyter notebook, choose **File** and **Save as**, and name the notebook\.

## <a name="ex1-prepare-2"></a>

**Next Step**  
[Step 4: Download, Explore, and Transform the Training Data](ex1-preprocess-data.md)