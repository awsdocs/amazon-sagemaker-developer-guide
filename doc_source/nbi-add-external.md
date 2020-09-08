# Install External Libraries and Kernels in Notebook Instances<a name="nbi-add-external"></a>

Amazon SageMaker notebook instances come with multiple environments already installed\. These environments contain Jupyter kernels and Python packages including: scikit, Pandas, NumPy, TensorFlow, and MXNet\. These environments, along with all files in the `sample-notebooks` folder, are refreshed when you stop and start a notebook instance\. You can also install your own environments that contain your choice of packages and kernels\.

The different Jupyter kernels in Amazon SageMaker notebook instances are separate conda environments\. For information about conda environments, see [Managing environments](https://conda.io/docs/user-guide/tasks/manage-environments.html) in the *Conda* documentation\.

Install custom environments and kernels on the notebook instance's Amazon EBS volume\. This ensures that they persist when you stop and restart the notebook instance, and that any external libraries you install are not updated by SageMaker\. To do that, use a lifecycle configuration that includes both a script that runs when you create the notebook instance \(`on-create)` and a script that runs each time you restart the notebook instance \(`on-start`\)\. For more information about using notebook instance lifecycle configurations, see [Customize a Notebook Instance Using a Lifecycle Configuration Script](notebook-lifecycle-config.md)\. There is a GitHub repository that contains sample lifecycle configuration scripts at [SageMaker Notebook Instance Lifecycle Config Samples](https://github.com/aws-samples/amazon-sagemaker-notebook-instance-lifecycle-config-samples)\.

The examples at [https://github\.com/aws\-samples/amazon\-sagemaker\-notebook\-instance\-lifecycle\-config\-samples/blob/master/scripts/persistent\-conda\-ebs/on\-create\.sh](https://github.com/aws-samples/amazon-sagemaker-notebook-instance-lifecycle-config-samples/blob/master/scripts/persistent-conda-ebs/on-create.sh) and [https://github\.com/aws\-samples/amazon\-sagemaker\-notebook\-instance\-lifecycle\-config\-samples/blob/master/scripts/persistent\-conda\-ebs/on\-start\.sh](https://github.com/aws-samples/amazon-sagemaker-notebook-instance-lifecycle-config-samples/blob/master/scripts/persistent-conda-ebs/on-start.sh) show the best practice for installing environments and kernels on a notebook instance\. The `on-create` script installs the `ipykernel` library so that you can use create custom environments as Jupyter kernels, and then uses `pip install` and `conda install` to install libraries\. You can adapt the script to create custom environments and install libraries that you want\. SageMaker does not update these libraries when you stop and restart the notebook instance, so you can ensure that your custom environment has specific versions of libraries that you want\. The `on-start` script installs any custom environments that you create as Jupyter kernels, so that they appear in the dropdown list in the Jupyter **New** menu\.

## Package installation tools<a name="nbi-add-external-tools"></a>

SageMaker notebooks support the following package installation tools:
+ conda install
+ pip install

You can install packages using the following methods:
+ Lifecycle configuration scripts\.

  For example scripts, see [SageMaker Notebook Instance Lifecycle Config Samples](https://github.com/aws-samples/amazon-sagemaker-notebook-instance-lifecycle-config-samples)\. For more information on lifecycle configuration, see [Customize a Notebook Instance Using a Lifecycle Configuration Script](https://docs.aws.amazon.com/notebook-lifecycle-config.html)\.
+ Notebooks – The following commands are supported\. The `% install` commands are recommended\.
  + `%conda install`
  + `%pip install`
  + `!conda install`
  + `!pip install`
+ The Jupyter terminal – You can install packages using pip and conda directly\.

From within a notebook you can use the system command syntax \(lines starting with \!\) to install packages, for example, `!pip install` and `!conda install`\. More recently, new commands have been added to IPython: `%pip` and `%conda`\. These commands are the recommended way to install packages from a notebook as they correctly take into account the activate environment or interpreter being used\. For more information, see [Add %pip and %conda magic functions](https://github.com/ipython/ipython/pull/11524)\.

### Conda<a name="nbi-add-external-tools-conda"></a>

Conda is an open source package management system and environment management system, which can install packages and their dependencies\. SageMaker supports using Conda with either of the two main channels, the default channel, and the conda\-forge channel\. For more information, see [Conda channels](https://docs.conda.io/projects/conda/en/latest/user-guide/concepts/channels.html)\. The conda\-forge channel is a community channel where contributors can upload packages\.

**Note**  
Due to how Conda resolves the dependency graph, installing packages from conda\-forge can take significantly longer \(in the worst cases, upwards of 10 minutes\)\.

The Deep Learning AMI comes with many conda environments and many packages preinstalled\. Due to the number of packages preinstalled, finding a set of packages that are guaranteed to be compatible is difficult\. You may see a warning "The environment is inconsistent, please check the package plan carefully"\. Despite this warning, SageMaker ensures that all the SageMaker provided environments are correct\. SageMaker cannot guarantee that any user installed packages will function correctly\.

Conda has two methods for activating environments: conda activate/deactivate, and source activate/deactivate\. For more information, see [Should I use 'conda activate' or 'source activate' in Linux](https://stackoverflow.com/questions/49600611/python-anaconda-should-i-use-conda-activate-or-source-activate-in-linux)\.

SageMaker supports moving Conda environments onto the Amazon EBS volume, which is persisted when the instance is stopped\. The environments aren't persisted when the environments are installed to the root volume, which is the default behavior\. For an example lifecycle script, see [persistent\-conda\-ebs](https://github.com/aws-samples/amazon-sagemaker-notebook-instance-lifecycle-config-samples/tree/master/scripts/persistent-conda-ebs)\.

**Supported conda operations \(see note at the bottom of this topic\)**
+ conda install of a package in a single environment
+ conda install of a package in all environments
+ conda install of a R package in the R environment
+ Installing a package from the main conda repository
+ Installing a package from conda\-forge
+ Changing the Conda install location to use EBS
+ Supporting both conda activate and source activate

### Pip<a name="nbi-add-external-tools-pip"></a>

Pip is the de facto tool for installing and managing Python packages\. Pip searches for packages on the Python Package Index \(PyPI\) by default\. Unlike Conda, pip doesn't have built in environment support, and is not as thorough as Conda when it comes to packages with native/system library dependencies\. Pip can be used to install packages in Conda environments\.

You can use alternative package repositories with pip instead of the PyPI\. For an example lifecycle script, see [on\-start\.sh](https://github.com/aws-samples/amazon-sagemaker-notebook-instance-lifecycle-config-samples/blob/master/scripts/add-pypi-repository/on-start.sh)\.

**Supported pip operations \(see note at the bottom of this topic\)**
+ Using pip to install a package without an active conda environment \(install packages system wide\)
+ Using pip to install a package in a conda environment
+ Using pip to install a package in all conda environments
+ Changing the pip install location to use EBS
+ Using an alternative repository to install packages with pip

### Unsupported<a name="nbi-add-external-tools-misc"></a>

SageMaker aims to support as many package installation operations as possible\. However, if the packages were installed by SageMaker or DLAMI, and you use the following operations on these packages, it might make your notebook instance unstable:
+ Uninstalling
+ Downgrading
+ Upgrading

We do not provide support for installing packages via yum install or installing R packages from CRAN\.

Due to potential issues with network conditions or configurations, or the availability of Conda or PyPi, we cannot guarantee that packages will install in a fixed or deterministic amount of time\.

**Note**  
We cannot guarantee that a package installation will be successful\. Attempting to install a package in an environment with incompatible dependencies can result in a failure\. In such a case you should contact the library maintainer to see if it is possible to update the package dependencies\. Alternatively you can attempt to modify the environment in such a way as to allow the installation\. This modification however will likely mean removing or updating existing packages, which means we can no longer guarantee stability of this environment\.