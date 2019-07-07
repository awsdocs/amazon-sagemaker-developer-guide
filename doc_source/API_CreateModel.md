# CreateModel<a name="API_CreateModel"></a>

Creates a model in Amazon SageMaker\. In the request, you name the model and describe a primary container\. For the primary container, you specify the docker image containing inference code, artifacts \(from prior training\), and custom environment map that the inference code uses when you deploy the model for predictions\.

Use this API to create a model if you want to use Amazon SageMaker hosting services or run a batch transform job\.

To host your model, you create an endpoint configuration with the `CreateEndpointConfig` API, and then create an endpoint with the `CreateEndpoint` API\. Amazon SageMaker then deploys all of the containers that you defined for the model in the hosting environment\. 

To run a batch transform using your model, you start a job with the `CreateTransformJob` API\. Amazon SageMaker uses your model and your dataset to get inferences which are then saved to a specified S3 location\.

In the `CreateModel` request, you must define a container with the `PrimaryContainer` parameter\.

In the request, you also provide an IAM role that Amazon SageMaker can assume to access model artifacts and docker image for deployment on ML compute hosting instances or for batch transform jobs\. In addition, you also use the IAM role to manage permissions the inference code needs\. For example, if the inference code access any other AWS resources, you grant necessary permissions via this role\.

## Request Syntax<a name="API_CreateModel_RequestSyntax"></a>

```
{
   "[Containers](#SageMaker-CreateModel-request-Containers)": [ 
      { 
         "[ContainerHostname](API_ContainerDefinition.md#SageMaker-Type-ContainerDefinition-ContainerHostname)": "string",
         "[Environment](API_ContainerDefinition.md#SageMaker-Type-ContainerDefinition-Environment)": { 
            "string" : "string" 
         },
         "[Image](API_ContainerDefinition.md#SageMaker-Type-ContainerDefinition-Image)": "string",
         "[ModelDataUrl](API_ContainerDefinition.md#SageMaker-Type-ContainerDefinition-ModelDataUrl)": "string",
         "[ModelPackageName](API_ContainerDefinition.md#SageMaker-Type-ContainerDefinition-ModelPackageName)": "string"
      }
   ],
   "[EnableNetworkIsolation](#SageMaker-CreateModel-request-EnableNetworkIsolation)": boolean,
   "[ExecutionRoleArn](#SageMaker-CreateModel-request-ExecutionRoleArn)": "string",
   "[ModelName](#SageMaker-CreateModel-request-ModelName)": "string",
   "[PrimaryContainer](#SageMaker-CreateModel-request-PrimaryContainer)": { 
      "[ContainerHostname](API_ContainerDefinition.md#SageMaker-Type-ContainerDefinition-ContainerHostname)": "string",
      "[Environment](API_ContainerDefinition.md#SageMaker-Type-ContainerDefinition-Environment)": { 
         "string" : "string" 
      },
      "[Image](API_ContainerDefinition.md#SageMaker-Type-ContainerDefinition-Image)": "string",
      "[ModelDataUrl](API_ContainerDefinition.md#SageMaker-Type-ContainerDefinition-ModelDataUrl)": "string",
      "[ModelPackageName](API_ContainerDefinition.md#SageMaker-Type-ContainerDefinition-ModelPackageName)": "string"
   },
   "[Tags](#SageMaker-CreateModel-request-Tags)": [ 
      { 
         "[Key](API_Tag.md#SageMaker-Type-Tag-Key)": "string",
         "[Value](API_Tag.md#SageMaker-Type-Tag-Value)": "string"
      }
   ],
   "[VpcConfig](#SageMaker-CreateModel-request-VpcConfig)": { 
      "[SecurityGroupIds](API_VpcConfig.md#SageMaker-Type-VpcConfig-SecurityGroupIds)": [ "string" ],
      "[Subnets](API_VpcConfig.md#SageMaker-Type-VpcConfig-Subnets)": [ "string" ]
   }
}
```

