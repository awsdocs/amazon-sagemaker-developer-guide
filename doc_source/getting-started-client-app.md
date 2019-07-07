# Step 9: Integrating Amazon SageMaker Endpoints into Internet\-facing Applications<a name="getting-started-client-app"></a>

In a production environment, you might have an internet\-facing application sending requests to the endpoint for inference\. The following high\-level example shows how to integrate your model endpoint into your application\.

1. Create an IAM role that the AWS Lambda service principal can assume\. Give the role permissions to call the Amazon SageMaker `InvokeEndpoint` API\.

1. Create a Lambda function that calls the Amazon SageMaker `InvokeEndpoint` API\.

1. Call the Lambda function from a mobile application\. For an example of how to call a Lambda function from a mobile application using Amazon Cognito for credentials, see [Tutorial: Using AWS Lambda as Mobile Application Backend](https://docs.aws.amazon.com/lambda/latest/dg/with-android-example.html)\. 