# Environment<a name="domain-space-environment"></a>

 This page gives information about modifications to the Amazon SageMaker Domain environment\. This includes custom images, lifecycle configurations, and git repositories attached to a Domain environment\. These can also be attached to a shared space using the AWS CLI by passing values to the [create\-space](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/sagemaker/create-space.html) command using the `space-settings` parameter\.

 For more information about bringing a custom Amazon SageMaker Studio image, see [Bring your own SageMaker image](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-byoi.html)\. 

 For more information about bringing a custom RStudio image, see [Bring your own image to RStudio on SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/rstudio-byoi.html)\. 

 For instructions on using a lifecycle configuration with Studio, see [Use Lifecycle Configurations with Amazon SageMaker Studio](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-lcc.html)\. 

For information about attaching a git repository to a Domain, see [Attach Suggested Git Repos to SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-git-attach.html)\. 

Complete the following procedure to view the custom images, lifecycle configurations, and git repositories attached to a Domain environment\.

 **Open the Environment page** 

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. On the left navigation pane, choose **Domains**\. 

1.  From the list of Domains, select a Domain to open the **Environment** page\. 

1. On the **Domain details** page, choose the **Environment** tab\. 