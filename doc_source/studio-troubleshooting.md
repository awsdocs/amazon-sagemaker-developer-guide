# Troubleshooting Amazon SageMaker Studio<a name="studio-troubleshooting"></a>

The following are common errors that you might run into when using Amazon SageMaker Studio\. Each error is followed by a solution to the error\. 
+ **SageMaker Studio core functionalities are not available\.**

  If you get this error message when opening Studio, it might be due to Python package version conflicts\. This occurs if you used the following commands in a notebook or terminal to install Python packages that have version conflicts with SageMaker package dependencies\.

  ```
  !pip install
  ```

  ```
  pip install --user
  ```

  To resolve this issue, complete the following steps:

  1. Uninstall recently installed Python packages\. If you’re not sure which package to uninstall, reach out using the feedback button on the lower left of the AWS Management Console\.

  1. Restart Studio:

     1. Shut down Studio from the **File** menu\.

     1. Wait for 1 minute\.

     1. Re\-open Studio by refreshing the page or opening it from the AWS Management Console\.

  The problem should be resolved if you have uninstalled the package which caused the conflict\. To install packages without causing this issue again, use `%pip install` without the `--user` flag\.

  If the issue persists, create a new user profile and set up your environment with that user profile\.

  If these solutions don't fix the issue, reach out using the feedback button on the lower left of the AWS Management Console\.
+ **Unable to open Studio from the AWS Management Console\.**

  If you are continually unable to open Studio, and are unable to make a new running instance with all default settings, reach out using the feedback button on the lower left of the AWS Management Console\.