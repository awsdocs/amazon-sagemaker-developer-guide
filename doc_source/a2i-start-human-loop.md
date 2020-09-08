# Create and Start a Human Loop<a name="a2i-start-human-loop"></a>

A *human loop* starts your human review workflow and sends data review tasks to human workers\. When you use one of the Amazon A2I built\-in task types, the corresponding AWS service creates and starts a human loop on your behalf when the conditions specified in your flow definition are met\. If no conditions were specified in your flow definition, a human loop is created for each object\. When using Amazon A2I for a custom task, a human loops starts when `StartHumanLoop` is called in your application\. 

Use the following instructions to configure a human loop with Amazon Rekognition or Amazon Textract built\-in task types and custom task types\. 

**Prerequisites**

To create and start a human loop, the AmazonAugmentedAIFullAccess policy must be attached to the AWS Identity and Access Management \(IAM\) user or role that configures or starts the human loop\. This will be the identity that you use to configure the human loop using `HumanLoopConfig` for built\-in task types\. For custom task types, this will be the identity that you use to call `StartHumanLoop`\.

Additionally, when using a built\-in task type, your IAM user or role must have permission to invoke API operations of the AWS service associated with your task type\. For example, if using Amazon Rekognition with Augmented AI, you must attach permissions required to call `DetectModerationLabels`\. For examples of identity\-based policies you can use to grant these permissions, see [Amazon Rekognition Identity\-Based Policy Examples](https://docs.aws.amazon.com/rekognition/latest/dg/security_iam_id-based-policy-examples.html) and [Amazon Textract Identity\-Based Policy Examples](https://docs.aws.amazon.com/textract/latest/dg/security_iam_id-based-policy-examples.html)\. You can also use the more general policy **AmazonAugmentedAIIntegratedAPIAccess** to grant these permissions\. For more information, see [Create an IAM User With Permissions to Invoke Amazon A2I, Amazon Textract, and Amazon Rekognition API Operations](a2i-permissions-security.md#a2i-grant-general-permission)\. 

To create and start a human loop, you will need a flow definition ARN\. To learn how to create a flow definition \(or human review workflow\), see [Create a Flow Definition](a2i-create-flow-definition.md)\.

## Create and Start a Human Loop for a Built\-in Task Type<a name="a2i-human-loop-built-in-task-type"></a>

To start a human loop for a built\-in task type jobs use the corresponding service's API to provide your input data and to configure the human loop\. For Amazon Textract, you use the `AnalyzeDocument` API operation\. For Amazon Rekognition, you use the `DetectModerationLabels` API operation\. You can use the AWS CLI, or a language\-specific SDK to create requests using these API operations\. 

After you start your ML job using your built\-in task type's AWS service API, Amazon A2I monitors the inference\-results of that service\. For example, when running a job with Amazon Rekognition, Amazon A2I checks the inference confidence score for each image and compares it to the confidence\-thresholds specified in your flow definition\. If the conditions to start a human review task are satisfied, or if you didn't specify conditions in your flow definition, a human review task is sent to workers\. 

### Create an Amazon Textract Human Loop<a name="a2i-human-loop-textract"></a>

Amazon A2I integrates with Amazon Textract so that you can configure and start a human loop using the Amazon Textract API\. To send a document file to Amazon Textract for text analysis, you use the Amazon Textract [`AnalyzeDocument` API operation](https://docs.aws.amazon.com/textract/latest/dg/API_AnalyzeDocument.html)\. To configure a human loop, set the `HumanLoopConfig` parameter when you configure `AnalyzeDocument`\. To learn how, see Step 3 in [Running AnalyzeDocument with Amazon A2I \(Preview\)](https://docs.aws.amazon.com/textract/latest/dg/a2i-textract.html#procedure-a2i-document) in the *Amazon Textract Developer Guide*\. 

After you run the `AnalyzeDocument` with a human loop configured, Amazon A2I monitors the results from `AnalyzeDocument` and checks it against the flow definition's activation conditions\. If Amazon Textract inference confidence score for one or more key value pairs meets the conditions for review, Amazon A2I starts a human review loop and includes the [https://docs.aws.amazon.com/textract/latest/dg/API_HumanLoopActivationOutput.html](https://docs.aws.amazon.com/textract/latest/dg/API_HumanLoopActivationOutput.html) object in the `AnalyzeDocument` response\.

### Create an Amazon Rekognition Human Loop<a name="a2i-human-loop-rekognition"></a>

Amazon A2I integrates with Amazon Rekognition so that you can configure and start a human loop using the Amazon Rekognition API\. To send images to Amazon Rekognition for content moderation, you use the Amazon Rekognition [`DetectModerationLabels` API operation](https://docs.aws.amazon.com/rekognition/latest/dg/API_DetectModerationLabels.html)\. To configure a human loop, set the `HumanLoopConfig` parameter when you configure `DetectModerationLabels`\. To learn how, see Step 3 in [Running DetectModerationLabels with Amazon A2I](https://docs.aws.amazon.com/rekognition/latest/dg/a2i-rekognition.html#procedure-a2i-moderation) in the *Amazon Rekognition Developer Guide*\. 

After you run the `DetectModerationLabels` with a human loop configured, Amazon A2I monitors the results from `DetectModerationLabels` and checks it against the flow definition's activation conditions\. If the Amazon Rekognition inference confidence score for an image meets the conditions for review, Amazon A2I starts a human review loop and includes the response element `HumanLoopActivationOutput` response object in the `DetectModerationLabels` response\.

## Create and Start a Human Loop for a Custom Task Type<a name="a2i-instructions-starthumanloop"></a>

To configure a human loop for a custom human review task, use the `StartHumanLoop` operation within your application\. This section provides an example of a human loop request using the AWS SDK for Python \(Boto3\) and the AWS Command Line Interface \(AWS CLI\)\. For documentation on other language specific SDK's that support `StartHumanLoop`, use the **See Also** section of [StartHumanLoop](https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_StartHumanLoop.html) in the Amazon Augmented AI Runtime API documentation\. 

**Prerequisites**

To complete this procedure, you need:
+ Input data formatted as a string representation of a JSON\-formatted file\.
+ The Amazon Resource Name \(ARN\) of your flow definition
+ A flow definition ARN\. 

**To configure the human loop**

1. For `DataAttributes`, specify a set of `ContentClassifiers` related to the input provided to the `StartHumanLoop` operation\. Use content classifiers to decalre that your content is free of personally identifiable information or adult content\. 

   To use Amazon Mechanical Turk, ensure your data is free of Personally Identifiable Information and include the `FreeOfPersonallyIdentifiableInformation` content classifier\. If you data is free of adult content, also include the `'FreeOfAdultContent'` classifier\. If you do not use these content classifiers, SageMaker may restrict the Mechanical Turk workers that can view your task\.

1. For `FlowDefinitionArn`, enter the Amazon Resource Name \(ARN\) of your flow definition\.

1. For `HumanLoopInput`, enter your input data as a string representation of a JSON\-formatted file\. Structure your input data and custom worker task template so that your input data is properly displayed to human workers when you start your human loop\. See to learn how to preview your custom worker task template\. 

1. For `HumanLoopName`, enter a name for the human loop\. The name must be unique within the Region in your account, and can have up to 63 characters\. Valid characters: a\-z, 0\-9, and \- \(hyphen\)\.

**To start a human loop**
+ To start a human loop, submit a request similar to the following examples using your preferred language specific SDK\. 

------
#### [ AWS SDK for Python \(Boto3\) ]

The following request example uses the SDK for Python \(Boto3\)\. For more information, see [Boto 3 Augmented AI Runtime](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker-a2i-runtime.html#AugmentedAIRuntime.Client.start_human_loop) in the *AWS SDK for Python \(Boto\) API Reference*\.

```
response = client.start_human_loop(
    HumanLoopName='string',
    FlowDefinitionArn='string',
    HumanLoopInput={
        'InputContent': 'string'
    },
    DataAttributes={
        'ContentClassifiers': [
            'FreeOfPersonallyIdentifiableInformation'|'FreeOfAdultContent',
        ]
    }
)
```

------
#### [ AWS CLI ]

The following request example uses the AWS CLI\. For more information, see [start\-human\-loop](https://docs.aws.amazon.com/cli/latest/reference/sagemaker-a2i-runtime/start-human-loop.html) in the *[AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/)*\. 

```
$ InputContent=string
ContentClassifiers="FreeOfPersonallyIdentifiableInformation","FreeOfAdultContent" 
start-human-loop --human-loop-name example-humanloop 
                 --flow-definition-arn arn:aws:sagemaker:us-west-2:111111111111:flow-definition/flowdef-nov-12  
                 --human-loop-input InputContent 
                 --data-attributes ContentClassifiers
```

------

When you successfully start a human loop by invoking `StartHumanLoop` directly, the response will include a `HumanLoopARN` and a `HumanLoopActivationResults` object which will be set to `NULL`\. You can use this the human loop name to monitor and manage your human loop\.

## Next Steps:<a name="a2i-next-step-starthumanloop"></a>

After starting a human loop, you can manage and monitor it with the Amazon Augmented AI Runtime API and Amazon CloudWatch Events\. To learn more, see [Monitor and Manage Your Human Loop](a2i-monitor-humanloop-results.md)\.