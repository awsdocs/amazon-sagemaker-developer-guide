# Manage your environment<a name="studio-lab-use-manage"></a>

 Your Amazon SageMaker Studio Lab environment comes with a base image installed that includes key packages and resources\. You can customize your environment by adding new packages and libraries to it\. You can also create new environments from Studio Lab, import compatible environments, and reset your environment to create space\. 

**Topics**
+ [Base image](#studio-lab-use-manage-base)
+ [Managing Conda environments](#studio-lab-use-manage-conda)

## Base image<a name="studio-lab-use-manage-base"></a>

 The default Amazon SageMaker Studio Lab base image includes the following packages\.
+  Python 3\.9 
+  bzip2 
+  build\-essential 
+  curl 
+  git 
+  libgl1\-mesa\-glx 
+  nano 
+  rsync 
+  unzip 
+  wget 
+  ca\-certificates 
+  pip 
+  ipykernel\-6\.4 

 **Supported ML frameworks and libraries** 

Machine learning frameworks simplify machine learning by abstracting complex algorithms and processes\. This abstraction helps you get started with machine learning\. Libraries are collections of files, programs, and other resources that you can use in your code\. Studio Lab supports the following frameworks and libraries, which you must install manually\.
+  PyTorch 1\.9 
+  TensorFlow 1\.15 and 2\.6 
+  MxNet 1\.8 
+  Hugging Face 
+  AutoGluon 0\.3\.1 
+  Scikit\-learn 0\.24 
+  PyTorch ecosystem 
+  OpenCV 
+  scipy 
+  numpy 

For a list of all of the packages currently installed in your environment, run the following command from your Jupyter notebook\.

```
%pip list
```

## Managing Conda environments<a name="studio-lab-use-manage-conda"></a>

 The following sections give information about your default Conda environment, how to customize it, and how to add new Conda environments\. For more information about Conda environments, see [Conda environments](https://docs.conda.io/projects/conda/en/latest/user-guide/concepts/environments.html)\. For a list of sample environments that you can install into Studio Lab, see [Creating Custom Conda Environments](https://github.com/aws/studio-lab-examples/tree/main/custom-environments)\. To use these sample environment YAML files with Studio Lab, see [Step 4 – Import your Studio Lab Conda environments in Studio](studio-lab-use-migrate.md#studio-lab-use-migrate-step4)\. 

### Your default environment<a name="studio-lab-use-manage-conda-default"></a>

 Studio Lab uses Conda environments to encapsulate the software packages that are needed to run notebooks\. Your project contains a default Conda environment, named `default`, with the [IPython kernel](https://ipython.readthedocs.io/en/stable/)\. This environment serves as the default kernel for your Jupyter notebooks\.

### Customize your environment<a name="studio-lab-use-manage-conda-default-customize"></a>

 You can customize your environment by installing extensions and packages, as needed\. You do not need to install your packages every time you work on your project\. Any installed extensions and packages persist in your project\.

**Note**  
Installed packages count against your 15 GB of instance storage\.

 To install additional packages to your environment from a Jupyter notebook, insert one of the following commands in a cell at the top of your notebook\. These commands install packages in the environment used by that notebook\. Any packages that you install are saved in your persistent project directory\.  
+  `%conda install <PACKAGE>` 
+  `%pip install <PACKAGE>` 

We don't recommend using the `!pip` or `!conda` commands because they can behave in unexpected ways when you have multiple environments\. After you install new packages to your environment, restart the kernel to ensure that the packages work in your notebook\.

### Create and activate new Conda environments<a name="studio-lab-use-manage-conda-new-conda"></a>

If you would like to maintain multiple environments for different use cases, you can create new Conda environments in your project\. The following sections show how to create and activate new Conda environments\. For a Jupyter notebook that shows how to create a custom environment, see [Setting up a Custom Environment in SageMaker Studio Lab](https://github.com/aws/studio-lab-examples/blob/main/custom-environments/custom_environment.ipynb)\.

**Note**  
Maintaining multiple environments in your project decreases your available memory\.

 **Create** 

 To create a new Conda environment, run the following Conda command from your terminal\. This example creates a new environment with Python 3\.9\. 

```
$ conda env create --name <ENVIRONMENT_NAME> python=3.9
```

 **Activate** 

 To activate any Conda environment, run the following command in the terminal\.

```
$ conda activate <ENVIRONMENT_NAME>
```

 When you run this command, any packages installed using Conda or pip are installed in the environment\. 

 To use your new Conda environments with notebooks, make sure the `ipykernel` package is installed in the environment\.

```
$ conda install ipykernel
```

 After you have created the environment, you can select it as the kernel for your notebook\. 

### Installing JupyterLab and Jupyter Server extensions<a name="studio-lab-use-manage-conda-jupyter"></a>

 You can install open\-source JupyterLab and Jupyter Server extensions in Studio Lab\. These extensions are typically Python packages that are installed using `conda` or `pip`\.  

 The following steps show how to install these extensions\. 

1.  Open the terminal and activate the `studiolab` environment\. 

   ```
   $ conda activate studiolab
   ```

1.  Install the JupyterLab or Jupyter Server extension\. 

   ```
   $ conda install <JUPYTER_EXTENSION>
   ```

1.  Navigate to the Studio Lab project overview page\. 

1. Select **Stop runtime**\.

1. Select **Start runtime**\.

### Reset environment<a name="studio-lab-use-manage-conda-reset"></a>

 To remove all of your files and reset your project, run the following command from the terminal\. 

```
rm -rf .*
```

 The following command deletes a Conda environment from your project\. 

```
conda remove --name <ENVIRONMENT_NAME> --all
```