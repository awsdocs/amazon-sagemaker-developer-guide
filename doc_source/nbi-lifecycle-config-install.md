# Lifecycle Configuration Best Practices<a name="nbi-lifecycle-config-install"></a>

The following are best practices for using lifecycle configurations:
+ Lifecycle configurations run as the `root` user\. If your script makes any changes within the `/home/ec2-user/SageMaker` directory, \(for example, installing a package with `pip`\), use the command `sudo -u ec2-user` to run as the `ec2-user` user\. This is the same user that Amazon SageMaker runs as\.
+ SageMaker notebook instances use `conda` environments to implement different kernels for Jupyter notebooks\. If you want to install packages that are available to one or more notebook kernels, enclose the commands to install the packages with `conda` environment commands that activate the conda environment that contains the kernel where you want to install the packages\.

  For example, if you want to install a package only for the `python3` environment, use the following code:

  ```
  #!/bin/bash
  sudo -u ec2-user -i <<'EOF'
  
  # This will affect only the Jupyter kernel called "conda_python3".
  source activate python3
  
  # Replace myPackage with the name of the package you want to install.
  pip install myPackage
  # You can also perform "conda install" here as well.
  
  source deactivate
  
  EOF
  ```

  If you want to install a package in all conda environments in the notebook instance, use the following code:

  ```
  #!/bin/bash
  sudo -u ec2-user -i <<'EOF'
  
  # Note that "base" is special environment name, include it there as well.
  for env in base /home/ec2-user/anaconda3/envs/*; do
      source /home/ec2-user/anaconda3/bin/activate $(basename "$env")
  
      # Installing packages in the Jupyter system environment can affect stability of your SageMaker
      # Notebook Instance.  You can remove this check if you'd like to install Jupyter extensions, etc.
      if [ $env = 'JupyterSystemEnv' ]; then
        continue
      fi
  
      # Replace myPackage with the name of the package you want to install.
      pip install --upgrade --quiet myPackage
      # You can also perform "conda install" here as well.
  
      source /home/ec2-user/anaconda3/bin/deactivate
  done
  
  EOF
  ```
+ You must store all conda environments in the default environments folder \(/home/user/anaconda3/envs\)\.

**Important**  
When you create or change a script, we recommend that you use a text editor that provides Unix\-style line breaks, such as the text editor available in the console when you create a notebook\. Copying text from a non\-Linux operating system might introduce incompatible line breaks and result in an unexpected error\.