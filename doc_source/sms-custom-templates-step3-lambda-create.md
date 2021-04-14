# Create Lambda Functions for a Custom Labeling Workflow<a name="sms-custom-templates-step3-lambda-create"></a>

You can create a Lambda function using the Lambda console, the AWS CLI, or an AWS SDK in a supported programming language of your choice\. Use the AWS Lambda Developer Guide to learn more about each of these options:
+ To learn how to create a Lambda function using the console, see [Create a Lambda function with the console](https://docs.aws.amazon.com/lambda/latest/dg/getting-started-create-function.html)\.
+ To learn how to create a Lambda function using the AWS CLI, see [Using AWS Lambda with the AWS Command Line Interface](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-awscli.html)\.
+ Select the relevant section in the table of contents to learn more about working with Lambda in the language of your choice\. For example, select [Working with Python](https://docs.aws.amazon.com/lambda/latest/dg/lambda-python.html) to learn more about using Lambda with the AWS SDK for Python \(Boto3\)\.

Ground Truth provides pre\-annotation and post\-annotation templates through an AWS Serverless Application Repository \(SAR\) *recipe*\. Use the following procedure to select the Ground Truth recipe in the Lambda console\.

**Use the Ground Truth SAR recipe to create pre\-annotation and post\-annotation Lambda functions:**

1. Open the [**Functions** page](https://console.aws.amazon.com/lambda/home#/functions) on the Lambda console\.

1. Select **Create function**\.

1. Select **Browse serverless app repository**\.

1. In the search text box, enter **aws\-sagemaker\-ground\-truth\-recipe** and select that app\.

1. Select **Deploy**\. The app may take a couple of minutes to deploy\. 

   Once the app deploys, two functions appear in the **Functions** section of the Lambda console: `serverlessrepo-aws-sagema-GtRecipePreHumanTaskFunc-<id>` and `serverlessrepo-aws-sagema-GtRecipeAnnotationConsol-<id>`\. 

1. Select one of these functions and add your custom logic in the **Code** section\.

1. When you are finished making changes, select **Deploy** to deploy them\.