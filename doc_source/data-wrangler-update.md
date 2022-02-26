# Update Data Wrangler<a name="data-wrangler-update"></a>

To update Data Wrangler to the latest release, first shut down the corresponding KernelGateway app from the Amazon SageMaker Studio control panel\. After the KernelGateway app is shut down, restart it by opening a new or existing Data Wrangler flow in Studio\. When you open a new or existing Data Wrangler flow, the kernel that starts contains the latest version of Data Wrangler\.

**Update your Studio and Data Wrangler instance**

1. Navigate to your [SageMaker Console](https://console.aws.amazon.com/sagemaker)\.

1. Choose SageMaker and then Studio\.

1. Choose your user name\.

1. Under **Apps**, in the row displaying the **App name**, choose **Delete app** for the app that starts with `sagemaker-data-wrang`, and for the JupyterServer app\.

1. Choose **Yes, delete app**\.

1. Type `delete` in the confirmation box\.

1. Choose **Delete**\.

1. Reopen your Studio instance\. When you begin to create a Data Wrangler flow, your instance now uses the latest version of Data Wrangler\.

Alternatively, if you are using a Data Wrangler application version that is not the latest version, and you have an existing Data Wrangler flow open, you are prompted to update your Data Wrangler application version in the Studio UI\. The following screenshot shows this prompt\. 

**Important**  
This updates the Data Wrangler kernel gateway app only\. You still need to shut down the JupyterServer app in your user account\. To do this, follow the preceding steps\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/data-wrangler-1click-restart.png)

You can also choose **Remind me later**, in which case an **Update** button appears in the top\-right corner of the screen\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/data-wrangler-1click-restart-update.png)