# Update a Notebook Instance<a name="nbi-update"></a>

After you create a notebook instance, you can update it using the SageMaker console and [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateNotebookInstance.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateNotebookInstance.html) API operation\.

You can update the tags of a notebook instance that is `InService`\. To update any other attribute of a notebook instance, its status must be `Stopped`\.

**To update a notebook instance in the SageMaker console:**

1. Open the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\. 

1. Choose **Notebook instances**\.

1. Choose the notebook instance that you want to update by selecting the notebook instance **Name** from the list\.

1. If your notebook **Status** is not `Stopped`, select the **Stop** button to stop the notebook instance\. 

   When you do this, the notebook instance status changes to `Stopping`\. Wait until the status changes to `Stopped` to complete the following steps\. 

1. Select the **Edit** button to open the **Edit notebook instance** page\. 

1. Update your notebook instance and select the **Update notebook instance** button at the bottom of the page when you are done to return to the notebook instances page\. Your notebook instance status changes to **Updating**\. 

   When the notebook instance update is complete, the status changes to `Stopped`\.