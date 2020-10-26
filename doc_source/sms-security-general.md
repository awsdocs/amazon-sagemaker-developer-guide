# Ground Truth Security and Permissions<a name="sms-security-general"></a>

Use the topics on this page to learn about Ground Truth security features, and how to configure IAM permissions to create a labeling job\. 

If you are new to Ground Truth or you do not require granular permissions to use the service, you can use the managed policy, AmazonSageMakerFullAccess to grant permission to an IAM role to create a labeling job\. You can also attach AmazonSageMakerFullAccess to an IAM role to create an execution role\. To learn more about this managed policy, see [AmazonSageMakerFullAccess Policy](sagemaker-roles.md#sagemaker-roles-amazonsagemakerfullaccess-policy)\.

**Important**  
When you create a custom labeling workflow, the AmazonSageMakerFullAccess policy is restricted to invoking AWS Lambda functions with one of the following four strings as part of the function name: `SageMaker`, `Sagemaker`, `sagemaker`, or `LabelingFunction`\. This applies to both your pre\-annotation and post\-annotation Lambda functions\. If you choose to use names without those strings, you must explicitly provide `lambda:InvokeFunction` permission to the IAM role used to create the labeling job\.

**Topics**
+ [Assign IAM Permissions to Use Ground Truth](sms-security-permission.md)
+ [Data and Storage Volume Encryption](sms-security.md)
+ [Workforce Authentication and Restrictions](sms-security-workforce-authentication.md)