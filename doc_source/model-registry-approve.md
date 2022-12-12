# Update the Approval Status of a Model<a name="model-registry-approve"></a>

After you create a model version, you typically want to evaluate its performance before you deploy it to a production endpoint\. If it performs to your requirements, you can update the approval status of the model version to `Approved`\. Setting the status to `Approved` can initiate CI/CD deployment for the model\. If the model version does not perform to your requirements, you can update the approval status to `Rejected`\.

You can manually update the approval status of a model version after you register it, or you can create a condition step to evaluate the model when you create a SageMaker pipeline\. For information about creating a condition step in a SageMaker pipeline, see [Pipeline Steps](build-and-manage-steps.md)\.

When you use one of the SageMaker provided project templates and the approval status of a model version changes, the following action occurs\. Only valid transitions are shown\.
+ `PendingManualApproval` to `Approved` – initiates CI/CD deployment for the approved model version
+ `PendingManualApproval` to `Rejected` – No action
+ `Rejected` to `Approved` – initiates CI/CD deployment for the approved model version
+ `Approved` to `Rejected` – initiates CI/CD to deploy the latest model version with an `Approved` status

You can update the approval status of a model version by using the AWS SDK for Python \(Boto3\) or by using Amazon SageMaker Studio\. You can also update the approval status of a model version as part of a condition step in a SageMaker pipeline\. For information about using a model approval step in a SageMaker pipeline, see [SageMaker Pipelines Overview](pipelines-sdk.md)\.

## Update the Approval Status of a Model \(Boto3\)<a name="model-registry-approve-api"></a>

When you created the model version in [Register a Model Version](model-registry-version.md), you set the `ModelApprovalStatus` to `PendingManualApproval`\. You update the approval status for the model by calling `update_model_package`\. Note that you can automate this process by writing code that, for example, sets the approval status of a model depending on the result of an evaluation of some measure of the model's performance\. You can also create a step in a pipeline that automatically deploys a new model version when it is approved\. The following code snippet shows how to manually change the approval status to `Approved`\.

```
model_package_update_input_dict = {
    "ModelPackageArn" : model_package_arn,
    "ModelApprovalStatus" : "Approved"
}
model_package_update_response = sm_client.update_model_package(**model_package_update_input_dict)
```

## Update the Approval Status of a Model \(Amazon SageMaker Studio\)<a name="model-registry-approve-studio"></a>

The following procedure shows how to manually change the approval status from `Approved` to `Rejected`\.

1. Sign in to Studio\. For more information, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.

1. In the left navigation pane, choose the **Home** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/icons/house.png) \)\.

1. Choose **Models**, and then **Model registry**\.

1. From the model groups list, choose the model group you want to view\. A new tab opens with a list of the model versions in the group\.

1. In the list of model versions, right\-click the model version you want to update and choose **Update model version status**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/model_registry/update-model-status-2.png)

1. In the **Update model version status** dialog box, for **Status** choose **Rejected**, and then choose **Update status**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/model_registry/approve-model.png)