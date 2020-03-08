# How Debugger Works<a name="debugger-how-it-works"></a>

Amazon SageMaker Debugger enables you go beyond just looking at scalars like losses and accuracies when evaluating model training\. It gives you full visibility into a training job by using a hook to capture tensors that define the state of the training process at each instance in its lifecycle\. It also provides the capability of defining 'rules' to analyze the captured tensors\. Built\-in rules monitor the training flow and alert you to problems with various common conditions that are critical for the success of the training job\. You can also create your own custom rules to watch for any issues specific to your model\. You can monitor the results of the analysis done by rules with Amazon CloudWatch events, using an Amazon SageMaker notebook, or in visualization provided by Amazon SageMaker Studio\. Here is a depiction of the flow for the model training process with Amazon SageMaker Debugger \.

The following diagram shows the flow for the model training process with Debugger\.

![\[Overview of how Debugger works.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/how-debugger-works-4.png)

 Amazon SageMaker Debugger automatically debugs your machine learning training process\. It helps you develop better, faster, cheaper models by catching common errors quickly\. While training, it automatically saves tensors and the network state \(no script changes needed\)\. By constantly monitoring the training process, a separate job detects anomalies, such as a vanishing gradient, poor weight initialization, or other warning signals\. That way, if a training job fails, you’ll know what happened and how to fix it\. There are over 15 built\-in “rules,” or state assertions to ensure that training happens efficiently\.

 Amazon SageMaker Debugger supports all popular machine learning frameworks \(TensorFlow, PyTorch, and Apache MXNet\) and XGBoost\. It provides both an automated and a configurable experience\. In the automated experience, it provides custom framework forks in the deep learning containers that automatically detect your training job and save tensors, without any changes to your training script\. There is also an advanced configurable mode where you choose precisely which tensors to save, how to organize them, and which custom rules to use\.

The data can be saved to an S3 bucket so you can run your own analysis afterwards by creating a “trial\.” This provides full insight into the training process\. 

Amazon SageMaker Debugger does the following: 
+ Creates a “hook” object\. 
+ Passes this hook as a callback inside the training process\. 
+ The hook listens to various events, such as the forward and backward pass through the network\. Upon registering a “step,” or forward and backward pass, it writes tensors and other data to your S3 bucket\. 
+ The hook can also detect when your mode has switched between training and validation, letting you easily segment your data\. 
+ The tensors are first saved locally on the training instance and then moved to the S3 location you specified\. 
+ A separate instance runs the rule monitoring job which fetches tensors from the S3 location locally and invokes the rule logic on the tensors saved for the training job\. If something is amiss, it raises an exception and triggers an Amazon CloudWatch event\. You can run multiple rules simultaneously\.