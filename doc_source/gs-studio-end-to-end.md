# Amazon SageMaker Studio Tour<a name="gs-studio-end-to-end"></a>

For a walkthrough that takes you on a tour of the main features of Amazon SageMaker Studio, see the [xgboost\_customer\_churn\_studio\.ipynb](https://sagemaker-examples.readthedocs.io/en/latest/aws_sagemaker_studio/getting_started/xgboost_customer_churn_studio.html) sample notebook from the [aws/amazon\-sagemaker\-examples](https://github.com/aws/amazon-sagemaker-examples) repository\. The code in the notebook trains multiple models and sets up the SageMaker Debugger and SageMaker Model Monitor\. The walkthrough shows you how to view the trials, compare the resulting models, show the debugger results, and deploy the best model using the SageMaker Studio UI\. You don't need to understand the code to follow this walkthrough\.

For a series of videos that shows how to use the main features of SageMaker Studio, see [NEW\! Amazon SageMaker Studio](https://www.youtube.com/playlist?list=PLJgojBtbsuc0MjdtpJPo4g4PL8mMsd2nK) on YouTube\.

**Prerequisites**

To run the notebook for this tour, you need:
+ An AWS SSO or IAM account to sign in to Studio\. For information, see [Onboard to Amazon SageMaker Studio](gs-studio-onboard.md)\.
+ Basic familiarity with the Studio user interface and Jupyter notebooks\. For information, see [Amazon SageMaker Studio UI Overview](studio-ui.md)\.
+ A copy of the [aws/amazon\-sagemaker\-examples](https://github.com/aws/amazon-sagemaker-examples) repository in your Studio environment\.

**To clone the repository**

1. Sign in to SageMaker Studio\. For AWS SSO users, sign in using the URL from your invitation email\. For IAM users, follow these steps\.

   1. Sign in to the [SageMaker console](https://console.aws.amazon.com/sagemaker/)\.

   1. Choose **Amazon SageMaker Studio** in the left navigation pane\.

   1. Choose **Open Studio** in the row next to your user name\.

1. On the top menu, choose **File** then **New** then **Terminal**\.

1. At the command prompt, run the following command to clone the [aws/amazon\-sagemaker\-examples](https://github.com/aws/amazon-sagemaker-examples) repository\.

   `git clone https://github.com/aws/amazon-sagemaker-examples.git`

**Note**  
If you encounter an error when you run the sample notebook, and some time has passed from when you cloned the repository, review the notebook on the remote repository for updates\.