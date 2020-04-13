# Use Amazon Augmented AI with Amazon Rekognition<a name="a2i-rekognition-task-type"></a>


****  

|  | 
| --- |
|  Amazon Augmented AI is in preview release and is subject to change\. We do not recommend using this product in production environments\. | 

Amazon Rekognition makes it easy to add image analysis to your applications\. The Amazon Rekognition `DetectModerationLabels`API operation is directly integrated with Amazon A2I so that you can easily create a human loop to review unsafe images, such as explicit adult or violent content\. You can use `DetectModerationLabels` to configure a human loop using a flow definition ARN\. This enables Amazon A2I to send low\-confidence predictions from Amazon Rekognition to a human for review\. 

## A2I Rekognition Worker Console Preview<a name="a2i-rekognition-console-preview"></a>

When they're assigned a review task in an Amazon Rekognition workflow, workers might see UI similar to the following:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/a2i-rekognition-example.png)

You can customize this interface in the Amazon SageMaker console when you create your human review definition, or by creating and using a custom template\. To learn more, see [Create a Worker UI](a2i-instructions-overview.md)\.

## Integrating a Human Review into Amazon Rekognition<a name="a2i-create-rekognition-human-review"></a>

To integrate a human review into an Amazon Rekognition, see the following topics:
+ [Create a Flow Definition \(Console\)](a2i-create-flow-definition.md#create-human-review-console)
+ [Create a Flow Definition \(API\)](a2i-create-flow-definition.md#create-human-review-api)

After you've created your flow definition, see [Using Augmented AI with Amazon Rekognition](https://docs.aws.amazon.com/rekognition/latest/dg/a2i-rekognition.html) to learn how to integrate your flow definition into your Amazon Rekognition task\. 