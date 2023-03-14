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
Each user in a domain gets a user directory on a shared Amazon Elastic File System volume in the image\. The location of the current user's directory on the Amazon EFS volume is configurable\. By default, the location of the directory is `/home/sagemaker-user`\.  
SageMaker configures POSIX UID/GID mappings between the image and the host\. This defaults to mapping the root user's UID/GID \(0/0\) to the UID/GID on the host\.  
You can specify these values using the [CreateAppImageConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAppImageConfig.html) API\.

**GID/UID limits**  
Amazon SageMaker Studio only supports the following `DefaultUID` and `DefaultGID` combinations:   
+  DefaultUID: 1000 and DefaultGID: 100, which corresponds to a non\-priveleged user\.
+  DefaultUID: 0 and DefaultGID: 0, which corresponds to root access\.

**Metadata**  
A metadata file is located at `/opt/ml/metadata/resource-metadata.json`\. No additional environment variables are added to the variables defined in the image\. For more information, see [Get App Metadata](notebooks-run-and-manage-metadata.md#notebooks-run-and-manage-metadata-app)\.

**GPU**  
On a GPU instance, the image is run with the `--gpus` option\. Only the CUDA toolkit should be included in the image not the NVIDIA drivers\. For more information, see [NVIDIA User Guide](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/user-guide.html)\.

**Metrics and logging**  
Logs from the KernelGateway process are sent to Amazon CloudWatch in the customer’s account\. The name of the log group is `/aws/sagemaker/studio`\. The name of the log stream is `$domainID/$userProfileName/KernelGateway/$appName`\.

**Image size**  
Limited to 25 GB\. To view the size of your image, run `docker image ls`\.  


## Sample Dockerfile<a name="studio-byoi-specs-sample"></a>

The following sample Dockerfile creates an image based Amazon Linux 2, installs third party packages and the `python3` kernel, and sets the scope to the non\-privileged user\.

```
FROM public.ecr.aws/amazonlinux/amazonlinux:2

ARG NB_USER="sagemaker-user"
ARG NB_UID="1000"
ARG NB_GID="100"

RUN \
    yum install --assumeyes python3 shadow-utils && \
    useradd --create-home --shell /bin/bash --gid "${NB_GID}" --uid ${NB_UID} ${NB_USER} && \
    yum clean all && \
    python3 -m pip install ipykernel && \
    python3 -m ipykernel install

USER ${NB_UID}
```