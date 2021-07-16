# Run Training and Inference Containers in Internet\-Free Mode<a name="mkt-algo-model-internet-free"></a>

SageMaker training and deployed inference containers are internet\-enabled by default\. This allows containers to access external services and resources on the public internet as part of your training and inference workloads\. However, this could provide an avenue for unauthorized access to your data\. For example, a malicious user or code that you accidentally install on the container \(in the form of a publicly available source code library\) could access your data and transfer it to a remote host\. 

If you use an Amazon VPC by specifying a value for the `VpcConfig` parameter when you call [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html), [ `CreateHyperParameterTuningJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateHyperParameterTuningJob.html), or [ `CreateModel`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html), you can protect your data and resources by managing security groups and restricting internet access from your VPC\. However, this comes at the cost of additional network configuration, and has the risk of configuring your network incorrectly\. If you do not want SageMaker to provide external network access to your training or inference containers, you can enable network isolation\.

## Network Isolation<a name="mkt-algo-model-internet-free-isolation"></a>

You can enable network isolation when you create your training job or model by setting the value of the `EnableNetworkIsolation` parameter to `True` when you call [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html), [ `CreateHyperParameterTuningJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateHyperParameterTuningJob.html), or [ `CreateModel`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html)\. 

**Note**  
Network isolation is required to run training jobs and models using resources from AWS Marketplace\.

If you enable network isolation, the containers can't make any outbound network calls, even to other AWS services such as Amazon S3\. Additionally, no AWS credentials are made available to the container runtime environment\. In the case of a training job with multiple instances, network inbound and outbound traffic is limited to the peers of each training container\. SageMaker still performs download and upload operations against Amazon S3 using your SageMaker execution role in isolation from the training or inference container\. 

The following managed SageMaker containers do not support network isolation because they require access to Amazon S3: 
+ Chainer
+ PyTorch
+ Scikit\-learn
+ SageMaker Reinforcement Learning

### Network isolation with a VPC<a name="mkt-algo-model-internet-free-isolation-marketplace"></a>

Network isolation can be used in conjunction with a VPC\. In this scenario, the download and upload of customer data and model artifacts are routed through your VPC subnet\. However, the training and inference containers themselves continue to be isolated from the network, and do not have access to any resource within your VPC or on the internet\. 