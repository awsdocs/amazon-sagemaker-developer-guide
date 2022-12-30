# Create a Model Group<a name="model-registry-model-group"></a>

A model group contains a group of versioned models\. Create a model group by using either the AWS SDK for Python \(Boto3\) or Amazon SageMaker Studio\.

## Create a Model Package Group \(Boto3\)<a name="model-registry-package-group-api"></a>

To create a model group by using Boto3, call the `create_model_package_group` method and specify a name and description as parameters\. The following example shows how to create a model group\. The response from the `create_model_package_group` call is the Amazon Resource Name \(ARN\) of the new model package group\.

First, import the required packages and set up the SageMaker Boto3 client\.

```
import time
import os
from sagemaker import get_execution_role, session
import boto3

region = boto3.Session().region_name

role = get_execution_role()

sm_client = boto3.client('sagemaker', region_name=region)
```

Now create the model group\.

```
import time
model_package_group_name = "scikit-iris-detector-" + str(round(time.time()))
model_package_group_input_dict = {
 "ModelPackageGroupName" : model_package_group_name,
 "ModelPackageGroupDescription" : "Sample model package group"
}

create_model_package_group_response = sm_client.create_model_package_group(**model_package_group_input_dict)
print('ModelPackageGroup Arn : {}'.format(create_model_package_group_response['ModelPackageGroupArn']))
```

## Create a Model Package Group \(Amazon SageMaker Studio\)<a name="model-registry-package-group-studio"></a>

To create a model group in Amazon SageMaker Studio, complete the following steps\.

1. Sign in to Studio\. For more information, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.

1. In the left navigation pane, choose the **Home** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/icons/house.png) \)\.

1. Choose **Models**, and then **Model registry**\.

1. Choose **Create model group**\.

1. In the **Create new model group** dialog box, enter the following information:
   + Enter the name of the new model group in the **Name** field\.
   + \(Optional\) Enter a description for the model group in the **Description** field\.
   + \(Optional\) Enter any key\-value pairs you want to associate with the model group in the **Tags** field\. For information about using tags, see [Tagging AWS resources](https://docs.aws.amazon.com/general/latest/gr/aws_tagging.html) in the *AWS General Reference*\.
   + \(Optional\) Choose a project with which to associate the model group in the **Project** field\. For information about projects, see [Automate MLOps with SageMaker Projects](sagemaker-projects.md)\.

1. Choose **Create model group**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/model_registry/model-group-details.png)