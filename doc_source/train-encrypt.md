# Protect Communications Between ML Compute Instances in a Distributed Training Job<a name="train-encrypt"></a>

By default, Amazon SageMaker runs training jobs in an Amazon Virtual Private Cloud \(Amazon VPC\) to help keep your data secure\. You can add another level of security to protect your training containers and data by configuring a *private* VPC\. Distributed ML frameworks and algorithms usually transmit information that is directly related to the model such as weights, not the training dataset\. When performing distributed training, you can further protect data that is transmitted between instances\. This can help you to comply with regulatory requirements\. To do this, use inter\-container traffic encryption\. For more information about running training jobs in a private VPC, see [Protect Training Jobs by Using an Amazon Virtual Private Cloud](train-vpc.md)\.

 

Enabling inter\-container traffic encryption can increase training time, especially if you are using distributed deep learning algorithms\. Enabling inter\-container traffic encryption doesn't affect training jobs with a single compute instance\. However, for training jobs with several compute instances, the effect on training time depends on the amount of communication between compute instances\. For affected algorithms, adding this additional level of security also increases cost\. The training time for most Amazon SageMaker built\-in algorithms, such as XGBoost, DeepAR, and linear learner, typically aren't affected\.

## Using Inter\-Container Traffic Encryption<a name="train-encrypt-api"></a>

  To enable inter\-container traffic encryption:  

1.  Add the following inbound and outbound rules in the security group for your private VPC:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/train-encrypt.html)

1. When you send a request to the [CreateTrainingJob](API_CreateTrainingJob.md) API, specify `True` for the `EnableInterContainerTrafficEncryption` parameter\. You can set the `EnableInterContainerTrafficEncryption` parameter only when configuring a private VPC\.