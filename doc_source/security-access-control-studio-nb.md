# Access control and SageMaker Studio notebooks<a name="security-access-control-studio-nb"></a>

Amazon SageMaker Studio uses filesystem and container permissions for access control and isolation of Studio users and notebooks\. This is one of the major differences between Studio notebooks and SageMaker notebook instances\. This topic describes how permissions are set up to avoid security threats, what SageMaker does by default, and how the customer can customize the permissions\. For more information about Studio notebooks and their runtime environment, see [Use Amazon SageMaker Studio Notebooks](notebooks.md)\.

**SageMaker app permissions**

A *run\-as user* is a POSIX user/group which is used to run the JupyterServer app and KernelGateway apps inside the container\.

The run\-as user for the JupyterServer app is sagemaker\-user \(1000\) by default\. This user has sudo permissions to enable the installation of dependencies such as yum packages\.

The run\-as user for the KernelGateway apps is root \(0\) by default\. This user is able to install dependencies using pip/apt\-get/conda\.

Due to the below mentioned user remapping, neither user is able to access resources or make changes to the host instance\.

**User remapping**

SageMaker performs user\-remapping to map a user inside the container to a user on the host instance outside the container\. The range of user IDs \(0 \- 65535\) in the container are mapped to non\-privileged user IDs above 65535 on the instance\. For example, sagemaker\-user \(1000\) inside the container might map to user \(200001\) on the instance, where the number in parentheses is the user ID\. If the customer creates a new user/group inside the container, it won't be privileged on the host instance regardless of the user/group ID\. The root user of the container is also mapped to a non\-privileged user on the instance\. For more information, see [Isolate containers with a user namespace](https://docs.docker.com/engine/security/userns-remap/)\.

**Custom image permissions**

Customers can bring their own custom SageMaker images\. These images can specify a different run\-as user/group to launch the KernelGateway app\. The customer can implement fine grained permission control inside the image, for example, to disable root access or perform other actions\. The same user remapping applies here\. For more information, see [Bring your own SageMaker image](studio-byoi.md)\.

**Container isolation**

Docker keeps a list of default capabilities that the container can use\. SageMaker doesn’t add additional capabilities\. SageMaker adds specific route rules to block requests to Amazon EFS and the [ instance metadata service](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/configuring-instance-metadata-service) \(IMDS\) from the container\. Customers can’t change these route rules from the container\. For more information, see [Runtime privilege and Linux capabilities](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities)\.

**App metadata access**

Metadata used by running apps are mounted to the container with read\-only permission\. Customers aren’t able to modify this metadata from the container\. For the available metadata, see [Get Notebook and App Metadata](notebooks-run-and-manage-metadata.md)\.

**User isolation on EFS**

When you onboard to Studio, SageMaker creates an Amazon Elastic File System \(EFS\) volume for your domain that is shared by all Studio users in the domain\. Each user gets their own private home directory on the EFS volume\. This home directory is used to store the user's notebooks, Git repositories, and other data\. To prevent other users in the domain from accessing the user's data, SageMaker creates a globally unique user ID for the user's profile and applies it as a POSIX user/group ID for the user’s home directory\.

**EBS access**

An Amazon Elastic Block Store \(Amazon EBS\) volume is attached to the host instance and shared across all images\. It's used for the root volume of the notebooks and stores temporary data that's generated inside the container\. The storage isn't persisted when the instance running the notebooks is deleted\. The root user inside the container can't access the EBS volume\.

**IMDS access**

Due to security concerns, access to the Amazon Elastic Compute Cloud \(Amazon EC2\) Instance Metadata Service \(IMDS\) is unavailable in SageMaker Studio\. For more information on IMDS, see [Instance metadata and user data](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html)\.
