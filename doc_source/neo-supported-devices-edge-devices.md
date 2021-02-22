# Supported Devices, Chip Architectures, and Systems<a name="neo-supported-devices-edge-devices"></a>

Amazon SageMaker Neo supports the following devices, chip architectures, and operating systems\.

## Devices<a name="neo-supported-edge-devices"></a>

You can select a device using the dropdown list in the [Amazon SageMaker console](https://console.aws.amazon.com/sagemaker/) or by specifying the `TargetDevice` in the output configuration of the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateCompilationJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateCompilationJob.html) API\.

You can choose from one of the following edge devices: 


| Device List | System on a Chip \(SoC\) | Operating System | Architecture | Accelerator | Compiler Options Example | 
| --- | --- | --- | --- | --- | --- | 
| aisage |  | Linux | ARM64 | Mali |  | 
| amba\_cv22 | CV22 | Arch Linux | ARM64 |  |  | 
| coreml |  | iOS, macOS |  |  | \{"class\_labels": "imagenet\_labels\_1000\.txt"\} | 
| deeplens | Intel Atom | Linux | X86\_64 | Intel Graphics |  | 
| imx8qm | NXP imx8 | Linux | ARM64 |  |  | 
| jacinto\_tda4vm | TDA4VM | Linux | ARM | TDA4VM |  | 
| jetson\_nano |  | Linux | ARM64 | NVIDIA | \{'gpu\-code': 'sm\_53', 'trt\-ver': '5\.0\.6', 'cuda\-ver': '10\.0'\} | 
| jetson\_tx1 |  | Linux | ARM64 | NVIDIA | \{'gpu\-code': 'sm\_53', 'trt\-ver': '6\.0\.1', 'cuda\-ver': '10\.0'\} | 
| jetson\_tx2 |  | Linux | ARM64 | NVIDIA | \{'gpu\-code': 'sm\_62', 'trt\-ver': '6\.0\.1', 'cuda\-ver': '10\.0'\} | 
| jetson\_xavier |  | Linux | ARM64 | NVIDIA | \{'gpu\-code': 'sm\_72', 'trt\-ver': '5\.1\.6', 'cuda\-ver': '10\.0'\} | 
| qcs605 |  | Android | ARM64 | Mali | \{'ANDROID\_PLATFORM': 27\} | 
| qcs603 |  | Android | ARM64 | Mali | \{'ANDROID\_PLATFORM': 27\} | 
| rasp3b | ARM A56 | Linux | ARM\_EABIHF |  | \{'mattr': \['\+neon'\]\} | 
| rasp4 | ARM A72 |  |  |  |  | 
| rk3288 |  | Linux | ARM\_EABIHF | Mali |  | 
| rk3399 |  | Linux | ARM64 | Mali |  | 
| sbe\_c |  | Linux | x86\_64 |  | \{'mcpu': 'core\-avx2'\} | 
| sitara\_am57x | AM57X | Linux | ARM64 | EVE and/or C66x DSP |  | 
| x86\_win32 |  | Windows 10 | X86\_32 |  |  | 
| x86\_win64 |  | Windows 10 | X86\_32 |  |  | 

For more information about JSON key\-value compiler options for each target device, see the `CompilerOptions` field in the [`OutputConfig` API](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_OutputConfig.html) data type\.

## Systems and Chip Architectures<a name="neo-supported-edge-granular"></a>

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