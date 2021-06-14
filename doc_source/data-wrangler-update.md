# Update Data Wrangler<a name="data-wrangler-update"></a>

To update Data Wrangler to the latest release, you must first shut down the corresponding KernelGateway app from the SageMaker Studio Control Panel\. After the KernelGateway app is shut down, you must restart it by opening a new or existing Data Wrangler flow in SageMaker Studio\. When you open a new or existing Data Wrangler flow, the kernel that starts contains the latest version of Data Wrangler\.

**Update your Studio and Data Wrangler instance**

1. Navigate to your [SageMaker Console](https://console.aws.amazon.com/sagemaker)\.

1. Choose SageMaker and then SageMaker Studio\.

1. Choose your user name\.

1. Under **Apps**, in the row displaying the **App name**, choose **Delete app** for the app that starts with sagemaker\-data\-wrang, and the JupyterServer app\.

1. Choose **Yes, delete app**\.

1. Type delete in the confirmation box\.

1. Choose **Delete**\.

1. Reopen your Studio instance\. When you begin to create a Data Wrangler flow, your instance will now use the latest version of Data Wrangler\.

Alternatively, if you are using a Data Wrangler application version that is not the latest version, and you have an existing Data Wrangler flow open, you are prompted to update your Data Wrangler application version in the Studio UI\. Below is a screenshot showing this prompt\. 

**Important**  
Note that this will update the Data Wrangler Kernel gateway app only\. You still need to shut down the JupyterServer app in your user account\. To do this, follow the steps above\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/data-wrangler-1click-restart.png)

You can also select Remind me later in which case an Update button will appear in the top right corner of the screen\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/data-wrangler-1click-restart-update.png)