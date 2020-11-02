# Protect Communications Between ML Compute Instances in a Distributed Training Job<a name="train-encrypt"></a>

By default, Amazon SageMaker runs training jobs in an Amazon Virtual Private Cloud \(Amazon VPC\) to help keep your data secure\. You can add another level of security to protect your training containers and data by configuring a *private* VPC\. Distributed ML frameworks and algorithms usually transmit information that is directly related to the model such as weights, not the training dataset\. When performing distributed training, you can further protect data that is transmitted between instances\. This can help you to comply with regulatory requirements\. To do this, use inter\-container traffic encryption\. 

Enabling inter\-container traffic encryption can increase training time, especially if you are using distributed deep learning algorithms\. Enabling inter\-container traffic encryption doesn't affect training jobs with a single compute instance\. However, for training jobs with several compute instances, the effect on training time depends on the amount of communication between compute instances\. For affected algorithms, adding this additional level of security also increases cost\. The training time for most SageMaker built\-in algorithms, such as XGBoost, DeepAR, and linear learner, typically aren't affected\.

You can enable inter\-container traffic encryption for training jobs or hyperparameter tuning jobs\. You can use SageMaker APIs or console to enable inter\-container traffic encryption\.

For information about running training jobs in a private VPC, see [Give SageMaker Training Jobs Access to Resources in Your Amazon VPC](train-vpc.md)\.

## Enable Inter\-Container Traffic Encryption \(API\)<a name="train-encrypt-api"></a>

Before enabling inter\-container traffic encryption on training or hyperparameter tuning jobs with APIs, you need to add inbound and outbound rules to your private VPC's security group\.

**To enable inter\-container traffic encryption \(API\)**

1.  Add the following inbound and outbound rules in the security group for your private VPC:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/train-encrypt.html)

1. When you send a request to the [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) or [ `CreateHyperParameterTuningJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateHyperParameterTuningJob.html) API, specify `True` for the `EnableInterContainerTrafficEncryption` parameter\.

**Note**  
The AWS Security Group Console might show display ports range as "All", however EC2 ignores the specified port range because it is not applicable for the ESP 50 IP protocol\.

## Enable Inter\-Container Traffic Encryption \(Console\)<a name="train-encrypt-console"></a>

### Enable Inter\-container Traffic Encryption in a Training Job<a name="train-encrypt-console-training"></a>

**To enable inter\-container traffic encryption in a training job**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. In the navigation pane, choose **Training**, then choose **Training jobs**\.

1. Choose **Create training job**\. 

1. Under **Network**, choose a **VPC**\. You can use the default VPC or one that you have created\. 

1. Choose **Enable inter\-container traffic encryption**\. 

After you enable inter\-container traffic encryption, finish creating the training job\. For more information, see [Step 5: Train a Model](ex1-train-model.md)\.

### Enable Inter\-container Traffic Encryption in a Hyperparameter Tuning Job<a name="train-encrypt-console-tuning"></a>

**To enable inter\-container traffic encryption in a hyperparameter tuning job**

1. Open the Amazon SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. In the navigation pane, choose **Training**, then choose **Hyperparameter tuning jobs**\.

1. Choose **Create hyperparameter tuning job**\. 

1. Under **Network**, choose a **VPC**\. You can use the default VPC or one that you created\. 

1. Choose **Enable inter\-container traffic encryption**\. 

After enabling inter\-container traffic encryption, finish creating the hyperparameter tuning job\. For more information, see [Configure and Launch a Hyperparameter Tuning Job](automatic-model-tuning-ex-tuning-job.md)\.