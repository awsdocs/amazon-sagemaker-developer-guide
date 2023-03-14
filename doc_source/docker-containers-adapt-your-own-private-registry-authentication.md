# Use a Docker registry that requires authentication for training<a name="docker-containers-adapt-your-own-private-registry-authentication"></a>

If your Docker registry requires authentication, you must create an AWS Lambda function that provides access credentials to SageMaker\. Then, create a training job and provide the ARN of this Lambda function inside the [create\_training\_job](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_training_job) API\. Lastly, you can optionally create an interface VPC endpoint so that your VPC can communicate with your Lambda function without sending traffic over the internet\. The following guide shows how to create a Lambda function, assign it the correct role and create an interface VPC endpoint\.

## Create the Lambda function<a name="docker-containers-adapt-your-own-private-registry-authentication-create-lambda"></a>

Create an AWS Lambda function that passes access credentials to SageMaker and returns a response\. The following code example creates the Lambda function handler, as follows\.

```
def handler(event, context):
   response = {
      "Credentials": {"Username": "username", "Password": "password"}
   }
   return response
```

The type of authentication used to set up your private Docker registry determines the contents of the response returned by your Lambda function as follows\.
+ If your private Docker registry uses basic authentication, the Lambda function will return the username and password needed in order to authenticate to the registry\.
+ If your private Docker registry uses [bearer token authentication](https://docs.docker.com/registry/spec/auth/token/), the username and password are sent to your authorization server, which then returns a bearer token\. This token is then used to authenticate to your private Docker registry\.

**Note**  
If you have more than one Lambda functions for your registries in the same account, and the execution role is the same for your training jobs, then training jobs for registry one would have access to the Lambda functions for other registries\.

## Grant the correct role permission to your Lambda function<a name="docker-containers-adapt-your-own-private-registry-authentication-lambda-role"></a>

The [IAMrole](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html) that you use in the `create_training_job` API must have permission to call an AWS Lambda function\. The following code example shows how to extend permissions policy of an IAM role to call `myLambdaFunction`\.

```
{
    "Effect": "Allow",
    "Action": [
        "lambda:InvokeFunction"
    ],
    "Resource": [
        "arn:aws:lambda:*:*:function:*myLambdaFunction*"
    ]
}
```

For information about editing a role permissions policy, see [Modifying a role permissions policy \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-managingrole-editing-console.html#roles-modify_permissions-policy) in the *AWS Identity and Access Management User Guide*\.

**Note**  
An IAM role with an attached **AmazonSageMakerFullAccess** managed policy has permission to call any Lambda function with "SageMaker" in its name\.

## Create an interface VPC endpoint for Lambda<a name="docker-containers-adapt-your-own-private-registry-authentication-lambda-endpoint"></a>

If you create an interface endpoint, your Amazon VPC can communicate with your Lambda function without sending traffic over the internet\. For more information, see [Configuring interface VPC endpoints for Lambda](https://docs.aws.amazon.com/lambda/latest/dg/configuration-vpc-endpoints.html) in the *AWS Lambda Developer Guide*\.

After your interface endpoint is created, SageMaker training will call your Lambda function by sending a request through your VPC to `lambda.region.amazonaws.com`\. If you select **Enable DNS Name** when you create your interface endpoint, [Amazon Route 53](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/Welcome.html) routes the call to the Lambda interface endpoint\. If you use a different DNS provider, you must map `lambda.region.amazonaws.co`m, to your Lambda interface endpoint\.