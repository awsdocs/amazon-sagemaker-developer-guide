# Register a Model Version<a name="model-registry-version"></a>

You register an Amazon SageMaker model by creating a model version that specifies the model group to which it belongs\. A model version must include both the model artifacts \(the trained weights of a model\) and the inference code for the model\.

An *inference pipeline* is a SageMaker model composed of a linear sequence of two to fifteen containers that process inference requests\. You register an inference pipeline by specifying the containers and the associated environment variables\. For more information on inference pipelines, see [Deploy an Inference Pipeline](inference-pipelines.md)\.

You can register a model with an inference pipeline, by specifying the containers and the associated environment variables\. To create a model version with an inference pipeline by using either the AWS SDK for Python \(Boto3\) or by creating a step in a SageMaker model building pipeline\. 

## Register a Model Version \(SageMaker Pipelines\)<a name="model-registry-pipeline"></a>

To register a model version by using a SageMaker model building pipeline, create a `RegisterModel` step in your pipeline\. For information about creating a `RegisterModel` step as part of a pipeline, see [Step 8: Define a RegisterModel Step to Create a Model Package](define-pipeline.md#define-pipeline-register)\.

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
# Specify the model source
model_url = "s3://your-bucket-name/model.tar.gz"

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