# Supported Instance Types and Frameworks<a name="neo-supported-cloud"></a>

Amazon SageMaker Neo supports popular deep learning frameworks for both compilation and deployment\. You can deploy your model to cloud instances, AWS Inferentia instance types, or Amazon Elastic Inference accelerators\.

The following describes frameworks SageMaker Neo supports and the target cloud instances you can compile and deploy to\. For information on how to deploy your compiled model to a cloud or Inferentia instance, see [Deploy a Model with Cloud Instances](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-deployment-hosting-services.html)\. For information on how to deploy your compiled model with Elastic Inference accelerators, see [Use EI on Amazon SageMaker Hosted Endpoints](ei-endpoints.md)\.

## Cloud Instances<a name="neo-supported-cloud-instances"></a>

SageMaker Neo supports the following deep learning frameworks for CPU and GPU cloud instances: 


| Framework | Framework Version | Model Version | Models | Model Formats \(packaged in \*\.tar\.gz\) | Toolkits | 
| --- | --- | --- | --- | --- | --- | 
| MXNet | 1\.8\.0 | Supports 1\.8\.0 or earlier | Image Classification, Object Detection, Semantic Segmentation, Pose Estimation, Activity Recognition | One symbol file \(\.json\) and one parameter file \(\.params\) | GluonCV v0\.8\.0 | 
| ONNX | 1\.7\.0 | Supports 1\.7\.0 or earlier | Image Classification, SVM | One model file \(\.onnx\) |  | 
| Keras | 2\.2\.4 | Supports 2\.2\.4 or earlier | Image Classification | One model definition file \(\.h5\) |  | 
| PyTorch | 1\.4, 1\.5, 1\.6, 1\.7 or 1\.8 | Supports 1\.4, 1\.5, 1\.6, 1\.7 and 1\.8 | Image Classification | One model definition file \(\.pt or \.pth\) with input dtype of float32 |  | 
| TensorFlow | 1\.15\.3 or 2\.9 | Supports 1\.15\.3 and 2\.9 | Image Classification | For saved models, one \.pb or one \.pbtxt file and a variables directory that contains variables For frozen models, only one \.pb or \.pbtxt file |  | 
| XGBoost | 1\.3\.3 | Supports 1\.3\.3 or earlier | Decision Trees | One XGBoost model file \(\.model\) where the number of nodes in a tree is less than 2^31 |  | 

**Note**  
“Model Version” is the version of the framework used to train and export the model\. 

## Instance Types<a name="neo-supported-cloud-instances-types"></a>

 You can deploy your SageMaker compiled model to one of the cloud instances listed below: 


| Instance | Compute Type | 
| --- | --- | 
| `ml_c4` | Standard | 
| `ml_c5` | Standard | 
| `ml_m4` | Standard | 
| `ml_m5` | Standard | 
| `ml_p2` | Accelerated computing | 
| `ml_p3` | Accelerated computing | 
| `ml_g4dn` | Accelerated computing | 

 For information on the available vCPU, memory, and price per hour for each instance type, see [Amazon SageMaker Pricing](https://aws.amazon.com/sagemaker/pricing/)\. 

**Note**  
When compiling for `ml_*` instances using PyTorch framework, use **Compiler options** field in **Output Configuration** to provide the correct data type \(`dtype`\) of the model’s input\.  
The default is set to `"float32"`\.

## AWS Inferentia<a name="neo-supported-inferentia"></a>

 SageMaker Neo supports the following deep learning frameworks for Inferentia: 


| Framework | Framework Version | Model Version | Models | Model Formats \(packaged in \*\.tar\.gz\) | Toolkits | 
| --- | --- | --- | --- | --- | --- | 
| MXNet | 1\.5 or 1\.8  | Supports 1\.8, 1\.5 and earlier | Image Classification, Object Detection, Semantic Segmentation, Pose Estimation, Activity Recognition | One symbol file \(\.json\) and one parameter file \(\.params\) | GluonCV v0\.8\.0 | 
| PyTorch | 1\.7, 1\.8 or 1\.9 | Supports 1\.9 and earlier | Image Classification | One model definition file \(\.pt or \.pth\) with input dtype of float32 |  | 
| TensorFlow | 1\.15 or 2\.5 | Supports 2\.5, 1\.15 and earlier | Image Classification | For saved models, one \.pb or one \.pbtxt file and a variables directory that contains variables For frozen models, only one \.pb or \.pbtxt file |  | 

**Note**  
 “Model Version” is the version of the framework used to train and export the model\. 

You can deploy your SageMaker Neo\-compiled model to AWS Inferentia\-based Amazon EC2 Inf1 instances\. AWS Inferentia is Amazon's first custom silicon chip designed to accelerate deep learning\. Currently, you can use the `ml_inf1` instance to deploy your compiled models\. 

## Amazon Elastic Inference<a name="neo-supported-ei"></a>

SageMaker Neo supports the following deep learning frameworks for Elastic Inference:


| Framework | Framework Version | Model Version | Models | Model Formats \(packaged in \*\.tar\.gz\) | 
| --- | --- | --- | --- | --- | 
| TensorFlow | 2\.3\.2 | Supports 2\.3 | Image Classification, Object Detection, Semantic Segmentation, Pose Estimation, Activity Recognition | For saved models, one \.pb or one \.pbtxt file and a variables directory that contains variables\. For frozen models, only one \.pb or \.pbtxt file\. | 

You can deploy your SageMaker Neo\-compiled model to an Elastic Inference Accelerator\. For more information, see [Use EI on Amazon SageMaker Hosted Endpoints](ei-endpoints.md)\.