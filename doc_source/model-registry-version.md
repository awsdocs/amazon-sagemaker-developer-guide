# Register a Model Version<a name="model-registry-version"></a>

You register a model by creating a model version that specifies the model group to which it belongs\. A model version must include both the model artifacts \(the trained weights of a model\), and the inference code for the model\. Create a model version by using either the AWS SDK for Python \(Boto3\) or by creating a step in a SageMaker mode building pipeline\. 

## Register a Model Version \(SageMaker Pipelines\)<a name="model-registry-pipeline"></a>

To register a model version by using a SageMaker model building pipeline, create a `RegisterModel` step in your pipeline\. For information about creating `RegisterModel` step as part of a pipeline, see [Define a Pipeline](define-pipeline.md)\.

## Register a Model Version \(Boto3\)<a name="model-registry-version-api"></a>

To register a model version by using Boto3, call the `create_model_package` method\.

First, you set up the parameter dictionary to pass to the `create_model_package` method\.

```
modelpackage_inference_specification =  { 
    "InferenceSpecification": {    
      "Containers": [ 
         {  
            "Image": '257758044811.dkr.ecr.us-east-2.amazonaws.com/sagemaker-xgboost:1.2-1',
         }
      ],
      "SupportedContentTypes": [ "text/csv" ],
      "SupportedResponseMIMETypes": [ "text/csv" ],
   }
 }

# Specify the model data
modelpackage_inference_specification["InferenceSpecification"]["Containers"][0]["ModelDataUrl"]=model_url

create_model_package_input_dict = {
    "ModelPackageGroupName" : model_package_group_name,
    "ModelPackageDescription" : "Model to detect 3 different types of irises (Setosa, Versicolour, and Virginica)",
    "ModelApprovalStatus" : "PendingManualApproval"
}
create_model_package_input_dict.update(modelpackage_inference_specification)
```

Then you call the `create_model_package` method, passing in the parameter dictionary that you just set up\.

```
create_mode_package_response = sm_client.create_model_package(**create_model_package_input_dict)
model_package_arn = create_mode_package_response["ModelPackageArn"]
print('ModelPackage Version ARN : {}'.format(model_package_arn))
```