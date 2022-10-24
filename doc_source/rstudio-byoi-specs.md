# Custom RStudio image specifications<a name="rstudio-byoi-specs"></a>

In this guide, you'll learn custom RStudio image specifications to use when you bring your own image\. There are two sets of requirements that you must satisfy with your custom RStudio image to use it with Amazon SageMaker\. These requirements are imposed by RStudio PBC and the Amazon SageMaker Studio platform\. If either of these sets of requirements aren't satisfied, then your custom image won't function properly\.

## RStudio PBC requirements<a name="rstudio-byoi-specs-rstudio"></a>

RStudio PBC requirements are laid out in the [Using Docker images with RStudio Workbench / RStudio Server Pro, Launcher, and Kubernetes](https://support.rstudio.com/hc/en-us/articles/360019253393-Using-Docker-images-with-RStudio-Server-Pro-Launcher-and-Kubernetes) article\. Follow the instructions in this article to create the base of your custom RStudio image\. 

For instructions about how to install multiple R versions in your custom image, see [Installing multiple versions of R on Linux](https://support.rstudio.com/hc/en-us/articles/215488098)\.

## Amazon SageMaker Studio requirements<a name="rstudio-byoi-specs-studio"></a>

Amazon SageMaker Studio imposes the following set of installation requirements for your RStudio image\.
+ You must use an RStudio base image of at least `2022.02.2-485.pro2`\. For more information, see [Upgrade the RStudio Version ](rstudio-version.md)\.
+ You must install the following packages:

  ```
  yum install -y sudo \
  openjdk-11-jdk \
  libpng-dev \
  && yum clean all \
  && /opt/R/${R_VERSION}/bin/R -e "install.packages('reticulate', repos='https://packagemanager.rstudio.com/cran/__linux__/centos7/latest')" \
  && /opt/python/${PYTHON_VERSION}/bin/pip install --upgrade \
      'boto3>1.0<2.0' \
      'awscli>1.0<2.0' \
      'sagemaker[local]<3'
  ```
+ You must provide default values for the `RSTUDIO_CONNECT_URL` and `RSTUDIO_PACKAGE_MANAGER_URL` environment values\.

  ```
  ENV RSTUDIO_CONNECT_URL "YOUR_CONNECT_URL"
  ENV RSTUDIO_PACKAGE_MANAGER_URL "YOUR_PACKAGE_MANAGER_URL"
  ```

The following general specifications apply to the image that is represented by an RStudio image version\.

**Running the image**  
`ENTRYPOINT` and `CMD` instructions are overridden so that the image is run as an RSession application\.

**Stopping the image**  
The `DeleteApp` API issues the equivalent of a `docker stop` command\. Other processes in the container won’t get the SIGKILL/SIGTERM signals\.

**File system**  
The `/opt/.sagemakerinternal` and `/opt/ml` directories are reserved\. Any data in these directories might not be visible at runtime\.

**User data**  
Each user in a SageMaker domain gets a user directory on a shared Amazon Elastic File System volume in the image\. The location of the current user’s directory on the Amazon Elastic File System volume is `/home/sagemaker-user`\.

**Metadata**  
A metadata file is located at `/opt/ml/metadata/resource-metadata.json`\. No additional environment variables are added to the variables defined in the image\. For more information, see [Get App Metadata](notebooks-run-and-manage-metadata.md#notebooks-run-and-manage-metadata-app)\.

**GPU**  
On a GPU instance, the image is run with the `--gpus` option\. Only the CUDA toolkit should be included in the image, not the NVIDIA drivers\. For more information, see [NVIDIA User Guide](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/user-guide.html)\.

**Metrics and logging**  
Logs from the RSession process are sent to Amazon CloudWatch in the customer’s account\. The name of the log group is `/aws/sagemaker/studio`\. The name of the log stream is `$domainID/$userProfileName/RSession/$appName`\.

**Image size**  
Image size is limited to 25 GB\. To view the size of your image, run `docker image ls`\.