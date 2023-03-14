# Export an Amazon SageMaker Studio Lab environment to Amazon SageMaker Studio<a name="studio-lab-use-migrate"></a>

Amazon SageMaker Studio offers many features for machine learning and deep learning work flows that are unavailable in Amazon SageMaker Studio Lab\. This page shows how to migrate a Studio Lab environment to Studio to take advantage of more compute capacity, storage, and features\. However, you may want to familiarize yourself with Studio's prebuilt containers, which are optimized for the full MLOP pipeline\. For more information, see [Amazon SageMaker Studio Lab](studio-lab.md)

To migrate your Studio Lab environment to Studio, you must first onboard to Studio following the steps in [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\. 

**Topics**
+ [Step 1: Export your Studio Lab Conda environment](#studio-lab-use-migrate-step1)
+ [Step 2: Save your Studio Lab artifacts](#studio-lab-use-migrate-step2)
+ [Step 3: Import your Studio Lab artifacts to Studio](#studio-lab-use-migrate-step3)
+ [Step 4: Install your Studio Lab Conda environments in Studio](#studio-lab-use-migrate-step4)

## Step 1: Export your Studio Lab Conda environment<a name="studio-lab-use-migrate-step1"></a>

You can export a Conda environment and add libraries or packages to the environment by following the steps in [Manage your environment](studio-lab-use-manage.md)\. The following example demonstrates using the `default` environment to be exported to Studio\. 

1. Open the Studio Lab terminal by opening the **File Browser** \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/icons/folder.png)\) panel, choose the plus \(**\+**\) sign on the menu at the top of the file browser to open the **Launcher**, then choose **Terminal**\. From the Studio Lab terminal, list the Conda environments by running the following\.

   ```
   conda env list
   ```

   This command outputs a list of the Conda environments and their locations in the file system\. When you onboard to Studio Lab, you automatically activate the `studiolab`  Conda environment\.

   ```
   # conda environments: #
              default                  /home/studio-lab-user/.conda/envs/default
              studiolab             *  /home/studio-lab-user/.conda/envs/studiolab
              studiolab-safemode       /opt/amazon/sagemaker/safemode-home/.conda/envs/studiolab-safemode
              base                     /opt/conda
   ```

   We recommend that you do not export the `studiolab`, `studiolab-safemode`, and `base` environments\. These environments are not usable in Studio for the following reasons: 
   +  `studiolab`: This sets up the JupyterLab environment for Studio Lab\. Studio Lab runs a different major version of JupyterLab than Studio, so it is not usable in Studio\. 
   +  `studiolab-safemode`: This also sets up the JupyterLab environment for Studio Lab\. Studio Lab runs a different major version of JupyterLab than Studio, so it is not usable in Studio\. 
   +  `base`: This environment comes with Conda by default\. The `base` environment in Studio Lab and the `base` environment in Studio have incompatible versions of many packages\. 

1. For the Conda environment that you want to migrate to Studio, first activate the Conda environment\.The `default` environment is then changed when new libraries are installed or removed from it\. To get the exact state of the environment, export it into a YAML file using the command line\. The following command lines export the default environment into a YAML file, creating a file called `myenv.yml`\.

   ```
   conda activate default
   conda env export > ~/myenv.yml
   ```

## Step 2: Save your Studio Lab artifacts<a name="studio-lab-use-migrate-step2"></a>

Now that you have saved your environment to a YAML file, you can move the environment file to any platform\. 

------
#### [ Save to a local machine using Studio Lab GUI ]

**Note**  
Downloading a directory from the Studio Lab GUI by right\-clicking on the directory is currently unavailable\. If you wish to export a directory, please follow the steps using the **Save to Git repository** tab\. 

One option is to save the environment onto your local machine\. To do this, use the following procedure\.

1. In Studio Lab, choose the **File Browser** \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/icons/folder.png)\) panel so that the **File Browser** panel shows on the left panel\. 

1. Navigate to your user directory by choosing the file icon beneath the file search bar\. 

1. Choose \(right\-click\) the `myenv.yml` file and then choose **Download**\. You can repeat this process for other files you want to import to Studio\. 

------
#### [ Save to a Git repository ]

Another option is to save your environment to a Git repository\. This option uses GitHub as an example\. These steps require a GitHub account and repository\. For more information, visit [GitHub](https://github.com/)\. The following procedure shows how to synchronize your content with GitHub using the Studio Lab terminal\. 

1. From the Studio Lab terminal, navigate to your user directory and make a new directory to contain the files you want to export\. 

   ```
   cd ~
   mkdir <NEW_DIRECTORY_NAME>
   ```

1. After you create a new directory, copy any file or directory you want to export to `<NEW_DIRECTORY_NAME>`\. 

   Copy a file using the following code format:

   ```
   cp <FILE_NAME> <NEW_DIRECTORY_NAME>
   ```

   For example, replace `<FILE_NAME>` with `myenv.yml`\. 

   Copy any directory using the following code format:

   ```
   cp -r <DIRECTORY_NAME> <NEW_DIRECTORY_NAME>
   ```

    For example, replace `<DIRECTORY_NAME>` with any directory name in your user directory\.

1.  Navigate to the new directory and initialize the directory as a Git repository using the following command\. For more information, see the [git\-init documentation](https://git-scm.com/docs/git-init)\. 

   ```
   cd <NEW_DIRECTORY_NAME>
   git init
   ```

1.  Using Git, add all relevant files and then commit your changes\. 

   ```
   git add .
   git commit -m "<COMMIT_MESSAGE>"
   ```

   For example, replace `<COMMIT_MESSAGE>` with `Add Amazon SageMaker Studio Lab artifacts to GitHub repository to migrate to Amazon SageMaker Studio `\.

1.  Push the commit to your remote repository\. This repository has the format `https://github.com/<GITHUB_USERNAME>/ <REPOSITORY_NAME>.git` where `<GITHUB_USERNAME>` is your GitHub user name and the `<REPOSITORY_NAME>` is your remote repository name\. Create a branch `<BRANCH_NAME>` to push the content to the GitHub repository\.

   ```
   git branch -M <BRANCH_NAME>
   git remote add origin https://github.com/<GITHUB_USERNAME>/<REPOSITORY_NAME>.git
   git push -u origin <BRANCH_NAME>
   ```

------

## Step 3: Import your Studio Lab artifacts to Studio<a name="studio-lab-use-migrate-step3"></a>

The following procedure shows how to import artifacts to Studio\. You first have to open Amazon SageMaker Studio\. For more information, see [Launch Amazon SageMaker Studio](studio-launch.md)\. 

 From Studio, you can import files from your local machine or from a Git repository\. You can do this using the Studio GUI or terminal\. The following procedure uses the examples from [Step 2: Save your Studio Lab artifacts](#studio-lab-use-migrate-step2)\. 

------
#### [ Import using the Studio GUI ]

If you saved the files to your local machine, you can import the files to Studio using the following steps\.

1.  Open the **File Browser** \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/icons/folder.png)\) panel at the top left of Studio\. 

1. Choose the **Upload Files** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/File_upload_squid.png)\) on the menu at the top of the **File Browser** panel\. 

