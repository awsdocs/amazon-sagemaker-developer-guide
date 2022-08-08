# Launch a custom SageMaker image in Amazon SageMaker Studio<a name="studio-byoi-launch"></a>

After you create your custom SageMaker image and attach it to your domain, the image appears in the image selector dialog box of the Studio Launcher, and the kernel appears in the kernel selector dialog box\.

**To launch and select your custom image**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. Use the keyboard shortcut `Ctrl + Shift + L` to open Studio Launcher\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-byoi-launcher-default.png)

1. Open the **Select a SageMaker image** dropdown menu\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-byoi-launcher-selector-r.png)

1. Choose your custom image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-byoi-launcher-r.png)

1. Launch a notebook or interactive shell in the custom image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-byoi-notebook-r.png)

1. In an open notebook, you can switch to the custom kernel by choosing a different kernel in the **Select Kernel** dialog box\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-byoi-kernel-selector-r.png)
**Note**  
If you encounter an error when launching the image, check your Amazon CloudWatch logs\. The name of the log group is `/aws/sagemaker/studio`\. The name of the log stream is `$domainID/$userProfileName/KernelGateway/$appName`\.