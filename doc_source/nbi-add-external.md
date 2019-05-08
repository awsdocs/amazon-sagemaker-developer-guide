# Install External Libraries and Kernels in Notebook Instances<a name="nbi-add-external"></a>

Amazon SageMaker notebook instances come with multiple environments already installed\. These environments contain Jupyter kernels and Python packages including: scikit, Pandas, NumPy, TensorFlow, and MXNet\. These environments, along with all files in the `sample-notebooks` folder, are refreshed when you stop and start a notebook instance\. You can also install your own environments that contain your choice of packages and kernels\. This is typically done using `conda install` or `pip install`\.

The different Jupyter kernels in Amazon SageMaker notebook instances are separate conda environments\. For information about conda environments, see [Managing environments](https://conda.io/docs/user-guide/tasks/manage-environments.html) in the *Conda* documentation\. If you want to use an external library in a specific kernel, install the library in the environment for that kernel\. You can do this either in the terminal or in a notebook cell\. The following procedures show how to install Theano so that you can use it in a notebook with a conda\_mxnet\_p36`` kernel\.

**To install Theano from a terminal**

1. Open a notebook instance\.

1. In the Jupyter dashboard, choose **New**, and then choose **Terminal**\.

1. In the terminal, type the following commands:

   ```
   conda install -n mxnet_p36 -c conda-forge theano
      python
      import theano
   ```

**To install Theano from a Jupyter notebook cell**

1. Open a notebook instance\.

1. In the Jupyter dashboard, choose **New**, and then choose **conda\_mxnet\_p36**\.

1. In a cell in the new notebook, type the following command:

   ```
   !pip install theano
   ```

## Maintain a Sandboxed Python Environment<a name="nbi-isolated-environment"></a>

Amazon SageMaker periodically updates the Python and dependency versions in the environments installed on a notebook instance\. To maintain an isolated Python environment that does not change versions, create a lifecycle configuration that runs each time you start your notebook instance\. For information about creating lifecycle configurations, see [Customize a Notebook Instance ](notebook-lifecycle-config.md)\.

The following example lifecycle configuration script installs Miniconda on your notebook instance\. This allows you to create environments in your notebook instance with specific versions of Python and dependencies that Amazon SageMaker does not update:

```
#!/bin/bash

set -e

WORKING_DIR=/home/ec2-user/.myproject
mkdir -p "$WORKING_DIR"

# Install Miniconda to get a separate python and pip
wget https://repo.anaconda.com/miniconda/Miniconda3-4.5.12-Linux-x86_64.sh -O "$WORKING_DIR/miniconda.sh"

# Install Miniconda into the working directory
bash "$WORKING_DIR/miniconda.sh" -b -u -p "$WORKING_DIR/miniconda"

# Install pinned versions of any dependencies
source "$WORKING_DIR/miniconda/bin/activate"
pip install boto3==1.9.86
pip install requests==2.21.0

# Bootstrapping code

# Cleanup
source "$WORKING_DIR/miniconda/bin/deactivate"
rm -rf "$WORKING_DIR/miniconda.sh"
```

You can also add a sandboxed Python installation as a kernel that you can use in a Jupyer notebook by including the following code to the above lifecycle configuration:

```
source "$WORKING_DIR/miniconda/bin/activate" 

# If required, add this as a kernel
pip install ipykernel 
python -m ipykernel install --user --name MyProjectEnv --display-name "Python (myprojectenv)"

source "$WORKING_DIR/miniconda/bin/deactivate"
```