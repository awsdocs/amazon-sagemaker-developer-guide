# Docker Container Basics<a name="docker-basics"></a>

Docker is a program that performs operating system\-level virtualization for installing, distributing, and managing software\. It packages applications and their dependencies into virtual containers that provide isolation, portability, and security\. With Docker, you can ship code faster, standardize application operations, seamlessly move code, and economize by improving resource utilization\. For more general information about Docker, see [Docker overview](https://docs.docker.com/engine/docker-overview/)\.

The following information outlines the most significant aspects of using Docker containers with Amazon SageMaker\.

**SageMaker Functions**

SageMaker uses Docker containers in the backend to manage training and inference processes\. SageMaker abstracts away from this process, so it happens automatically when an estimator is used\. While you don't need to use Docker containers explicitly with SageMaker for most use cases, you can use Docker containers to extend and customize SageMaker functionality\. 

**Containers with SageMaker Studio**

SageMaker Studio runs from a Docker container and uses it to manage functionality\. As a result, you cannot create and upload a Docker container from a SageMaker Studio instance\. However, you can use a prebuilt SageMaker container as long as that container was created outside of Studio\.