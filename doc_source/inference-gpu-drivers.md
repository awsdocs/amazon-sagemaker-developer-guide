# Best practices to minimize interruptions during GPU driver upgrades<a name="inference-gpu-drivers"></a>

SageMaker Model Deployment upgrades GPU drivers on the ML instances for Real\-time, Batch, and Asynchronous Inference options over time to provide customers access to improvements from the driver providers\. Below you can see the GPU version supported for each Inference option\. Different driver versions can change how your model interacts with the GPUs\. Below are some strategies to help you understand how your application works with different driver versions\. 

## Current versions and supported instance families<a name="inference-gpu-drivers-versions"></a>

Amazon SageMaker Inference supports the following drivers and instance families:


| Service | GPU | Driver version | Instance types | 
| --- | --- | --- | --- | 
| Real\-time | NVIDIA | 470\.57\.02 | ml\.p2\.\*, ml\.p3\.\*, ml\.g4dn\.\* | 
| Batch | NVIDIA | 470\.57\.02 | ml\.p2\.\*, ml\.p3\.\*, ml\.g4dn\.\* | 
| Asynchronous Inference | NVIDIA | 470\.57\.02 | ml\.p2\.\*, ml\.p3\.\*, ml\.g4dn\.\* | 

## Troubleshoot your model container with GPU capabilities<a name="inference-gpu-drivers-troubleshoot"></a>

If you encounter an issue when running your GPU workload, see the following guidance:

### GPU card detection failure or NVIDIA initialization error<a name="collapsible-section-0"></a>

Run the `nvidia-smi` \(NVIDIA System Management Interface\) command from within the Docker container\. If the NVIDIA System Management Interface detects a GPU detection error or NVIDIA initialization error, it will return the following error message:

```
Failed to initialize NVML: Driver/library version mismatch
```

Based on your use case, follow these best practices to resolve the failure or error:
+ Follow the best practice recommendation described in the [If you bring your own \(BYO\) model containers](#collapsible-byoc) dropdown\.
+ Follow the best practice recommendation described in the [If you use a CUDA compatibility layer](#collapsible-cuda-compat) dropdown\.

Refer to the [NVIDIA System Management Interface page](https://developer.nvidia.com/nvidia-system-management-interface) on the NVIDIA website for more information\.

## Best practices for working with mismatched driver versions<a name="inference-gpu-drivers-cuda-toolkit-updates"></a>

The following provides information on how to update your GPU driver:

### The driver my container depends on is lower than the version on the ML GPU instance<a name="collapsible-driver-dependency-lower"></a>

No action is required\. NVIDIA provides backwards compatibility\.

### The driver my container depends on is greater than the version on the ML GPU instances<a name="collapsible-driver-dependency-higher"></a>

If it is a minor version difference, no action is required\. NVIDIA provides minor version forward compatibility\.

If it is a major version difference, the CUDA Compatibility Package will need to be installed\. Please refer to [CUDA Compatibility Package](https://docs.nvidia.com/deploy/cuda-compatibility/index.html) in the NVIDIA documentation\.

**Important**  
The CUDA Compatibility Package is not backwards compatible so it needs to be disabled if the driver version on the instance is greater than the CUDA Compatibility Package version\.

### If you bring your own \(BYO\) model containers<a name="collapsible-byoc"></a>

Ensure no NVIDIA driver packages are bundled in the image which could cause conflict with on host NVIDIA driver version\.

### If you use a CUDA compatibility layer<a name="collapsible-cuda-compat"></a>

To verify if the platform Nvidia driver version supports the CUDA Compatibility Package version installed in the model container, see the [CUDA documentation](https://docs.nvidia.com/deploy/cuda-compatibility/index.html#use-the-right-compat-package)\. If the platform Nvidia driver version does not support the CUDA Compatibility Package version, you can disable or remove the CUDA Compatibility Package from the model container image\. If the CUDA compatibility libs version is supported by the latest Nvidia driver version, we suggest that you enable the CUDA Compatibility Package based on the detected Nvidia driver version for future compatibility by adding the code snippet below into the container start up shell script \(at the `ENTRYPOINT` script\)\.

The script demonstrates how to dynamically switch the use of the CUDA Compatibility Package based on the detected Nvidia driver version on the deployed host for your model container\. When SageMaker releases a newer Nvidia driver version, the installed CUDA Compatibility Package can be turned off automatically if the CUDA application is supported natively on the new driver\.

```
#!/bin/bash
 
verlte() {
    [ "$1" = "$2" ] $$ return 1 || [ "$2" = "`echo -e "$1\n$2" | sort -V | head -n1`" ]
}
 
if [ -f /usr/local/cuda/compat/libcuda.so.1 ]; then
    cat /usr/local/cuda/version.txt
    CUDA_COMPAT_MAX_DRIVER_VERSION=$(readlink /usr/local/cuda/compat/libcuda.so.1 |cut -d'.' -f 3-)
    echo "CUDA compat package requires Nvidia driver ⩽${CUDA_COMPAT_MAX_DRIVER_VERSION}"
    NVIDIA_DRIVER_VERSION=$(sed -n 's/^NVRM.*Kernel Module *\([0-9.]*\).*$/\1/p' /proc/driver/nvidia/version 2>/dev/null || true)
    echo "Current installed Nvidia driver version is ${NVIDIA_DRIVER_VERSION}"
    if [ $(verlte $CUDA_COMPAT_MAX_DRIVER_VERSION $NVIDIA_DRIVER_VERSION) ]; then
        echo "Setup CUDA compatibility libs path to LD_LIBRARY_PATH"
        export LD_LIBRARY_PATH=/usr/local/cuda/compat:$LD_LIBRARY_PATH
        echo $LD_LIBRARY_PATH
    else
        echo "Skip CUDA compat libs setup as newer Nvidia driver is installed"
    fi
else
    echo "Skip CUDA compat libs setup as package not found"
fi
```