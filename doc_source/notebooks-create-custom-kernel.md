# Create a Custom Kernel<a name="notebooks-create-custom-kernel"></a>

You can create a custom kernel in any of the SageMaker images\. The kernel will display in the Amazon SageMaker Studio Launcher when the SageMaker image that hosts the kernel is chosen\. The kernel will also display in the **Select Kernel** dialog when changing a notebook's kernel\.

Conda must be installed in the SageMaker image in order to create the kernel\. The `Data Science` SageMaker image comes with Conda preinstalled\. To create a kernel in any of the other SageMaker images, you must first install Conda\. For more information, see [Conda Installation](https://conda.io/projects/conda/en/latest/user-guide/install/index.html)\.

A kernel is defined by a Conda environment\. For more information about Conda environments, see [Managing environments](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html)\.

**To create a custom kernel**

1. Choose **File** and then **New Launcher** \(`Ctrl + Shift + L`\)\.

1. From the Studio Launcher, choose the SageMaker image that will host the kernel\.

1. Choose **Image Terminal**\.

   The terminal opens inside the chosen SageMaker image\.

1. If the chosen SageMaker image doesn't have Conda installed, run the following command to install Conda\.

   ```
   wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh && bash Miniconda3-latest-Linux-x86_64.sh -b -p /tmp/miniconda3
   ```

1. Run the following command to create the kernel\. We're showing `IPython` as it's available on all Jupyter notebooks\. Refer to the [Conda create](https://conda.io/projects/conda/en/latest/commands/create.html) documentation for other options\.

   ```
   conda create --name KernelName ipykernel
   ```

   You can use the `--yes` option to ignore the package installation confirmation\.

**Note**  
You don't need to activate the kernel after creating it\.

The kernel isn't persisted when the SageMaker image hosting the kernel is shutdown\. To save the kernel, export the kernel as a YML file and save the file to your Amazon EFS volume\.

**To export and recreate a custom kernel**

1. Run the following command to export the kernel as a YML file\.

   ```
   conda env export --name KernelName --file ~/KernelFileName.yml
   ```

1. Run the following command to recreate the kernel from the YML file\.

   ```
   conda env create --file ~/KernelFileName.yml
   ```