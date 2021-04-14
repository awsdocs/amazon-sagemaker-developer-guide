# Custom SageMaker image specifications<a name="studio-byoi-specs"></a>

The following specifications apply to the container image that is represented by a SageMaker image version\.

**Running the image**  
`ENTRYPOINT` and `CMD` instructions are overridden to enable the image to run as a KernelGateway app\.  
Port 8888 in the image is reserved for running the KernelGateway web server\.

**Stopping the image**  
The `DeleteApp` API issues the equivalent of a `docker stop` command\. Other processes in the container won’t get the SIGKILL/SIGTERM signals\.

**Kernel discovery**  
SageMaker recognizes kernels as defined by Jupyter [kernel specs](https://jupyter-client.readthedocs.io/en/latest/kernels.html#kernelspecs)\.  
You can specify a list of kernels to display before running the image\. If not specified, python3 is displayed\. Use the [DescribeAppImageConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeAppImageConfig.html) API to view the list of kernels\.  
Conda environments are recognized as kernel specs by default\. 

**File system**  
The `/opt/.sagemakerinternal` and `/opt/ml` directories are reserved\. Any data in these directories might not be visible at runtime\.

**User data**  
Each user in a Studio domain gets a user directory on a shared Amazon Elastic File System volume in the image\. The location of the current user's directory on the Amazon EFS volume is configurable\. By default, the location of the directory is `/home/sagemaker-user`\.  
SageMaker configures POSIX UID/GID mappings between the image and the host\. This defaults to mapping the root user's UID/GID \(0/0\) to the UID/GID on the host\.  
You can specify these values using the [CreateAppImageConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAppImageConfig.html) API\.

**GID/UID Limits**  
SageMaker Studio only supports UID and GID values in the range between 0 and 65535\. This limit applies to files in every layer of the image\.

**Metadata**  
A metadata file is located at `/opt/ml/metadata/resource-metadata.json`\. No additional environment variables are added to the variables defined in the image\. For more information, see [Get App Metadata](notebooks-run-and-manage-metadata.md#notebooks-run-and-manage-metadata-app)\.

**GPU**  
On a GPU instance, the image is run with the `--gpus` option\. Only the CUDA toolkit should be included in the image not the NVIDIA drivers\. For more information, see [NVIDIA User Guide](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/user-guide.html)\.

**Metrics and Logging**  
Logs from the KernelGateway process are sent to Amazon CloudWatch in the customer’s account\. The name of the log group is `/aws/sagemaker/studio`\. The name of the log stream is `$domainID/$userProfileName/KernelGateway/$appName`\.

**Image size**  
Limited to 11 GB\.