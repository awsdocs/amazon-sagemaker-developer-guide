# Update the Approval Status of a Model<a name="model-registry-approve"></a>

After you create a model version, you typically want to evaluate its performance before you deploy it to a production endpoint\. If it performs to your requirements, you can update the approval status of the model version to `Approved`\. If it does not perform to your requirements, you update the approval status to `Rejected`\.

Setting the approval status of a model version to `Approved` can trigger CI/CD deployment for the model\. Similarly, you can block promotion of a model by setting its approval status to `Rejected`\. You can manually set the status approval of a model version after you register it, or you can create a conditional step to evaluate the model when you create a SageMaker pipeline\. For information about creating a conditional step in a SageMaker pipeline, see [Create and Manage SageMaker Pipelines](pipelines-build.md)\.

**Note**  
SageMaker does not support changing the approval status from `Approved` to `Rejected`\.

You can update the approval status of a model version by using the AWS SDK for Python \(Boto3\) or by using SageMaker Studio\. You can also update the approval status of a model version as part of a conditional step in a SageMaker pipeline\. For information about using a model approval step in a SageMaker pipeline, see [SageMaker Pipelines Overview](pipelines-sdk.md)\.

## Update the Approval Status of a Model \(Boto3\)<a name="model-registry-approve-api"></a>

When you created the model version in [Register a Model Version](model-registry-version.md), you set the `ModelApprovalStatus` to `PendingManualApproval`\. You must update the approval status for the model to `Approved` by calling `update_model_package`\. Note that you can also automate this process by writing code that, for example, sets the approval status of a model depending on the result of an evaluation of some measure of the model's performance\. You can also create a step in a pipeline that automatically deploys a new model version when it is approved\. The following code snippet shows how to manually change the approval status to `Approved`\.

```
model_package_update_input_dict = {
    "ModelPackageArn" : model_package_arn,
    "ModelApprovalStatus" : "Approved"
}
model_package_update_response = sm_client.update_model_package(**model_package_update_input_dict)
```

## Update the Approval Status of a Model \(SageMaker Studio\)<a name="model-registry-approve-studio"></a>

To update the approval status of a model version in SageMaker Studio, complete the following steps\.

1. Sign in to Studio\. For more information, see [Onboard to Amazon SageMaker Studio](gs-studio-onboard.md)\.

1. In the left navigation pane, choose the **Components and registries** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Components_registries.png) \)\.

1. Choose **Model registry**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/model_registry/model-registry.png)

1. From the model groups list, choose the model group you want to view\.

1. A new tab appears with a list of the model versions in the model group\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/model_registry/model-versions.png)

1. In the list of model versions, double\-click the model version for which you want to update the approval status\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/model_registry/model-versions.png)

1. On the model version tab that opens, choose **Update status**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/model_registry/update-model-status.png)

1. In the **Update model version status** dialog box, for **Status** choose either **Approved** or **Rejected**, and then choose **Update status**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/model_registry/approve-model.png)