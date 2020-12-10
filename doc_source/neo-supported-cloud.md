# Supported Instance Types and Frameworks<a name="neo-supported-cloud"></a>

Amazon SageMaker Neo supports popular deep learning frameworks for both compilation and deployment\. You can deploy your model either to a cloud instance or to AWS Inferentia instance types\. The following describes frameworks SageMaker Neo supports and the cloud and Inferentia instance types you can deploy your model to\. 

 For information on how to deploy your compiled model to a cloud or Inferentia instance, see [Deploy a Model with Cloud Instances](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-deployment-hosting-services.html)\. 

## Cloud Instances<a name="neo-supported-cloud-instances"></a>

SageMaker Neo supports the following deep learning frameworks for CPU and GPU cloud instances: 


| Framework | Framework Version | Model Version | Models | Model Formats \(packaged in \*\.tar\.gz\) | Toolkits | 
| --- | --- | --- | --- | --- | --- | 
| MXNet | 1\.7\.0 | Supports 1\.7\.0 or earlier | Image Classification, Object Detection, Semantic Segmentation, Pose Estimation, Activity Recognition | One symbol file \(\.json\) and one parameter file \(\.params\) | GluonCV v0\.8\.0 | 
| ONNX | 1\.5\.0 | Supports 1\.5\.0 or earlier | Image Classification, SVM | One model file \(\.onnx\) |  | 
| Keras | 2\.2\.4 | Supports 2\.2\.4 or earlier | Image Classification | One model definition file \(\.h5\) |  | 
| PyTorch | 1\.4\.0 | Supports 1\.4\.0 or earlier | Image Classification | One model definition file \(\.pt or \.pth\) with input dtype of float32 |  | 
| TensorFlow | 1\.15\.0 | Supports 1\.15\.0 or earlier | Image Classification | \*For saved models, one \.pb or one \.pbtxt file and a variables directory that contains variables \*For frozen models, only one \.pb or \.pbtxt file |  | 
| TensorFlow\-Lite | 1\.13\.1 | Supports 1\.13\.1 or earlier | Image Classification, Object Detection | One model definition flatbuffer file \(\.tflite\) |  | 
| XGBoost | 0\.9 | Supports 0\.9 or earlier | Decision Trees | One XGBoost model file \(\.model\) where the number of nodes in a tree is less than 2^31 |  | 
| DARKNET |  |  | Image Classification, Object Detection | One config \(\.cfg\) file and one weights \(\.weights\) file |  | 

**Note**  
“Model Version” is the version of the framework used to train and export the model\. 

 You can deploy your SageMaker compiled model to one of the cloud instances listed below: 
+ `ml_m5` 
+ `ml_c4`
+ `ml_c5`
+ `ml_p2`
+ `ml_p3`
+ `ml_g4dn`

 For information on the available vCPU, memory, and price per hour for each instance type, see [Amazon SageMaker Pricing](https://aws.amazon.com/sagemaker/pricing/)\. 

## AWS Inferentia<a name="neo-supported-inferentia"></a>

 SageMaker Neo supports the following deep learning frameworks for Inferentia: 


| Framework | Framework Version | Model Version | Models | Model Formats \(packaged in \*\.tar\.gz\) | Toolkits | 
| --- | --- | --- | --- | --- | --- | 
| MXNet | 1\.5\.1 | Supports 1\.5\.1 or earlier | Image Classification, Object Detection, Semantic Segmentation, Pose Estimation, Activity Recognition | One symbol file \(\.json\) and one parameter file \(\.params\) | GluonCV v0\.8\.0 | 
| PyTorch | 1\.5\.1 | Supports 1\.5\.1 or earlier | Image Classification | One model definition file \(\.pt or \.pth\) with input dtype of float32 |  | 
| TensorFlow | 1\.15\.0 | Supports 1\.15\.0 or earlier | Image Classification | \*For saved models, one \.pb or one \.pbtxt file and a variables directory that contains variables \*For frozen models, only one \.pb or \.pbtxt file |  | 

**Note**  
 “Model Version” is the version of the framework used to train and export the model\. 

You can deploy your SageMaker Neo\-compiled model to AWS Inferentia\-based Amazon EC2 Inf1 instances\. AWS Inferentia is Amazon's first custom silicon chip designed to accelerate deep learning\. Currently, you can use the `ml_inf1` instace to deploy your compiled models\. 