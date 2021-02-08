# Step 2: Create a Jupyter Notebook<a name="ex1-prepare"></a>

To start scripting for training and deploying your model, create a Jupyter notebook in the SageMaker notebook instance\. Using the Jupyter notebook, you can conduct machine learning \(ML\) experiments for training and inference while accessing the SageMaker features and the AWS infrastructure\.

**To create a Jupyter notebook**  
![\[Animated screenshot that shows how to create a new Jupyter notebook in the SageMaker notebook instance.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/get-started-ni/gs-ni-create-notebook.gif)

1. Open the notebook instance as follows:

   1. Sign in to the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

   1. On the **Notebook instances** page, open your notebook instance by choosing either **Open JupyterLab** for the JupyterLab interface or **Open Jupyter** for the classic Jupyter view\.
**Note**  
If the notebook instance status shows **Pending** in the **Status** column, your notebook instance is still being created\. The status will change to **InService** when the notebook instance is ready for use\. 

1. Create a notebook as follows: 
   + If you opened the notebook in the JupyterLab view, on the **File** menu, choose **New**, and then choose **Notebook**\. For **Select Kernel**, choose **conda\_python3**\. This preinstalled environment includes the default Anaconda installation and Python 3\.
   + If you opened the notebook in the classic Jupyter view, on the **Files** tab, choose **New**, and then choose **conda\_python3**\. This preinstalled environment includes the default Anaconda installation and Python 3\.

1. Save the notebooks as follows:
   + In the JupyterLab view, choose **File**, choose **Save Notebook As\.\.\.**, and then rename the notebook\.
   + In the Jupyter classic view, choose **File**, choose **Save as\.\.\.**, and then rename the notebook\.