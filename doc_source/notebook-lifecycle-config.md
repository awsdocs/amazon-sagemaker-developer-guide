# Step 2\.1: \(Optional\) Customize a Notebook Instance<a name="notebook-lifecycle-config"></a>

To install packages or sample notebooks on your notebook instance, configure networking and security for it, or otherwise use a shell script to customize it, use a lifecycle configuration\. A *lifecycle configuration* provides shell scripts that run only when you create the notebook instance or whenever you start one\. When you create a notebook instance, you can create a new lifecycle configuration and the scripts it uses or apply one that you already have\.

**Note**  
Each script has a limit of 16384 characters\.  
The value of the `$PATH` environment variable that is available to both scripts is `/sbin:bin:/usr/sbin:/usr/bin`\.  
View CloudWatch Logs for notebook instance lifecycle configurations in log group `/aws/sagemaker/NotebookInstances` in log stream `[notebook-instance-name]/[LifecycleConfigHook]`\.  
Scripts cannot run for longer than 5 minutes\. If a script runs for longer than 5 minutes, it fails and the notebook instance is not created or started\.

**To create a lifecycle configuration**

1. For **Lifecycle configuration \- *Optional***, choose **Create a new lifecycle configuration**\.

1. For **Name**, type a name\.

1. \(Optional\) To create a script that runs when you create the notebook and every time you start it, choose **Start notebook**\.

1. In the **Start notebook** editor, type the script\.

1. \(Optional\) To create a script that runs only once, when you create the notebook,, choose **Create notebook**\.

1. In the **Create notebook** editor, type the script configure networking\.

1. Choose **Create configuration**\.

You can see a list of notebook instance lifecycle configurations you previously created by choosing **Lifecycle configuration** in the Amazon SageMaker console\. From there, you can view, edit, delete existing lifecycle configurations\. You can create a new notebook instance lifecycle configuration by choosing **Create configuration**\. These notebook instance lifecycle configurations are available when you create a new notebook instance\.

## Lifecycle Configuration Best Practices<a name="nbi-lifecycle-config-install"></a>

The following are best practices for using lifecycle configurations:
+ Lifecycle configurations run as the `root` user\. If your script makes any changes within the `/home/ec2-user/SageMaker` directory, \(for example, installing a packge with `pip`\), use the command `sudo -u ec2-user` command to run as the `ec2-user` user\. This is the same user that Amazon SageMaker runs as\.
+ Amazon SageMaker notebook instances use `conda` environments to implement different kernels for Jupyter notebooks\. If you want to install packages that are available to one or more notebook kernels, enclose the commands to install the packages with `conda` environment commands that activate the conda environment that contains the kernel where you want to install the packages\.

  For example, if you want to install a package only in for the `python3` environment, use the following code:

  ```
  sudo -u ec2-user -i <<'EOF'
  
  # This will affect only the Jupyter kernel called "conda_python3".
  source activate python3
  
  # Replace myPackage with the name of the package you want to install..
  pip install myPackage
  # You can also perform "conda install" here as well.
  
  source deactivate
  
  EOF
  ```

  If you want to install a package in all conda environments in the notebook instance, use the following code:

  ```
  sudo -u ec2-user -i <<'EOF'
  
  # Note that "base" is special environment name, include it there as well.
  for env in base /home/ec2-user/anaconda3/envs/*; do
      source /home/ec2-user/anaconda3/bin/activate $(basename "$env")
  
      # Installing packages in the Jupyter system environment can affect stability of your SageMaker
      # Notebook Instance.  You can remove this check if you'd like to install Jupyter extensions, etc.
      if [ $env != 'JupyterSystemEnv' ]; then
        continue
      fi
  
      # Replace myPackage with the name of the package you want to install.
      pip install --upgrade --quiet myPackage
      # You can also perform "conda install" here as well.
  
      source /home/ec2-user/anaconda3/bin/deactivate
  done
  
  EOF
  ```

## Next Step<a name="gs-setup-working-env-nextstep"></a>

You are now ready to train your first model\. For step\-by\-step instructions, see [Step 3: Train a Model with a Built\-in Algorithm and Deploy It](ex1.md)\.