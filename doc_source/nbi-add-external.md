# Installing External Libraries and Kernels in Notebook Instances<a name="nbi-add-external"></a>

Amazon SageMaker notebook instances come with multiple environments already installed\. These environments contain Jupyter kernels and Python packages including: scikit, Pandas, NumPy, TensorFlow, and MXNet\. These environments, along with all files in the `sample-notebooks` folder, are refreshed when you stop and start a notebook instance\. You can also install your own environments that contain your choice of packages and kernels\. This is typically done using `conda install` or `pip install`\.

**For example, to install the R kernel:**

1. Open a notebook instance\.

1. In the Jupyter dashboard, choose **New**, and then choose **Terminal**\.

1. In the terminal, type the following command:

   ```
   conda install --yes --name JupyterSystemEnv --channel r r-essentials=1.6.0
   ```

1. When the command completes, exit from the terminal, and refresh the Jupyter dashboard page\.

1. In the Jupyter dashboard page, choose **New**\. **R** now appears as a **Notebook** option\.

Notebook instances come with git installed\. You can use git to install projects from GitHub\.

**For example, to install the Scala kernel:**

1. Open a notebook instance\.

1. In the Jupyter dashboard, choose **New**, and then choose **Terminal**\.

1. In the terminal, type the following commands:

   ```
   git clone https://github.com/jupyter-scala/jupyter-scala.git
      cd jupyter-scala
      ./jupyter-scala
   ```

1. When the command completes, exit from the terminal, and refresh the Jupyter dashboard page\.

1. In the Jupyter dashboard page, choose **New**\. **Scala** now appears as a **Notebook** option\.

The different Jupyter kernels in Amazon SageMaker notebook instances are separate conda environments\. For information about conda environments, see [Managing environments](https://conda.io/docs/user-guide/tasks/manage-environments.html) in the *Conda* documentation\. If you want to use an external library in a specific kernel, install the library in the environment for that kernel\. You can do this either in the terminal or in a notebook cell\. The following procedures show how to install Theano so that you can use it in a notebook with a conda\_mxnet\_p36`` kernel\.

**To install Theano from a terminal:**

1. Open a notebook instance\.

1. In the Jupyter dashboard, choose **New**, and then choose **Terminal**\.

1. In the terminal, type the following commands:

   ```
   conda install -n mxnet_p36 -c conda-forge theano
      python
      import theano
   ```

**To install Theano from a Jupyter notebook cell:**

1. Open a notebook instance\.

1. In the Jupyter dashboard, choose **New**, and then choose **conda\_mxnet\_p36**\.

1. In a cell in the new notebook, type the following command:

   ```
   !pip install theano
   ```