1. Navigate to the file you want to import, then choose **Open**\. 

**Note**  
If you wish to import a directory into Studio, first compress the directory on your local machine to a file\. On a Mac, right\-click the directory and choose **Compress "*<DIRECTORY\_NAME>*"**\. In Windows, right\-click the directory and choose **Send to**, and then choose **Compressed \(zipped\) folder**\. After the directory is compressed, import the compressed file using the preceding steps\. Unzip the compressed file by navigating to the Studio terminal and running the command `<DIRECTORY_NAME>.zip`\. 

------
#### [ Import using a Git repository ]

This example provides two options for how to clone a GitHub repository into Studio\. You can use the Studio GUI by choosing the **Git** \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/icons/git.png)\) tab on the left side of Studio\. Choose **Clone a Repository**, then paste your GitHub repository URL from [Step 2: Save your Studio Lab artifacts](#studio-lab-use-migrate-step2)\. Another option is to use the Studio terminal by using the following procedure\. 

1. Open the Studio **Launcher**\. For more information on opening the **Launcher**, see [Amazon SageMaker Studio Launcher](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-launcher.html)\. 

1. In the **Launcher**, in the **Notebooks and compute resources** section, choose **Change environment**\.

1. In Studio, open the **Launcher**\. To open the **Launcher**, choose **Amazon SageMaker Studio** at the top\-left corner of Studio\. 

   To learn about all the available ways to open the **Launcher**, see [Use the Amazon SageMaker Studio Launcher](studio-launcher.md)\.

1. In the **Change environment** dialog, use the **Image** dropdown list to select the **Data Science** image and choose **Select**\. This image comes with Conda pre\-installed\. 

1. In the Studio **Launcher**, choose **Open image terminal**\.

1. From the image terminal, run the following command to clone your repository\. This command creates a directory named after `<REPOSITORY_NAME>` in your Studio instance and clones your artifacts in that repository\.

   ```
   git clone https://github.com/<GITHUB_USERNAME>/<REPOSITORY_NAME>.git
   ```

------

## Step 4: Install your Studio Lab Conda environments in Studio<a name="studio-lab-use-migrate-step4"></a>

You can now recreate your Conda environment by using your YAML file in your Studio instance\. Open the Studio **Launcher**\. For more information on opening the **Launcher**, see [Amazon SageMaker Studio Launcher](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-launcher.html)\. From the **Launcher**, choose **Open image terminal**\. In the terminal navigate to the directory that contains the YAML file, then run the following commands\. 

```
conda env create --file <ENVIRONMENT_NAME>.yml
conda activate <ENVIRONMENT_NAME>
```

After these commands are complete, you can select your environment as the kernel for your Studio notebook instances\. To view the available environment, run `conda env list`\. To activate your environment, run `conda activate <ENVIRONMENT_NAME>`\.

