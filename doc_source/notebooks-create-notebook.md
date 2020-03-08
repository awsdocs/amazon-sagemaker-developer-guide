# Create a Notebook<a name="notebooks-create-notebook"></a>


****  

|  | 
| --- |
| Amazon SageMaker Studio Notebooks is in preview release and is subject to change\. | 

 After you’re logged in to Amazon SageMaker Studio, you can create a notebook in the following ways: 
+ Using the JupyterLab launcher\. 
+ On the **File** menu, choose **New**, and then choose **Notebooks**\. 

**Environment Options**  
 Depending on your goals and processing requirements, SageMaker has a collection of pre\-built environments\.  
+ Data Science \(includes most common data science packages such as NumPy, SciKit Learn, and more\) 
+ Base Python \(a plain, vanilla Python environment\)  
+ MXNet CPU optimized 
+ TensorFlow CPU optimized 
+ PyTorch CPU optimized 

## Use the File Menu to Create a New Notebook<a name="notebooks-create-notebook-file-menu"></a>

 When you create a new notebook from the File menu, you will be asked to choose a kernel from a dropdown first, before your notebook is created\. 

## Use the Launcher to Create a New Notebook<a name="notebooks-create-notebook-launcher"></a>

**Selecting an Environment**  
When you create a notebook, first you must select the environment\. Environments are the combination of software, such as frameworks, and other dependencies that your notebook needs to run\. For example, an environment could be TensorFlow with Python 3, Keras with an MXNet backend, or PyTorch and Python 3 customized with additional Python packages\. 

 When you’re logged in as an Amazon SageMaker Studio user, you can have one instance of each instance type\. Each instance can have only one environment running on it at a time\. However, an environment can run multiple kernels or terminal instances\. 

**Selecting a Kernel**  
 After selecting an environment, you need to select an kernel\. The options are a Python 3 kernel or console, or you can launch a Terminal\.  

 After you choose the kernel, your new notebook launches and opens in a JupyterLab interface\. Your notebook will be hosted in a t3\.medium instance by default\.  