## Request Parameters<a name="API_CreateModel_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [Containers](#API_CreateModel_RequestSyntax) **   <a name="SageMaker-CreateModel-request-Containers"></a>
Specifies the containers in the inference pipeline\.  
Type: Array of [ContainerDefinition](API_ContainerDefinition.md) objects  
Array Members: Maximum number of 5 items\.  
Required: No

 ** [EnableNetworkIsolation](#API_CreateModel_RequestSyntax) **   <a name="SageMaker-CreateModel-request-EnableNetworkIsolation"></a>
Isolates the model container\. No inbound or outbound network calls can be made to or from the model container\.  
The Semantic Segmentation built\-in algorithm does not support network isolation\.
Type: Boolean  
Required: No

 ** [ExecutionRoleArn](#API_CreateModel_RequestSyntax) **   <a name="SageMaker-CreateModel-request-ExecutionRoleArn"></a>
The Amazon Resource Name \(ARN\) of the IAM role that Amazon SageMaker can assume to access model artifacts and docker image for deployment on ML compute instances or for batch transform jobs\. Deploying on ML compute instances is part of model hosting\. For more information, see [Amazon SageMaker Roles](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html)\.   
To be able to pass this role to Amazon SageMaker, the caller of this API must have the `iam:PassRole` permission\.
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.  
Pattern: `^arn:aws[a-z\-]*:iam::\d{12}:role/?[a-zA-Z_0-9+=,.@\-_/]+$`   
Required: Yes

 ** [ModelName](#API_CreateModel_RequestSyntax) **   <a name="SageMaker-CreateModel-request-ModelName"></a>
The name of the new model\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

 ** [PrimaryContainer](#API_CreateModel_RequestSyntax) **   <a name="SageMaker-CreateModel-request-PrimaryContainer"></a>
The location of the primary docker image containing inference code, associated artifacts, and custom environment map that the inference code uses when the model is deployed for predictions\.   
Type: [ContainerDefinition](API_ContainerDefinition.md) object  
Required: No

 ** [Tags](#API_CreateModel_RequestSyntax) **   <a name="SageMaker-CreateModel-request-Tags"></a>
An array of key\-value pairs\. For more information, see [Using Cost Allocation Tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html#allocation-what) in the *AWS Billing and Cost Management User Guide*\.   
Type: Array of [Tag](API_Tag.md) objects  
Array Members: Minimum number of 0 items\. Maximum number of 50 items\.  
Required: No

 ** [VpcConfig](#API_CreateModel_RequestSyntax) **   <a name="SageMaker-CreateModel-request-VpcConfig"></a>
A [VpcConfig](https://docs.aws.amazon.com/sagemaker/latest/dg/API_VpcConfig.html) object that specifies the VPC that you want your model to connect to\. Control access to and from your model container by configuring the VPC\. `VpcConfig` is used in hosting services and in batch transform\. For more information, see [Protect Endpoints by Using an Amazon Virtual Private Cloud](https://docs.aws.amazon.com/sagemaker/latest/dg/host-vpc.html) and [Protect Data in Batch Transform Jobs by Using an Amazon Virtual Private Cloud](https://docs.aws.amazon.com/sagemaker/latest/dg/batch-vpc.html)\.  
Type: [VpcConfig](API_VpcConfig.md) object  
Required: No

## Response Syntax<a name="API_CreateModel_ResponseSyntax"></a>

```
{
   "[ModelArn](#SageMaker-CreateModel-response-ModelArn)": "string"
}
```

## Response Elements<a name="API_CreateModel_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [ModelArn](#API_CreateModel_ResponseSyntax) **   <a name="SageMaker-CreateModel-response-ModelArn"></a>
The ARN of the model created in Amazon SageMaker\.  
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:model/.*` 

## Errors<a name="API_CreateModel_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ResourceLimitExceeded**   
 You have exceeded an Amazon SageMaker resource limit\. For example, you might have too many training jobs created\.   
HTTP Status Code: 400

## See Also<a name="API_CreateModel_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/CreateModel) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/CreateModel) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/CreateModel) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/CreateModel) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/CreateModel) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/CreateModel) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/CreateModel) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/CreateModel) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/CreateModel) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/CreateModel) 