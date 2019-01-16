# Deploy Models Compiled with Neo with Hosting Services<a name="neo-deployment-hosting-services"></a>

To deploy a Neo\-compiled model to an HTTPS endpoint, you must configure and create the endpoint for the model using Amazon SageMaker hosting services\. Currently developers can use Amazon SageMaker APIs to deploy modules on to ml\.c5, ml\.c4, ml\.m5, ml\.m4, ml\.p3, and ml\.p2 instances\.

When you deploy a compiled model, you need to use the same instance for the target that you used for compilation\. This creates an Amazon SageMaker endpoint that you can use to perform inferences\. There are three options available for deploying Neo\-compiled models:

**Topics**
+ [Deploy Neo\-Compiled Models with the CLI](neo-deployment-hosting-services-cli.md)
+ [Deploy Neo\-Compiled Models from the Amazon SageMaker Console](neo-deployment-hosting-services-console.md)
+ [Deploy Neo\-Compiled Models with the Amazon SageMaker SDK](neo-deployment-hosting-services-sdk.md)