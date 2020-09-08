# Deploy a Model Compiled with Neo with Hosting Services<a name="neo-deployment-hosting-services"></a>

To deploy a Neo\-compiled model to an HTTPS endpoint, you must configure and create the endpoint for the model using Amazon SageMaker hosting services\. Currently developers can use Amazon SageMaker APIs to deploy modules on to ml\.c5, ml\.c4, ml\.m5, ml\.m4, ml\.p3, ml\.p2, and ml\.inf1 instances\.

For [Inf1 instances](http://aws.amazon.com/ec2/instance-types/inf1/), models need to be compiled specifically for ml\.inf1 instances\. Models compiled for other instance types are not guaranteed to work with ml\.inf1 instances\. For more information on compiling your model, see [Use Neo to Compile a Model](neo-job-compilation.md)\.

When you deploy a compiled model, you need to use the same instance for the target that you used for compilation\. This creates a SageMaker endpoint that you can use to perform inferences\. There are three options available for deploying Neo\-compiled models:

**Topics**
+ [Deploy a Model Compiled with Neo \(AWS CLI\)](neo-deployment-hosting-services-cli.md)
+ [Deploy a Model Compiled with Neo \(Console\)](neo-deployment-hosting-services-console.md)
+ [Deploy a Model Compiled with Neo \(Amazon SageMaker SDK\)](neo-deployment-hosting-services-sdk.md)