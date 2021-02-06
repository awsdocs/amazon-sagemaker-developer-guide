# Supported Devices, Chip Architectures, and Systems<a name="neo-supported-devices-edge-devices"></a>

Amazon SageMaker Neo supports the following devices, chip architectures, and operating systems\.

## Devices<a name="neo-supported-edge-devices"></a>

You can select a device using the dropdown list in the [Amazon SageMaker console](https://console.aws.amazon.com/sagemaker/) or by specifying the `TargetDevice` in the output configuration of the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateCompilationJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateCompilationJob.html) API\.

You can choose from one of the following edge devices: 


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