# Export Amazon SageMaker Studio Lab environment to Amazon SageMaker Studio<a name="studio-lab-use-migrate"></a>

Amazon SageMaker Studio offers many features for machine learning and deep learning workflows that are unavailable in Amazon SageMaker Studio Lab\. To take advantage of the features offered in Amazon SageMaker Studio, you must first onboard following the steps in [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\. 

 After you’ve onboarded to Studio, you can migrate your Studio Lab environment and artifacts to Studio\. 

 Studio Lab does not currently support sharing your project with others\. However, you may download a copy of your project files to share using the following steps\.

**Topics**
+ [Step 1 – Export your Studio Lab Conda environment](#studio-lab-use-migrate-step1)
+ [Step 2 – Export your Studio Lab artifacts to GitHub](#studio-lab-use-migrate-step2)
+ [Step 3 – Import your Studio Lab artifacts from GitHub to Studio](#studio-lab-use-migrate-step3)
+ [Step 4 – Import your Studio Lab Conda environments in Studio](#studio-lab-use-migrate-step4)

## Step 1 – Export your Studio Lab Conda environment<a name="studio-lab-use-migrate-step1"></a>

When you add libraries to your environment following the steps in [Manage your environment](studio-lab-use-manage.md), they become part of your Conda environments\. The following procedure shows how to export the definitions of those environments so they can be rebuilt in Studio\. 

1. From the terminal, list the Conda environments in your Studio Lab\.

   ```
   conda env list
   ```

   This command outputs a list of the Conda environments and their locations in the file system\. When you onboard to Studio Lab, you use the `default-kernel` Conda environment by default\.

   ```
   # conda environments: #
         default-kernel           /home/studio-lab-user/.conda/envs/default-kernel 
         studiolab                /home/studio-lab-user/.conda/envs/studiolab base                     
         /opt/conda
   ```

   We recommend that you do not export the  `studiolab` and `base` environments\. These environments are not usable in Studio for the following reasons: 
   +  `studiolab`: This sets up the JupyterLab environment for Studio Lab\. Studio Lab runs a different major version of JupyterLab than Studio, so it is not usable in Studio\. 
   +  `base`: This environment comes with Conda by default\. The `base` environment in Studio Lab and the `base` environment in Studio have incompatible versions of many packages\. 

1. For each Conda environment that you want to migrate to Studio, run the following command\. This command exports the Conda environment definition to a YAML file in your Studio Lab home directory\. 

   ```
   conda env export -n <ENVIRONMENT_NAME> > ~/<ENVIRONMENT_NAME>.yml
   ```

## Step 2 – Export your Studio Lab artifacts to GitHub<a name="studio-lab-use-migrate-step2"></a>

 Next, you must clone your artifacts to a GitHub repository\. The repository will be cloned in Studio in Step 3\. 

 The following procedure shows how to synchronize your content with GitHub using the Studio Lab terminal\.  

1.  From the Studio Lab terminal, navigate to your home directory\. 

1.  Initialize the directory as a Git repository using the following command\. For more information, see the [git\-init documentation](https://git-scm.com/docs/git-init)\. 

   ```
   git init
   ```

1.  Move all of your artifacts, including the Conda environments’ YAML file definitions, to your home directory\. 

1.  Add all relevant files and then commit your changes\. 

   ```
   git add <FILE_NAME>
   git commit -m "<COMMIT_MESSAGE>"
   ```

1.  Push the commit to your remote repository\. This repository has the format `https://github.com/<GITHUB_USERNAME>/<REPOSITORY_NAME>` where `<GITHUB_USERNAME>` is your GitHub user name and the `<REPOSITORY_NAME>` is your remote repository\. 

   ```
   git remote add origin git@github.com/<GITHUB_USERNAME>/<REPOSITORY_NAME>
   git push -u origin <BRANCH_NAME>
   ```

## Step 3 – Import your Studio Lab artifacts from GitHub to Studio<a name="studio-lab-use-migrate-step3"></a>

The following procedure shows how to import your artifacts to Studio from your GitHub repository\. 

1.  Navigate to Studio\. 

1.  From the Launcher, navigate to **Notebooks and compute resources**\. 

1.  For **Select a SageMaker Image**, select **Data Science**\. This image comes with Conda preinstalled\. 

1.  Select **Image Terminal**\. 

1. From the image terminal, run the following command to clone your repository\. This command creates a directory named after `<REPOSITORY_NAME>` in your Studio instance\. After that, it clones your artifacts in that repository\. 

   ```
   git clone https://github.com/<GITHUB_USERNAME>/<REPOSITORY_NAME>.git
   ```

## Step 4 – Import your Studio Lab Conda environments in Studio<a name="studio-lab-use-migrate-step4"></a>

After you’ve cloned your GitHub repository to your Studio instance, you can use the YAML files to recreate your Conda environments in Studio\.  

 For each Conda environment that you want to recreate, run the following commands\. 

```
conda env create --file /<PATH_TO_DIRECTORY>/<ENVIRONMENT_NAME>.yml
conda activate <ENVIRONMENT_NAME>
conda install ipykernel
python -m ipykernel install
```

 After these commands are complete, you can select your environment as the kernel for your Studio notebook instances\. 

