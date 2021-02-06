# Supported Frameworks<a name="neo-supported-devices-edge-frameworks"></a>

Amazon SageMaker Neo supports the following frameworks\. 


| Framework | Framework Version | Model Version | Models | Model Formats \(packaged in \*\.tar\.gz\) | Toolkits | 
| --- | --- | --- | --- | --- | --- | 
| MXNet | 1\.6\.0 | Supports 1\.6\.0 or earlier | Image Classification, Object Detection, Semantic Segmentation, Pose Estimation, Activity Recognition | One symbol file \(\.json\) and one parameter file \(\.params\) | GluonCV v0\.8\.0 | 
| ONNX | 1\.5\.0 | Supports 1\.5\.0 or earlier | Image Classification, SVM | One model file \(\.onnx\) |  | 
| Keras | 2\.2\.4 | Supports 2\.2\.4 or earlier | Image Classification | One model definition file \(\.h5\) |  | 
| PyTorch | 1\.6\.0 | Supports 1\.6\.0 or earlier | Image Classification | One model definition file \(\.pth\) |  | 
| TensorFlow | 1\.15\.0 | Supports 1\.15\.0 or earlier | Image Classification | \*For saved models, one \.pb or one \.pbtxt file and a variables directory that contains variables \*For frozen models, only one \.pb or \.pbtxt file |  | 
| TensorFlow\-Lite | 1\.13\.1 | Supports 1\.13\.1 or earlier | Image Classification, Object Detection | One model definition flatbuffer file \(\.tflite\) |  | 
| XGBoost | 0\.9 | Supports 0\.9 or earlier | Decision Trees | One XGBoost model file \(\.model\) where the number of nodes in a tree is less than 2^31 |  | 
| DARKNET |  |  | Image Classification, Object Detection | nOne config \(\.cfg\) file and one weights \(\.weights\) file |  | 