# How to Use Built\-in Rules for Model Analysis<a name="use-debugger-built-in-rules"></a>

Amazon SageMaker Debugger rules analyze tensors emitted during the training of a model\. They monitor conditions that are critical for the success of a training job\. For example, they can detect whether gradients are getting too large or too small or if a model is being overfit\. Debugger comes pre\-packaged with certain Python\-coded, built\-in rules\. 

You can deploy a built\-in rule to monitor your training job either by using the [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) API or by using the open source [*smdebug* Python library](https://github.com/awslabs/sagemaker-debugger/) with the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\. The *smdebug* programming model provides the context for understanding this task\. For information on the programming model, see [Analysis](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/analysis.md)\.

**Note**  
You are not charged for the instances when running SageMaker built\-in rules

**Topics**
+ [Use the `CreateTrainingJob` API to Create a Built\-in Rule](debugger-built-in-rules-api.md)
+ [Use [*smdebug* Python library](https://github.com/awslabs/sagemaker-debugger/) with the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) to Create a Built\-in Rule](debugger-built-in-rules-python-sdk.md)