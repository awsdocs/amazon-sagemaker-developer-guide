# How to Use Custom Rules<a name="debugger-custom-rules"></a>

You can deploy a custom rule to monitor your training job either by using the [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) API or by using the open source [*smdebug* Python library](https://github.com/awslabs/sagemaker-debugger/) with the Amazon SageMaker Python SDK\. The *smdebug* programming model provides the context for understanding this task\. For information on the programming model, see [Analysis](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/analysis.md)\.

To run a custom rule, you have to provide a few additional parameters for the interface\. Key parameters are the Python file that has the implementation of your `Rule` class, the name of the `Rule` class, the type of instance to run the `Rule` job on, the size of the volume on that instance, and the docker image to use for running this job\. 

**Topics**
+ [Use the `CreateTrainingJob` API to Create a Custom Rule](debugger-custom-rules-api.md)
+ [Use [*smdebug* Python library](https://github.com/awslabs/sagemaker-debugger/) with the Amazon SageMaker Python SDK to Create a Custom Rule](debugger-custom-rules-python-sdk.md)