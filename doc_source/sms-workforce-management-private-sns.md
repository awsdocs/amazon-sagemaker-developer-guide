# Create and manage Amazon SNS topics for your work teams<a name="sms-workforce-management-private-sns"></a>

Use the procedures in this topic when you want to:
+ Create a topic to which you want an existing work team to subscribe\.
+ Create a topic before you've created a work team\.
+ Create or modify the work team with an API call, and specify a topic Amazon Resource Name \(ARN\)\.

If you create a work team using the console, the console provides an option to create a new topic for the team so that you don't have to perform these steps\.

**Important**  
The Amazon SNS feature is not supported by Amazon A2I\. If you subscribe your work team to an Amazon SNS topic, workers will only receive notifications about Ground Truth labeling jobs\. Workers will not receive notifications about new Amazon A2I human review tasks\.

## Create the Amazon SNS topic<a name="workteam-private-sns-create-topic"></a>

The steps for creating Amazon SNS topics for work team notifications are similar to the steps in [Getting Started](https://docs.aws.amazon.com/sns/latest/dg/sns-getting-started.html) in the *Amazon SNS Developer Guide*, with one significant addition—you must add an access policy so that Amazon SageMaker can publish messages to the topic on your behalf\.

**To add the policy when you create the topic**

1. Open the Amazon SNS console at [https://console.aws.amazon.com/sns/](https://console.aws.amazon.com/sns/)\.

1. In **Create topic**, enter the name of your topic and then choose **Next steps**\.

1. In **Access policy**, choose **Advanced**\.

1. In the **JSON editor**, find the `Resource` property, which displays the topic's ARN\.

1. Copy the `Resource` ARN value\.

1. Before the final closing brace \(`]`\), add the following policy\.

   ```
       , {
         "Sid": "AwsSagemaker_SnsAccessPolicy",
         "Effect": "Allow",
         "Principal": {
           "Service": "sagemaker.amazonaws.com"
         },
         "Action": "sns:Publish",
         "Resource": "ARN of the topic you copied in the previous step"
       }
   ```

1. Create the topic\.

After you create the topic, it appears in your **Topics** summary screen\. For more information about creating topics, see [Creating a Topic](https://docs.aws.amazon.com/sns/latest/dg/sns-tutorial-create-topic.html) in the *Amazon SNS Developer Guide*\.

## Manage worker subscriptions<a name="workteam-private-sns-manage-topic"></a>

If you subscribe a work team to a topic after you've already created the work team, the individual work team members who were added to the team when the work team was created are not automatically subscribed to the topic\. For information about subscribing workers' email addresses to the topic, see [Subscribing an Endpoint to an Amazon SNS Topic](https://docs.aws.amazon.com/sns/latest/dg/sns-tutorial-create-subscribe-endpoint-to-topic.html) in the *Amazon SNS Developer Guide*\.

The only situation in which workers are automatically subscribed to your topic is when you create or import an Amazon Cognito user group at the time that you create a work team ***and*** you set up the topic subscription when you create that work team\. For more information about creating and managing your workteams with Amazon Cognito, see [Create Work Teams \(Amazon Cognito Console\)](sms-workforce-management-private-cognito.md#create-work-teams-cog)\.