# Docker Container Basics<a name="docker-basics"></a>

Docker containers provide isolation, portability, and security\. They simplify the creation of highly distributed systems and save money by improving resource utilization\. Docker relies on Linux kernel functionality to provide a lightweight virtualization to package applications into an image that is totally self\-contained\. Docker uses a file, called a Dockerfile, to specify how the image is assembled\. When you have an image, you use Docker to build and run a container based on that image\. 

You can build your Docker images from scratch or base them on other Docker images that you or others have built\. Images are stored in repositories that are indexed and maintained by registries\. An image can be pushed into or pulled out of a repository using its registry address, which is similar to a URL\. Docker Hub is a registry hosted by Docker, Inc\. that provides publicly available repositories\. AWS provides the [Amazon Elastic Container Service \(Amazon ECS\)](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html), a highly scalable, fast container management service\. With Amazon ECS, you can deploy any kind of code in Amazon SageMaker\. You can also create a logical division of labor by creating a deployment team that handles DevOps and infrastructure, and that maintains the container, and a data science team that creates the algorithms and models that are later added to a container\.

Docker builds images by reading the instructions from a Dockerfile text file that contains all of the commands, in order, that are needed to build the image\. A Dockerfile adheres to a specific format and set of instructions\. For more information, see [Dockerfile reference](https://docs.docker.com/engine/reference/builder/)\. Dockerfiles used in SageMaker must also satisfy additional requirements regarding the environmental variables, directory structure, timeouts, and other common functionality\. For information, see [Use Your Own Training Algorithms](your-algorithms-training-algo.md) and [Use Your Own Inference Code](your-algorithms-inference-main.md)\. 

For general information about Docker containers managed by Amazon ECS, see [Docker Basics for Amazon ECS](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/docker-basics.html) in the *Amazon Elastic Container Service Developer Guide*\.

For more information about writing Dockerfiles to build images, see [Best practices for writing Dockerfiles](https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/#the-dockerfile-instructions)\.

For general information about Docker, see the following: 
+ [Docker home page](http://www.docker.com)
+ [Docker overview](https://docs.docker.com/engine/docker-overview/)
+ [Getting Started with Docker](http://www.docker.com/get-started)
+ [Dockerfile reference](https://docs.docker.com/engine/reference/builder/)