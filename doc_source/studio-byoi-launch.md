# Launch a custom SageMaker image in Amazon SageMaker Studio<a name="studio-byoi-launch"></a>

After you create your custom SageMaker image and attach it to your domain or shared space, the custom image and kernel appear in selectors in the **Change environment** dialog box of the Studio Launcher\.

**To launch and select your custom image and kernel**

1. In Amazon SageMaker Studio, open the Launcher\. To open the Launcher, choose **Amazon SageMaker Studio** at the top left of the Studio interface or use the keyboard shortcut `Ctrl + Shift + L`\.

   To learn about all the available ways to open the Launcher, see [Use the Amazon SageMaker Studio Launcher](studio-launcher.md)  
![\[SageMaker Studio launcher.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/studio-new-launcher.png)

1. In the Launcher, in the **Notebooks and compute resources** section, choose **Change environment**\.

1. In the **Change environment** dialog, use the dropdown menus to select your **Image** from the **Custom Image** section, and your **Kernel**, then choose **Select**\.

1. In the Launcher, choose **Create notebook** or **Open image terminal**\. Your notebook or terminal launches in the selected custom image and kernel\.

To change your image or kernel in an open notebook, see [Change an Image or a Kernel](notebooks-run-and-manage-change-image.md)\.

**Note**  
If you encounter an error when launching the image, check your Amazon CloudWatch logs\. The name of the log group is `/aws/sagemaker/studio`\. The name of the log stream is `$domainID/$userProfileName/KernelGateway/$appName`\.