# Supported Frameworks, Devices, Systems, and Architectures<a name="neo-supported-devices-edge"></a>

Amazon SageMaker Neo supports the following frameworks and edge devices\. 

## Supported Frameworks<a name="neo-supported-devices-edge-frameworks"></a>


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

## Devices<a name="neo-supported-devices-edge-devices"></a>

You can select a device using the dropdown list in the console or API\. You can choose from one of the following edge devices: 


| Device List | System on a Chip \(SoC\) | Operating System | Architecture | Accelerator | Compiler Options | 
| --- | --- | --- | --- | --- | --- | 
| jetson\_tx1 |  | LINUX | ARM64 | NVIDIA | \{'gpu\-code': 'sm\_53', 'trt\-ver': '6\.0\.1', 'cuda\-ver': '10\.0'\} | 
| jetson\_tx2 |  | LINUX | ARM64 | NVIDIA | \{'gpu\-code': 'sm\_62', 'trt\-ver': '6\.0\.1', 'cuda\-ver': '10\.0'\} | 
| jetson\_nano |  | LINUX | ARM64 | NVIDIA | \{'gpu\-code': 'sm\_53', 'trt\-ver': '5\.0\.6', 'cuda\-ver': '10\.0'\} | 
| jetson\_xavier |  | LINUX | ARM64 | NVIDIA | \{'gpu\-code': 'sm\_72', 'trt\-ver': '5\.1\.6', 'cuda\-ver': '10\.0'\} | 
| rasp3b | ARM A56 | LINUX | ARM\_EABIHF |  | \{'mattr': \['\+neon'\]\} | 
| rasp4 | ARM A72 |  |  |  |  | 
| imx8qm | NXP imx8 | LINUX | ARM64 |  |  | 
| deeplens | Intel Atom | LINUX | X86\_64 | INTEL\_GRAPHICS |  | 
| rk3399 |  | LINUX | ARM64 | MALI |  | 
| rk3288 |  | LINUX | ARM\_EABIHF | MALI |  | 
| aisage |  | LINUX | ARM64 | MALI |  | 
| sbe\_c |  | LINUX | x86\_64 |  | \{'mcpu': 'core\-avx2'\} | 
| qcs605 |  | ANDROID | ARM64 | MALI | \{'ANDROID\_PLATFORM': 27\} | 
| qcs603 |  | ANDROID | ARM64 | MALI | \{'ANDROID\_PLATFORM': 27\} | 

## Systems and Architectures<a name="neo-supported-cloud-granular"></a>

The following look\-up tables provide information regarding available operating systems and architectures for Neo model compilation jobs\. 

------
#### [ Linux ]


|  | X86\_64 | X86 | ARM64 | ARM\_EABIHF | ARM\_EABI | 
| --- | --- | --- | --- | --- | --- | 
| No accelerator \(CPU\) | X |  | X | X | X | 
| Nvidia GPU | X |  | X |  |  | 
| Intel\_Graphics | X |  |  |  |  | 
| ARM Mali |  |  | X | X | X | 

------
#### [ Android ]


|  | X86\_64 | X86 | ARM64 | ARM\_EABIHF | ARM\_EABI | 
| --- | --- | --- | --- | --- | --- | 
| No accelerator \(CPU\) | X | X | X |  | X | 
| Nvidia GPU |  |  |  |  |  | 
| Intel\_Graphics | X | X |  |  |  | 
| ARM Mali |  |  | X |  | X | 

------
#### [ Windows ]


|  | X86\_64 | X86 | ARM64 | ARM\_EABIHF | ARM\_EABI | 
| --- | --- | --- | --- | --- | --- | 
| No accelerator \(CPU\) | X | X |  |  |  | 

------