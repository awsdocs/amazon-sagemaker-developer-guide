# Use external resources in Amazon SageMaker Studio Lab<a name="studio-lab-use-external"></a>

With Amazon SageMaker Studio Lab, you can integrate external resources, such as Jupyter notebooks and data, from Git repositories and Amazon S3\. You can also add an **Open in Amazon SageMaker Studio Lab** button to your GitHub repo and notebooks\. This button lets you clone your notebooks directly from Studio Lab\.

The following topics show how to integrate external resources\.

**Topics**
+ [Use GitHub resources](#studio-lab-use-external-clone-github)
+ [Add an **Open in Amazon SageMaker Studio Lab** button to your notebook](#studio-lab-use-external-add-button)
+ [Import files from your computer](#studio-lab-use-external-import)
+ [Connect to Amazon S3](#studio-lab-use-external-s3)

## Use GitHub resources<a name="studio-lab-use-external-clone-github"></a>

Amazon SageMaker Studio Lab offers integration with GitHub\. With this integration, you can clone notebooks and repositories directly to your Studio Lab project\. 

The following topics give information about how to use GitHub resources with Amazon SageMaker Studio Lab\.

### Amazon SageMaker Studio Lab sample notebooks<a name="studio-lab-use-external-clone-examples"></a>

To get started with a repository of sample notebooks tailored for Studio Lab, see [Amazon SageMaker Studio Lab Sample Notebooks](https://github.com/aws/studio-lab-examples#sagemaker-studio-lab-sample-notebooks)\.

This repository provides notebooks for the following use cases and others\.
+ Computer vision
+ Connecting to AWS
+ Creating custom environments
+ Geospatial data analysis
+ Natural language processing
+ Using R

### Clone a GitHub repo<a name="studio-lab-use-external-clone-repo"></a>

 To clone a GitHub repo to your Amazon SageMaker Studio Lab project, follow these steps\. 

1.  Open the Studio Lab project runtime\. 

1.  From the menu, select **Git** to open a new dropdown menu\. 

1.  Select **Clone Git Repository** to open a new window\. 

1.  In the new window, paste the repository's URL\. 

1.  Select **Clone**\. 

### Clone individual notebooks from GitHub<a name="studio-lab-use-external-clone-individual"></a>

To open a notebook in Studio Lab, you must have access to the repo that the notebook is in\. The following examples describe Studio Lab permission\-related behavior in various situations\.
+ If a repo is public, you can automatically clone the notebook into your project from the Studio Lab preview page\.
+ If a repo is private, you are prompted to sign in to GitHub from the Studio Lab preview page\. If you have access to a private repo, you can clone the notebook into your project\.
+ If you don't have access to a private repo, you cannot clone the notebook from the Studio Lab preview page\.

The following sections show two options for you to copy a GitHub notebook in your Studio Lab project\. These options depend on whether the notebook has an **Open in Amazon SageMaker Studio Lab** button\. 

#### Option 1: Copy notebook with an **Open in Amazon SageMaker Studio Lab** button<a name="studio-lab-use-external-clone-individual-button"></a>

 The following procedure shows how to copy a notebook that has an **Open in Amazon SageMaker Studio Lab** button\. If you want to add this button to your notebook, see [Add an **Open in Amazon SageMaker Studio Lab** button to your notebook](#studio-lab-use-external-add-button)\.

1.  Sign in to Studio Lab following the steps in [Sign in to Studio Lab](studio-lab-onboard.md#studio-lab-onboard-signin)\.

1.  In a new browser tab, navigate to the GitHub notebook that you want to clone\. 

1.  In the notebook, select the **Open in Amazon SageMaker Studio Lab** button to open a new page in Studio Lab with a preview of the notebook\.

1.  If the project runtime is not already running, start it by choosing the **Start runtime** button at the top of the preview page\. Wait for the runtime to start before proceeding to the next step\.

1.  After the project runtime has started, select **Copy to project** to open the project runtime in a new browser tab\. 

1.  In the **Copy from GitHub?** dialog box, select **Copy notebook only**\. This copies the notebook file to your project\.

#### Option 2: Clone any GitHub notebook<a name="studio-lab-use-external-clone-individual-general"></a>

 The following procedure shows how to copy any notebook from GitHub\. 

1.  Navigate to the notebook in GitHub\. 

1.  In the browser’s address bar, modify the notebook URL, as follows\.

   ```
   # Original URL
   https://github.com/<PATH_TO_NOTEBOOK>
   
   # Modified URL 
   https://studiolab.sagemaker.aws/import/github/<PATH_TO_NOTEBOOK>
   ```

1. Navigate to the modified URL\. This opens a preview of the notebook in Studio Lab\. 

1.  If the project runtime is not already running, start it by choosing the **Start runtime** button at the top of the preview page\. Wait for the runtime to start before proceeding to the next step\. 

1.  After the project runtime has started, select **Copy to project** to open the project runtime in a new browser tab\. 

1.  In the **Copy from GitHub?** dialog box, select **Copy notebook only** to copy the notebook file to your project\.

## Add an **Open in Amazon SageMaker Studio Lab** button to your notebook<a name="studio-lab-use-external-add-button"></a>

When you add the **Open in Amazon SageMaker Studio Lab** button to your notebooks, others can clone your notebooks or repositories directly to their Studio Lab projects\. If you are sharing your notebook within a public GitHub repository, your content will be publicly readable\. Do not share private content, such as AWS access keys or AWS Identity and Access Management credentials, in your notebook\.

 To add the functional **Open in Amazon SageMaker Studio Lab** button to your Jupyter notebook or repository, add the following markdown to the top of your notebook or repository\. 

```
[![Open In SageMaker Studio Lab](https://studiolab.sagemaker.aws/studiolab.svg)](https://studiolab.sagemaker.aws/import/github/<PATH_TO_YOUR_NOTEBOOK_ON_GITHUB>)
```

## Import files from your computer<a name="studio-lab-use-external-import"></a>

 The following steps show how to import files from your computer to your Studio Lab project\.  

1.  Open the Studio Lab project runtime\. 

1.  Open the **File Browser** panel\. 

1.  In the actions bar of the **File Browser** panel, select the **Upload Files** button\. 

1.  Select the files that you want to upload from your local machine\. 

1.  Select **Open**\. 



 Alternatively, you can drag and drop files from your computer into the **File Browser** panel\. 

## Connect to Amazon S3<a name="studio-lab-use-external-s3"></a>

The AWS CLI enables AWS integration in your Studio Lab project\. With this integration, you can pull resources from Amazon S3 to use with your Jupyter notebooks\.

To use AWS CLI with Studio Lab, complete the following steps\. For a notebook that outlines this integration, see [Using Studio Lab with AWS Resources](https://github.com/aws/studio-lab-examples/blob/main/connect-to-aws/Access_AWS_from_Studio_Lab.ipynb)\.

1. Install the AWS CLI following the steps in [Installing or updating the latest version of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)\. 

1. Configure your AWS credentials by following the steps in [Quick setup](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-quickstart.html)\. The role for your AWS account must have permissions to access the Amazon S3 bucket that you are copying data from\. 

1. From your Jupyter notebook, clone resources from the Amazon S3 bucket, as needed\. The following command shows how to clone all resources from an Amazon S3 path to your project\. For more information, see the [AWS CLI Command Reference](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/s3/cp.html)\.

   ```
   !aws s3 cp s3://<BUCKET_NAME>/<PATH_TO_RESOURCES>/ <PROJECT_DESTINATION_PATH>/ --recursive
   ```