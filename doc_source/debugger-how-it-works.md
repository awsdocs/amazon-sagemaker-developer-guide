# How Debugger Works<a name="debugger-how-it-works"></a>

With Amazon SageMaker Debugger, you can go beyond just looking at scalars, like losses and accuracies, when evaluating model training\. It gives you full visibility into a training job by using a hook** to capture tensors that define the state of the training process at each point in the job's lifecycle\. It also lets you define *rules* to analyze the captured tensors\. 

Built\-in rules monitor the training flow and alert you to problems with various common conditions that are critical for the success of the training job\. You can also create your own custom rules to watch for any issues specific to your model\. You can monitor the results of the analysis done by rules with Amazon CloudWatch events, using an Amazon SageMaker notebook, or in visualization provided by Amazon SageMaker Studio\.

The following diagram shows the flow for the model training process with Amazon SageMaker Debugger\.

![\[Overview of how Amazon SageMaker Debugger works.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/how-debugger-works-4.png)

 Amazon SageMaker Debugger automatically debugs your machine learning training jobs\. It helps you develop better, faster, cheaper models by quickly catching common errors\. During training, it automatically saves tensors and the network state \(no script changes needed\)\. By constantly monitoring the training process, it detects anomalies, such as a vanishing gradient, poor weight initialization, or other warning signals\. That way, if a training job fails, you’ll know what happened and how to fix it\. There are over 15 built\-in *rules*, or state assertions\. to ensure that training happens efficiently\.

 Amazon SageMaker Debugger supports all popular machine learning frameworks \(TensorFlow, PyTorch, and Apache MXNet\) and XGBoost\. It provides both an automated and a configurable mode\. In automated mode, it provides custom framework forks in the deep learning containers that automatically detect your training job and save tensors, without any changes to your training script\. In configurable mode, you can choose which tensors to save, how to organize them, and which custom rules to use\.

You save the data to an Amazon S3 bucket so you can run your own analysis later by creating a *trial*\. This provides full insight into the training process\. 

This is how it works: 

1. Amazon SageMaker Debugger creates a `hook` object\. 

1. Debugger passes the `hook` object as a callback inside the training process\. 

1. The hook listens to various events, such as the forward and backward pass through the network\. Upon registering a *step*, or forward and backward pass, it writes tensors and other data to your S3 bucket\. 

1. The hook can also detect when your mode has switched between training and validation, letting you easily segment your data\. 

1. Debugger first saves the tensors locally on the training EC2 instance, and then moves them to an Amazon S3 location that you specify\. 

1. A separate EC2 instance runs the rule monitoring job, which fetches tensors from the Amazon S3 location to your local machine and invokes the rule logic on the tensors saved for the training job\. If something is amiss, the rule monitoring job raises an exception and triggers an Amazon CloudWatch event\. You can run multiple rules simultaneously\.