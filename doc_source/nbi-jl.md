# JupyterLab versioning<a name="nbi-jl"></a>

 The Amazon SageMaker notebook instance interface is based on JupyterLab, which is a web\-based interactive development environment for notebooks, code, and data\. Notebooks now support using either JupyterLab 1 or JupyterLab 3\. A single notebook instance can run a single instance of JupyterLab \(at most\)\. You can have multiple notebook instances with different JupyterLab versions\. 

 You can configure your notebook to run your preferred JupyterLab version by selecting the appropriate platform identifier\. Use either the AWS CLI or the SageMaker console when creating your notebook instance\. For more information about platform identifiers, see [Amazon Linux 2 vs Amazon Linux notebook instances](https://docs.aws.amazon.com/sagemaker/latest/dg/nbi-al2.html)\. If you don’t explicitly configure a platform identifier, your notebook instance defaults to running JupyterLab 1\. 

**Topics**
+ [JupyterLab 3](#nbi-jl-3)
+ [Creating a notebook with your JupyterLab version](#nbi-jl-create)
+ [View the JupyterLab version of a notebook from the console](#nbi-jl-view)

## JupyterLab 3<a name="nbi-jl-3"></a>

 JupyterLab 3 support is available only on the Amazon Linux 2 operating system platform\. JupyterLab 3 includes the following features that are not available in JupyterLab 1\. For more information about these features, see [JupyterLab 3\.0 is released\!](https://blog.jupyter.org/jupyterlab-3-0-is-out-4f58385e25bb)\. 
+  Visual debugger when using the following kernels: 
  +  conda\_pytorch\_p38 
  +  conda\_tensorflow2\_p38 
  +  conda\_amazonei\_pytorch\_latest\_p37 
+ File browser filter
+ Table of Contents \(TOC\)
+ Multi\-language support
+ Simple mode
+ Single interface mode
+ Live editing SVG files with updated rendering
+ User interface for notebook cell tags

### Important changes to JupyterLab 3<a name="nbi-jl-3-changes"></a>

 For information about important changes when using JupyterLab 3, see the following JupyterLab change logs: 
+  [v2\.0\.0](https://github.com/jupyterlab/jupyterlab/releases) 
+  [v3\.0\.0](https://jupyterlab.readthedocs.io/en/stable/getting_started/changelog.html#for-developers) 

 **Package version changes** 

 JupyterLab 3 has the following package version changes from JupyterLab 1: 
+  JupyterLab has been upgraded from 1\.x to 3\.x\.
+  Jupyter notebook has been upgraded from 5\.x to 6\.x\.
+  jupyterlab\-git has been updated to version 0\.37\.1\.
+  nbserverproxy 0\.x \(0\.3\.2\) has been replaced with jupyter\-server\-proxy 3\.x \(3\.2\.1\)\.

## Creating a notebook with your JupyterLab version<a name="nbi-jl-create"></a>

 You can select the JupyterLab version when creating your notebook instance from the console following the steps in [Create a Notebook Instance](howitworks-create-ws.md)\. 

 You can also select the JupyterLab version by passing the `platform-identifier` parameter when creating your notebook instance using the AWS CLI as follows: 

```
create-notebook-instance --notebook-instance-name <NEW_NOTEBOOK_NAME> \
--instance-type <INSTANCE_TYPE> \
--role-arn <YOUR_ROLE_ARN> \
--platform-identifier <PLATFORM_TO_USE>
```

## View the JupyterLab version of a notebook from the console<a name="nbi-jl-view"></a>

 You can view the JupyterLab version of a notebook using the following procedure: 

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1.  Navigate to the **Notebook instances** page\. 

1.  From the list of notebook instances, select your notebook instance name\. 

1.  On the **Notebook instance settings** page, view the **Platform Identifier** to see the JupyterLab version of the notebook\. 