# View Model Groups and Versions<a name="model-registry-view"></a>

Model groups and versions help you organize your models\. You can view a list of the model versions in a model group\.

## View a List of Model Versions in a Group<a name="model-registry-view-list"></a>

You can view all of the model versions that are associated with a model group\. If a model group represents all models that you train to address a specific ML problem, you can view all of those related models\.

### View a List of Model Versions in a Group \(Boto3\)<a name="model-registry-view-list-api"></a>

To view model versions associated with a model group by using Boto3, call the `list_model_packages` method, and pass the name of the model group as the value of the `ModelPackageGroupName` parameter\. The following code lists the model versions associated with the model group you created in [Create a Model Package Group \(Boto3\)](model-registry-model-group.md#model-registry-package-group-api)\.

```
sm_client.list_model_packages(ModelPackageGroupName=model_package_group_name)
```

### View a List of Model Versions in a Group \(Amazon SageMaker Studio\)<a name="model-registry-view-list-studio"></a>

To view a list of the model versions in a model group, complete the following steps\.

1. Sign in to Amazon SageMaker Studio\. For more information, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.

1. In the left navigation pane, choose the **Components and registries** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/icons/Components_registries.png) \)\.

1. Choose **Model registry**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/model_registry/model-registry.png)

1. From the model groups list, choose the model group you want to view\.

1. A new tab appears with a list of the model versions in the model group, as shown in the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/model_registry/model-versions.png)