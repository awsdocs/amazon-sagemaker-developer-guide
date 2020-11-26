# Notebook Instance Software Updates<a name="nbi-software-updates"></a>

Amazon SageMaker periodically tests and releases software that is installed on notebook instances\. This includes:
+ Kernel updates
+ Security patches
+ AWS SDK updates
+ [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) updates
+ Open source software updates

SageMaker does not automatically update software on a notebook instance when it is in service\. To ensure that you have the most recent software updates, stop and restart your notebook instance, either in the SageMaker console or by calling [  `StopNotebookInstance`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_StopNotebookInstance.html)\.

You can also manually update software installed on your notebook instance while it is running by using update commands in a terminal or in a notebook\.

**Note**  
Updating kernels and some packages might depend on whether root access is enabled for the notebook instance\. For more information, see [Control Root Access to a Notebook Instance](nbi-root-access.md)\.

Notebook instances do not notify you if you are running outdated software\. You can check the [Personal Health Dashboard](https://aws.amazon.com/premiumsupport/technology/personal-health-dashboard/) or the security bulletin at [https://aws\.amazon\.com/security/security\-bulletins/ ](https://aws.amazon.com/security/security-bulletins/) for updates\.