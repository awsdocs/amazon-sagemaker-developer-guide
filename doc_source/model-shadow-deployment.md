# Shadow variants<a name="model-shadow-deployment"></a>

 You can use SageMaker Model Shadow Deployments to create long running shadow variants to validate any new candidate component of your model serving stack before promoting it to production\. The following diagram shows how shadow variants work in more detail\. 

![\[Details of a shadow variant.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/juxtaposer/shadow-variant.png)

## Deploy shadow variants<a name="model-shadow-deployment-deploy"></a>

 The following code example shows how you can programmatically deploy shadow variants\. Replace the *user placeholder text* in the example with your own information\. 

1.  Create two SageMaker models: one for your production variant, and one for your shadow variant\. 

   ```
   import boto3
   from sagemaker import get_execution_role, Session
                   
   aws_region = "aws-region"
   
   boto_session = boto3.Session(region_name=aws_region)
   sagemaker_client = boto_session.client("sagemaker")
   
   role = get_execution_role()
   
   bucket = Session(boto_session).default_bucket()
   
   model_name1 = "name-of-your-first-model"
   model_name2 = "name-of-your-second-model"
   
   sagemaker_client.create_model(
       ModelName = model_name1,
       ExecutionRoleArn = role,
       Containers=[
           {
               "Image": "ecr-image-uri-for-first-model",
               "ModelDataUrl": "s3-location-of-trained-first-model" 
           }
       ]
   )
   
   sagemaker_client.create_model(
       ModelName = model_name2,
       ExecutionRoleArn = role,
       Containers=[
           {
               "Image": "ecr-image-uri-for-second-model",
               "ModelDataUrl": "s3-location-of-trained-second-model" 
           }
       ]
   )
   ```

1.  Create an endpoint configuration\. Specify both your production and shadow variants in the configuration\. 

   ```
   endpoint_config_name = name-of-your-endpoint-config
   
   create_endpoint_config_response = sagemaker_client.create_endpoint_config(
       EndpointConfigName=endpoint_config_name,
       ProductionVariants=[
           {
               "VariantName": name-of-your-production-variant,
               "ModelName": model_name1,
               "InstanceType": "ml.m5.xlarge",
               "InitialInstanceCount": 1,
               "InitialVariantWeight": 1,
           }
       ],
       ShadowProductionVariants=[
           {
               "VariantName": name-of-your-shadow-variant,
               "ModelName": model_name2,
               "InstanceType": "ml.m5.xlarge",
               "InitialInstanceCount": 1,
               "InitialVariantWeight": 1,
           }
      ]
   )
   ```

1. Create an endpoint\.

   ```
   create_endpoint_response = sm.create_endpoint(
       EndpointName=name-of-your-endpoint,
       EndpointConfigName=endpoint_config_name,
   )
   ```