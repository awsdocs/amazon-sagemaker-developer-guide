# Launch your Amazon SageMaker Studio Lab project runtime<a name="studio-lab-manage-runtime"></a>

 The Amazon SageMaker Studio Lab project runtime lets you write and run code directly from your browser\. It is based on JupyterLab and has an integrated terminal and console\. For more information about JupyterLab, see the [JupyterLab Documentation](https://jupyterlab.readthedocs.io/en/stable/)\.

 The following topic gives information about how to manage your project runtime\. These topics require that you sign in to your Amazon SageMaker Studio Lab account\. For more information about signing in, see [Sign in to Studio Lab](studio-lab-onboard.md#studio-lab-onboard-signin)\. For more information about your project, see [Amazon SageMaker Studio Lab components overview](studio-lab-overview.md)\. 

**Topics**
+ [Start the project runtime](#studio-lab-manage-runtime-start)
+ [Stop the project runtime](#studio-lab-manage-runtime-stop)
+ [View remaining compute time](#studio-lab-manage-runtime-view)
+ [Change your compute type](#studio-lab-manage-runtime-change)

## Start the project runtime<a name="studio-lab-manage-runtime-start"></a>

To use Studio Lab, you must start your project runtime\. This runtime gives you access to the JupyterLab environment\.

1.  Navigate to the Studio Lab project overview page\. The URL takes the following format\.

   ```
   https://studiolab.sagemaker.aws/users/<YOUR_USER_NAME>
   ```

1. Under **My Project**, select a compute type\. For more information about compute types, see [Compute instance type](studio-lab-overview.md#studio-lab-overview-project-compute)\.

1.  Select **Start runtime**\. 

1.  After the runtime is running, select **Open project**\. This opens the project runtime environment in a new browser tab\. 

## Stop the project runtime<a name="studio-lab-manage-runtime-stop"></a>

When you stop your project runtime, your files are not automatically saved\. To ensure that you don't lose your work, save all of your changes before stopping your project runtime\.
+  Under **My Project**, select **Stop runtime**\. 

## View remaining compute time<a name="studio-lab-manage-runtime-view"></a>

Your project runtime has limited compute time based on the compute type that you select\. For more information about compute time in Studio Lab, see [Compute instance type](studio-lab-overview.md#studio-lab-overview-project-compute)\.
+  Under **My Project**, view **Time remaining**\. 

## Change your compute type<a name="studio-lab-manage-runtime-change"></a>

You can switch your compute type based on your workflow\. For more information about compute types, see [Compute instance type](studio-lab-overview.md#studio-lab-overview-project-compute)\.

1. Save any project files before changing the compute type\. 

1.  Navigate to the Studio Lab project overview page\. The URL takes the following format\.

   ```
   https://studiolab.sagemaker.aws/users/<YOUR_USER_NAME>
   ```

1. Under **My Project**, select the desired compute type \(CPU or GPU\)\. 

1. Confirm your choice by selecting **Restart** in the **Restart project runtime?** dialog box\. Studio Lab stops your current project runtime, then starts a new project runtime with your updated compute type\.

1. After the project runtime has started, select **Open project**\. This opens the project runtime environment in a new browser tab\. For information about using the project runtime environment, see [Use the Amazon SageMaker Studio Lab project runtime](studio-lab-use.md)\.