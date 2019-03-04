# Use Your Own Algorithms with Amazon SageMaker<a name="your-algorithms"></a>

 

You can easily package your own algorithms for use with Amazon SageMaker, regardless of programming language or framework\. Amazon SageMaker is highly flexible\. It allows you to do the following:
+ Use a suitable algorithm provided by Amazon SageMaker for model training and your own inference code, such as code for embedded applications, or devices, or both\.
+ Use your own training algorithm and inference code provided by Amazon SageMaker\. 
+ Use your own training algorithm and your own inference code\. You package the algorithm and inference code in Docker images, and use the images to train a model and deploy it with Amazon SageMaker\.
+ Use deep learning containers provided by Amazon SageMaker for model training and your own inference code\. You provide a script written for the deep learning framework, such as Apache MXNet or TensorFlow\. For more information about training, see [Use Apache MXNet with Amazon SageMaker](mxnet.md) and [Use TensorFlow with Amazon SageMaker](tf.md)\. 

Amazon SageMaker algorithms are packaged as Docker images\. This gives you the flexibility to use almost any algorithm code with Amazon SageMaker, regardless of implementation language, dependent libraries, frameworks, and so on\. For more information on creating Docker images, see [The Dockerfile instructions](https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/#the-dockerfile-instructions)\.

You can provide separate Docker images for the training algorithm and inference code, or you can combine them into a single Docker image\. When creating Docker images for use with Amazon SageMaker, consider the following:
+ Providing two Docker images can increase storage requirements and cost because common libraries might be duplicated\.
+ In general, smaller containers start faster for both training and hosting\. Models train faster and the hosting service can react to increases in traffic by automatically scaling more quickly\.
+ You might be able to write an inference container that is significantly smaller than the training container\. This is especially common when you use GPUs for training, but your inference code is optimized for CPUs\.
+  Amazon SageMaker requires that Docker containers run without privileged access\.
+ Docker containers might send messages to the `Stdout` and `Stderr` files\. Amazon SageMaker sends these messages to Amazon CloudWatch logs in your AWS account\.

The following sections provide detailed information about how Amazon SageMaker interacts with Docker containers and explain Amazon SageMaker requirements for Docker images\. Use this information when creating your own containers\. For general information about Docker containers, see [Docker Basics](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/docker-basics.html) in the *Amazon Elastic Container Service Developer Guide*\.

**Topics**
+ [Using Your Own Training Algorithms](your-algorithms-training-algo.md)
+ [Using Your Own Inference Code](your-algorithms-inference-main.md)
+ [Using Your Own Algorithm Sample Notebooks](adv-bring-own-examples.md)
+ [Create Algorithm and Model Package Resources](sagemaker-mkt-create.md)
+ [Use Algorithm and Model Package Resources](sagemaker-mkt-buy.md)