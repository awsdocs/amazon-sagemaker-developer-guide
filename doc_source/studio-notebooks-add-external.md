# Install External Libraries and Kernels in Amazon SageMaker Studio<a name="studio-notebooks-add-external"></a>

Amazon SageMaker Studio notebooks come with multiple images already installed\. These images contain kernels and Python packages including scikit, Pandas, NumPy, TensorFlow, and MXNet\. You can also install your own images that contain your choice of packages and kernels\. For more information on installing your own image, see [Bring your own SageMaker image](studio-byoi.md)\.

The different Jupyter kernels in Amazon SageMaker Studio notebooks are separate conda environments\. For information about conda environments, see [Managing environments](https://conda.io/docs/user-guide/tasks/manage-environments.html)\.

## Package installation tools<a name="nbi-add-external-tools"></a>

The method that you use to install Python packages from the terminal differs depending on the image\. Studio supports the following package installation tools:
+ **Notebooks** – The following commands are supported\. If one of the following does not work on your image, try the other one\.
  + `%conda install`
  + `%pip install`
+ **The Jupyter terminal** – You can install packages using pip and conda directly\. You can also use `apt-get install` to install system packages from the terminal\.

**Note**  
We do not recommend using `pip install -u` or `pip install --user`, because those commands install packages on the user's Amazon EFS volume and can potentially block JupyterServer app restarts\. Instead, use a lifecycle configuration to reinstall the required packages on app restarts as shown in [Install packages using lifecycle configurations](#nbi-add-external-lcc)\.

We recommend using `%pip` and `%conda` to install packages from within a notebook because they correctly take into account the activate environment or interpreter being used\. For more information, see [Add %pip and %conda magic functions](https://github.com/ipython/ipython/pull/11524)\. You can also use the system command syntax \(lines starting with \!\) to install packages\. For example, `!pip install` and `!conda install`\. 

### Conda<a name="nbi-add-external-tools-conda"></a>

Conda is an open source package management system and environment management system that can install packages and their dependencies\. SageMaker supports using conda with either of these two main channels: the default channel or the conda\-forge channel\. For more information, see [Conda channels](https://docs.conda.io/projects/conda/en/latest/user-guide/concepts/channels.html)\. The conda\-forge channel is a community channel where contributors can upload packages\.

**Note**  
Installing packages from conda\-forge can take up to 10 minutes\. Timing relates to how conda resolves the dependency graph\.

All of the SageMaker provided environments are functional\. User installed packages may not function correctly\.

Conda has two methods for activating environments: `conda activate`, and `source activate`\. For more information, see [Managing environment](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html)\.

**Supported conda operations**
+ `conda install` of a package in a single environment
+ `conda install` of a package in all environments
+ Installing a package from the main conda repository
+ Installing a package from conda\-forge
+ Changing the conda install location to use Amazon EBS
+ Supporting both `conda activate` and `source activate`

### Pip<a name="nbi-add-external-tools-pip"></a>

Pip is the tool for installing and managing Python packages\. Pip searches for packages on the Python Package Index \(PyPI\) by default\. Unlike conda, pip doesn't have built in environment support\. Therfore, pip isn't as thorough as conda when it comes to packages with native or system library dependencies\. Pip can be used to install packages in conda environments\. You can use alternative package repositories with pip instead of the PyPI\.

**Supported pip operations**
+ Using pip to install a package without an active conda environment
+ Using pip to install a package in a conda environment
+ Using pip to install a package in all conda environments
+ Changing the pip install location to use Amazon EBS
+ Using an alternative repository to install packages with pip

### Unsupported<a name="nbi-add-external-tools-misc"></a>

SageMaker aims to support as many package installation operations as possible\. However, if the packages were installed by SageMaker and you use the following operations on these packages, it might make your environment unstable:
+ Uninstalling
+ Downgrading
+ Upgrading

Due to potential issues with network conditions or configurations, or the availability of conda or PyPi, packages may not install in a fixed or deterministic amount of time\.

**Note**  
Attempting to install a package in an environment with incompatible dependencies can result in a failure\. If issues occur, you can contact the library maintainer about updating the package dependencies\. When you modify the environment, such as removing or updating existing packages, this may result in instability of that environment\.

## Install packages using lifecycle configurations<a name="nbi-add-external-lcc"></a>

Install custom images and kernels on the Studio instance's Amazon EBS volume so that they persist when you stop and restart the notebook, and that any external libraries you install are not updated by SageMaker\. To do that, use a lifecycle configuration that includes both a script that runs when you create the notebook \(`on-create)` and a script that runs each time you restart the notebook \(`on-start`\)\. For more information about using lifecycle configurations with Studio, see [Use Lifecycle Configurations with Amazon SageMaker Studio](studio-lcc.md)\. For sample lifecycle configuration scripts, see [SageMaker Studio Lifecycle Configuration Samples](https://github.com/aws-samples/sagemaker-studio-lifecycle-config-examples)\.