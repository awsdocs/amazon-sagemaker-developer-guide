# Create and Delete a Worker Task Templates<a name="a2i-worker-template-console"></a>

Use the instructions on this page to create and delete worker task template resources in the Augmented AI area of the Amazon SageMaker console\.

A starter template is provided for Amazon Textract and Amazon Rekognition tasks\. To learn how to customize your template using HTML crowd elements, see [Create Custom Worker Task Template](a2i-custom-templates.md)\. 

When you create a worker task template using the Worker task templates page of the Augmented AI console, a human task UI ARN will be generated\. Use this ARN as the input to `HumanTaskUiArn` when you create a flow definition using the API operation [ `CreateFlowDefinition`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateFlowDefinition.html)\. You can choose this template when creating a human review workflow on the Human review workflows page of the console\. 

If you are creating a worker task template resource for an Amazon Textract or Amazon Rekognition task type, you can preview the worker UI that will be generated from your template on the Worker task templates console page\. You will need to attach the policy described in [Enable Worker Task Template Previews ](a2i-permissions-security.md#permissions-for-worker-task-templates-augmented-ai) to the IAM role that you use to preview the template\.

## Create a Worker Task Template<a name="a2i-create-worker-template-console"></a>

You can create a worker task template using the SageMaker console and using the SageMaker API operation [ `CreateHumanTaskUi`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateHumanTaskUi.html)\. 

**Create a worker task template \(console\)**

1. Open the Amazon A2I console at [https://console.aws.amazon.com/a2i](https://console.aws.amazon.com/a2i)\.

1. Under Amazon Augmented AI in the left navigation pane, choose **Worker task templates**\.

1. Choose **Create template**\.

1. In **Template name**, enter a unique name\.

1. \(Optional\) Enter an **IAM role** that grants A2I the permissions necessary to call services on your behalf\. 

1. In **Template type**, choose a template type from the drop\-down menu\. If you are creating a template for a **Textract\-form extraction** or **Rekognition\-image moderation** task, choose the appropriate option\. 

1. Enter your custom template elements as follows:
   + If you selected the Amazon Textract or Amazon Rekognition task template, the **Template editor** autopopulates with a default template that you can customize\. 
   + If you are using a custom template, enter your predefined template in the editor\. 

1. \(Optional\) To complete this step, you must provided an IAM role ARN with permission to read Amazon S3 objects that get rendered on your user interface in **Step 5**\. 

   You can only preview your template if you are creating templates for Amazon Textract or Amazon Rekognition\. 

   Choose **See preview** to preview the interface and instructions that workers will see\. This is an interactive preview\. After you complete the sample task and choose **Submit**, you see the resulting output from the task that you just performed\. 

   If you are creating a worker task template for a custom task type, you can preview your worker task UI using `RenderUiTemplate`\. For more information, see [Preview a Worker Task Template](a2i-custom-templates.md#a2i-preview-your-custom-template)\.

1. When you're satisfied with your template, choose **Create**\.

After you've created your template, you can select that template when you create a human review workflow in the console\. Your template also appears in the Amazon Augmented AI section of the SageMaker console under **Worker task templates**\. Choose your template to view its ARN\. Use this ARN when using the API operation [ `CreateFlowDefinition`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateFlowDefinition.html)\. 

**Create a worker task template using a worker task template \(API\)**  
To generate a worker task template using the SageMaker API operation [ `CreateHumanTaskUi`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateHumanTaskUi.html), specify a name for your UI in `HumanTaskUiName` and input your HTML template in `Content` under `UiTemplate`\. Find documentation on language\-specific SDKs that support this API operation in the **See Also** section of the [ `CreateHumanTaskUi`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateHumanTaskUi.html)\.

## Delete a Worker Task Template<a name="sms-delete-worker-task-template"></a>

Once you have created a worker task template, it can be deleted using the SageMaker console or using the SageMaker API operation [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DeleteHumanTaskUi.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DeleteHumanTaskUi.html)\.

When you delete a worker task template, you will not be able to use human review workflows \(flow definitions\) that were created using that template to start human loops\. Any human loops that have already been created using the worker task template that you delete will continue to be processed until completion and will not be impacted\. 

**Delete a worker task template \(console\)**

1. Open the Amazon A2I console at [https://console.aws.amazon.com/a2i](https://console.aws.amazon.com/a2i)\.

1. Under Amazon Augmented AI in the left navigation pane, choose **Worker task templates**\.

1. Select the template that you want to delete\. 

1. Selete **Delete**\.

1. A modal will appear to confirm your choice\. Select **Delete**\.

**Delete a worker task template \(API\)**  
To delete a worker task template using the SageMaker API operation [ `DeleteHumanTaskUi`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DeleteHumanTaskUi.html), specify a name of your UI in `HumanTaskUiName`\. 