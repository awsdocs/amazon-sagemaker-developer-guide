# Attach a custom SageMaker image \(Control Panel\)<a name="studio-byoi-attach"></a>

To use a custom SageMaker image, you must attach a version of the image to your domain\. When you attach an image version, it appears in the SageMaker Studio Launcher and is available in the **Select image** dropdown list, which users use to launch an activity or change the image used by a notebook\.

There is a limit to the number of image versions that can be attached at any given time\. After you reach the limit, you must detach a version in order to attach another version of the image\.

## Attach an existing image version to your domain<a name="studio-byoi-attach-existing"></a>

This topic describes how you can attach an existing custom SageMaker image version to your domain using the SageMaker Studio control panel\. You can also create a custom SageMaker image and image version, and then attach that version to your domain\.

The steps to create an image and image version are the same whether you use the Studio control panel or the SageMaker console\. For the procedure to create an image and image version, see [Create a custom SageMaker image \(Console\)](studio-byoi-create.md)\.

**To attach an existing image**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. In the left navigation pane, choose **Amazon SageMaker Studio**\.

1. On the **SageMaker Studio Control Panel**, under **Custom images attached to domain**, choose **Attach image**\.

1. For **Image source**, choose **Existing image**\.

1. Choose an existing image from the list\.

1. Choose a version of the image from the list\.

1. Choose **Next**\.

1. Choose the IAM role\. For more information, see [Create a custom SageMaker image \(Console\)](studio-byoi-create.md)\.

1. Choose **Next**\.

1. Under **Studio configuration**, enter or change the following settings\. For information on how to get the kernel information from the image, see [DEVELOPMENT](https://github.com/aws-samples/sagemaker-studio-custom-image-samples/blob/main/DEVELOPMENT.md) in the SageMaker Studio Custom Image Samples repository\.
   + EFS mount path – The path within the image to mount the user's Amazon Elastic File System \(EFS\) home directory\.
   + Kernel:
     + For **Kernel name**, enter the name of an existing kernel in the image\.
     + \(Optional\) For **Kernel display name**, enter the display name for the kernel\.
     + Choose **Add kernel**\.
   + \(Optional\) Configuration tags – Choose **Add new tag** and then add a configuration tag\.
**Note**  
For more information, see the **Kernel discovery** and **User data** sections of [Custom SageMaker image specifications](studio-byoi-specs.md)\.

1. Choose **Submit**

Wait for the image version to be attached to the domain\. When attached, the version is displayed in the **Custom images** list and briefly highlighted\.

## Detach a custom SageMaker image<a name="studio-byoi-detach"></a>

When you detach an image from a domain, all versions of the image are detached\. When an image is detached, all users of the domain lose access to the image versions\.

A running notebook that has a kernel session on an image version when the version is detached, continues to run\. When the notebook is stopped or the kernel is shut down the image version becomes unavailable\.

**To detach an image**

1. In the SageMaker Studio control panel, under **Custom images attached to domain**, choose the image and then choose **Detach**\.

1. \(Optional\) To delete the image and all versions from SageMaker, select **Also delete the selected images \.\.\.**\. This does not delete the associated container images from Amazon ECR\.

1. Choose **Detach**\.
