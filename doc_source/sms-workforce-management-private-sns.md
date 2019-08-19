# Create and manage Amazon SNS topics for your work teams<a name="sms-workforce-management-private-sns"></a>

## Create the Amazon SNS topic<a name="workteam-private-sns-create-topic"></a>

These instructions are needed when you:
+ Create a topic to which you will subscribe an existing work team\.
+ Create a topic prior to creating a work team\.
+ Create or modify the work team with an API call and need to specify a topic ARN\.

If you create a work team using the console, that process provides an option to create a new topic for the team, and if used, handles these steps for you\.

The creation of Amazon SNS topics for work team notification is similar to the steps in the [Amazon SNS Getting Started](https://docs.aws.amazon.com/sns/latest/dg/sns-getting-started.html) document, with one significant addition: adding an access policy so the Sagemaker service can publish messages to the topic on your behalf\.

**Add the policy while creating the topic**

1. In the **Access policy** panel, select the advanced method\.

1. In the **JSON editor**, find the `Resource` property, which displays what the ARN of the topic will be when it is created\. If the value ends in "undefined," change "undefined" to the name of the topic\.

1. Copy the `Resource` value\.

1. Before the final closing bracket \(`]`\), add the following policy\.

   ```
       {
         "Sid": "AWSSagemaker_AccessPolicy",
         "Effect": "Allow",
         "Principal": {
           "Service": "sagemaker.amazonaws.com"
         },
         "Action": "sns:Publish",
         "Resource": "resource ARN you copied in the previous step"
       }
   ```

1. Create the topic\.

For more details on creating topics, see the Amazon SNS ["Creating a Topic"](https://docs.aws.amazon.com/sns/latest/dg/sns-tutorial-create-topic.html) tutorial\.

## Manage worker subscriptions<a name="workteam-private-sns-manage-topic"></a>

Workers are auto\-subscribed to your topic only when a Amazon Cognito user group is created or imported during the creation of a work team that is subscribed to the topic at creation\.

If a work team is subscribed to a topic after creation, the individual members at the time of creation are not automatically subscribed\. See ["Subscribing an Endpoint to an Amazon SNS Topic"](https://docs.aws.amazon.com/sns/latest/dg/sns-tutorial-create-subscribe-endpoint-to-topic.html) for information on subscribing the workers' email addresses to the topic\.