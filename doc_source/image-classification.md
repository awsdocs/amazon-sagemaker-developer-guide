# Image Classification Algorithm<a name="image-classification"></a>

The Amazon SageMaker image classification algorithm is a supervised learning algorithm that takes an image as input and classifies it into one of multiple output categories\. It uses a convolutional neural network \(ResNet\) that can be trained from scratch, or trained using transfer learning when a large number of training images are not available\. 

The recommended input format for the Amazon SageMaker image classification algorithms is Apache MXNet [RecordIO](https://mxnet.incubator.apache.org/architecture/note_data_loading.html)\. However, you can also use raw images in \.jpg or \.png format\.

**Note**  
To maintain better interoperability with existing deep learning frameworks, this differs from the protobuf data formats commonly used by other Amazon SageMaker algorithms\.

For more information on convolutional networks, see: 
+ [Deep residual learning for image recognition](https://arxiv.org/abs/1512.03385) Kaiming He, et al\., 2016 IEEE Conference on Computer Vision and Pattern Recognition
+ [ImageNet image database](http://www.image-net.org/)
+ [Image classification in MXNet](https://github.com/apache/incubator-mxnet/tree/master/example/image-classification)

**Topics**
+ [Input/Output Interface for the Image Classification Algorithm](#IC-inputoutput)
+ [EC2 Instance Recommendation for the Image Classification Algorithm](#IC-instances)
+ [Image Classification Sample Notebooks](#IC-sample-notebooks)
+ [How Image Classification Works](IC-HowItWorks.md)
+ [Image Classification Hyperparameters](IC-Hyperparameter.md)
+ [Tune an Image Classification Model](IC-tuning.md)

## Input/Output Interface for the Image Classification Algorithm<a name="IC-inputoutput"></a>

The Amazon SageMaker Image Classification algorithm supports both RecordIO \(`application/x-recordio`\) and image \(`image/png`, `image/jpeg`, and `application/x-image`\) content types for training in file mode and supports RecordIO \(`application/x-recordio`\) content type for training in pipe mode\. However you can also train in pipe mode using the image files \(`image/png`, `image/jpeg`, and `application/x-image`\), without creating RecordIO files, by using the augmented manifest format\. The algorithm supports `image/png`, `image/jpeg`, and `application/x-image` for inference\.

### Train with RecordIO Format<a name="IC-recordio-training"></a>

If you use the RecordIO format for training, specify both `train` and `validation` channels as values for the `InputDataConfig` parameter of the [CreateTrainingJob](API_CreateTrainingJob.md) request\. Specify one RecordIO \(`.rec`\) file in the `train` channel and one RecordIO file in the `validation` channel\. Set the content type for both channels to `application/x-recordio`\. 

### Train with Image Format<a name="IC-image-training"></a>

If you use the Image format for training, specify `train`, `validation`, `train_lst`, and `validation_lst` channels as values for the `InputDataConfig` parameter of the [CreateTrainingJob](API_CreateTrainingJob.md) request\. Specify the individual image data \(`.jpg` or `.png` files\) for the `train` and `validation` channels\. Specify one `.lst` file in each of the `train_lst` and `validation_lst` channels\. Set the content type for all four channels to `application/x-image`\. 

A `.lst` file is a tab\-separated file with three columns that contains a list of image files\. The first column specifies the image index, the second column specifies the class label index for the image, and the third column specifies the relative path of the image file\. The image index in the first column must be unique across all of the images\. The set of class label indices are numbered successively and the numbering should start with 0\. For example, 0 for the cat class, 1 for the dog class, and so on for additional classes\. 

 The following is an example of a `.lst` file: 

```
5      1   your_image_directory/train_img_dog1.jpg
1000   0   your_image_directory/train_img_cat1.jpg
22     1   your_image_directory/train_img_dog2.jpg
```

For example, if your training images are stored in `s3://<your_bucket>/train/class_dog`, `s3://<your_bucket>/train/class_cat`, and so on, specify the path for your `train` channel as `s3://<your_bucket>/train`, which is the top\-level directory for your data\. In the `.lst` file, specify the relative path for an individual file named `train_image_dog1.jpg` in the `class_dog` class directory as `class_dog/train_image_dog1.jpg`\. You can also store all your image files under one subdirectory inside the `train` directory\. In that case, use that subdirectory for the relative path\. For example, `s3://<your_bucket>/train/your_image_directory`\. 

### Train with Augmented Manifest Image Format<a name="IC-augmented-manifest-training"></a>

The augmented manifest format enables you to do training in Pipe mode using image files without needing to create RecordIO files\. You need to specify both train and validation channels as values for the `InputDataConfig` parameter of the [ CreateTrainingJob Service: Amazon SageMaker Service APIrequestsCreateTrainingJob  Starts a model training job\. After training completes, Amazon SageMaker saves the resulting model artifacts to an Amazon S3 location that you specify\.  If you choose to host your model using Amazon SageMaker hosting services, you can use the resulting model artifacts as part of the model\. You can also use the artifacts in a machine learning service other than Amazon SageMaker, provided that you know how to use them for inferences\.  In the request body, you provide the following:     `AlgorithmSpecification` \- Identifies the training algorithm to use\.     `HyperParameters` \- Specify these algorithm\-specific parameters to influence the quality of the final model\. For a list of hyperparameters for each training algorithm provided by Amazon SageMaker, see [Algorithms](https://docs.aws.amazon.com/sagemaker/latest/dg/algos.html)\.     `InputDataConfig` \- Describes the training dataset and the Amazon S3 location where it is stored\.    `OutputDataConfig` \- Identifies the Amazon S3 location where you want Amazon SageMaker to save the results of model training\.      `ResourceConfig` \- Identifies the resources, ML compute instances, and ML storage volumes to deploy for model training\. In distributed training, you specify more than one instance\.     `RoleARN` \- The Amazon Resource Number \(ARN\) that Amazon SageMaker assumes to perform tasks on your behalf during model training\. You must grant this role the necessary permissions so that Amazon SageMaker can successfully complete model training\.     `StoppingCondition` \- Sets a duration for training\. Use this parameter to cap model training costs\.     For more information about Amazon SageMaker, see [How It Works](https://docs.aws.amazon.com/sagemaker/latest/dg/how-it-works.html)\.   Request Syntax  

```
{
   "[AlgorithmSpecification](#SageMaker-CreateTrainingJob-request-AlgorithmSpecification)": { 
      "[AlgorithmName](API_AlgorithmSpecification.md#SageMaker-Type-AlgorithmSpecification-AlgorithmName)": "string",
      "[MetricDefinitions](API_AlgorithmSpecification.md#SageMaker-Type-AlgorithmSpecification-MetricDefinitions)": [ 
         { 
            "[Name](API_MetricDefinition.md#SageMaker-Type-MetricDefinition-Name)": "string",
            "[Regex](API_MetricDefinition.md#SageMaker-Type-MetricDefinition-Regex)": "string"
         }
      ],
      "[TrainingImage](API_AlgorithmSpecification.md#SageMaker-Type-AlgorithmSpecification-TrainingImage)": "string",
      "[TrainingInputMode](API_AlgorithmSpecification.md#SageMaker-Type-AlgorithmSpecification-TrainingInputMode)": "string"
   },
   "[EnableInterContainerTrafficEncryption](#SageMaker-CreateTrainingJob-request-EnableInterContainerTrafficEncryption)": boolean,
   "[EnableNetworkIsolation](#SageMaker-CreateTrainingJob-request-EnableNetworkIsolation)": boolean,
   "[HyperParameters](#SageMaker-CreateTrainingJob-request-HyperParameters)": { 
      "string" : "string" 
   },
   "[InputDataConfig](#SageMaker-CreateTrainingJob-request-InputDataConfig)": [ 
      { 
         "[ChannelName](API_Channel.md#SageMaker-Type-Channel-ChannelName)": "string",
         "[CompressionType](API_Channel.md#SageMaker-Type-Channel-CompressionType)": "string",
         "[ContentType](API_Channel.md#SageMaker-Type-Channel-ContentType)": "string",
         "[DataSource](API_Channel.md#SageMaker-Type-Channel-DataSource)": { 
            "[S3DataSource](API_DataSource.md#SageMaker-Type-DataSource-S3DataSource)": { 
               "[AttributeNames](API_S3DataSource.md#SageMaker-Type-S3DataSource-AttributeNames)": [ "string" ],
               "[S3DataDistributionType](API_S3DataSource.md#SageMaker-Type-S3DataSource-S3DataDistributionType)": "string",
               "[S3DataType](API_S3DataSource.md#SageMaker-Type-S3DataSource-S3DataType)": "string",
               "[S3Uri](API_S3DataSource.md#SageMaker-Type-S3DataSource-S3Uri)": "string"
            }
         },
         "[InputMode](API_Channel.md#SageMaker-Type-Channel-InputMode)": "string",
         "[RecordWrapperType](API_Channel.md#SageMaker-Type-Channel-RecordWrapperType)": "string",
         "[ShuffleConfig](API_Channel.md#SageMaker-Type-Channel-ShuffleConfig)": { 
            "[Seed](API_ShuffleConfig.md#SageMaker-Type-ShuffleConfig-Seed)": number
         }
      }
   ],
   "[OutputDataConfig](#SageMaker-CreateTrainingJob-request-OutputDataConfig)": { 
      "[KmsKeyId](API_OutputDataConfig.md#SageMaker-Type-OutputDataConfig-KmsKeyId)": "string",
      "[S3OutputPath](API_OutputDataConfig.md#SageMaker-Type-OutputDataConfig-S3OutputPath)": "string"
   },
   "[ResourceConfig](#SageMaker-CreateTrainingJob-request-ResourceConfig)": { 
      "[InstanceCount](API_ResourceConfig.md#SageMaker-Type-ResourceConfig-InstanceCount)": number,
      "[InstanceType](API_ResourceConfig.md#SageMaker-Type-ResourceConfig-InstanceType)": "string",
      "[VolumeKmsKeyId](API_ResourceConfig.md#SageMaker-Type-ResourceConfig-VolumeKmsKeyId)": "string",
      "[VolumeSizeInGB](API_ResourceConfig.md#SageMaker-Type-ResourceConfig-VolumeSizeInGB)": number
   },
   "[RoleArn](#SageMaker-CreateTrainingJob-request-RoleArn)": "string",
   "[StoppingCondition](#SageMaker-CreateTrainingJob-request-StoppingCondition)": { 
      "[MaxRuntimeInSeconds](API_StoppingCondition.md#SageMaker-Type-StoppingCondition-MaxRuntimeInSeconds)": number
   },
   "[Tags](#SageMaker-CreateTrainingJob-request-Tags)": [ 
      { 
         "[Key](API_Tag.md#SageMaker-Type-Tag-Key)": "string",
         "[Value](API_Tag.md#SageMaker-Type-Tag-Value)": "string"
      }
   ],
   "[TrainingJobName](#SageMaker-CreateTrainingJob-request-TrainingJobName)": "string",
   "[VpcConfig](#SageMaker-CreateTrainingJob-request-VpcConfig)": { 
      "[SecurityGroupIds](API_VpcConfig.md#SageMaker-Type-VpcConfig-SecurityGroupIds)": [ "string" ],
      "[Subnets](API_VpcConfig.md#SageMaker-Type-VpcConfig-Subnets)": [ "string" ]
   }
}
```   Request Parameters  For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\. The request accepts the following data in JSON format\.  

 ** [AlgorithmSpecification](#API_CreateTrainingJob_RequestSyntax) **   <a name="SageMaker-CreateTrainingJob-request-AlgorithmSpecification"></a>
The registry path of the Docker image that contains the training algorithm and algorithm\-specific metadata, including the input mode\. For more information about algorithms provided by Amazon SageMaker, see [Algorithms](https://docs.aws.amazon.com/sagemaker/latest/dg/algos.html)\. For information about providing your own algorithms, see [Using Your Own Algorithms with Amazon SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/your-algorithms.html)\.   
Type: [AlgorithmSpecification](API_AlgorithmSpecification.md) object  
Required: Yes 

 ** [EnableInterContainerTrafficEncryption](#API_CreateTrainingJob_RequestSyntax) **   <a name="SageMaker-CreateTrainingJob-request-EnableInterContainerTrafficEncryption"></a>
To encrypt all communications between ML compute instances in distributed training, choose `True`\. Encryption provides greater security for distributed training, but training might take longer\. How long it takes depends on the amount of communication between compute instances, especially if you use a deep learning algorithm in distributed training\. For more information, see [Protect Communications Between ML Compute Instances in a Distributed Training Job](https://docs.aws.amazon.com/sagemaker/latest/dg/train-encrypt.html)\.  
Type: Boolean  
Required: No 

 ** [EnableNetworkIsolation](#API_CreateTrainingJob_RequestSyntax) **   <a name="SageMaker-CreateTrainingJob-request-EnableNetworkIsolation"></a>
Isolates the training container\. No inbound or outbound network calls can be made, except for calls between peers within a training cluster for distributed training\. If you enable network isolation for training jobs that are configured to use a VPC, Amazon SageMaker downloads and uploads customer data and model artifacts through the specified VPC, but the training container does not have network access\.  
The Semantic Segmentation built\-in algorithm does not support network isolation\.
Type: Boolean  
Required: No 

 ** [HyperParameters](#API_CreateTrainingJob_RequestSyntax) **   <a name="SageMaker-CreateTrainingJob-request-HyperParameters"></a>
Algorithm\-specific parameters that influence the quality of the model\. You set hyperparameters before you start the learning process\. For a list of hyperparameters for each training algorithm provided by Amazon SageMaker, see [Algorithms](https://docs.aws.amazon.com/sagemaker/latest/dg/algos.html)\.   
You can specify a maximum of 100 hyperparameters\. Each hyperparameter is a key\-value pair\. Each key and value is limited to 256 characters, as specified by the `Length Constraint`\.   
Type: String to string map  
Key Length Constraints: Maximum length of 256\.  
Value Length Constraints: Maximum length of 256\.  
Required: No 

 ** [InputDataConfig](#API_CreateTrainingJob_RequestSyntax) **   <a name="SageMaker-CreateTrainingJob-request-InputDataConfig"></a>
An array of `Channel` objects\. Each channel is a named input source\. `InputDataConfig` describes the input data and its location\.   
Algorithms can accept input data from one or more channels\. For example, an algorithm might have two channels of input data, `training_data` and `validation_data`\. The configuration for each channel provides the S3 location where the input data is stored\. It also provides information about the stored data: the MIME type, compression method, and whether the data is wrapped in RecordIO format\.   
Depending on the input mode that the algorithm supports, Amazon SageMaker either copies input data files from an S3 bucket to a local directory in the Docker container, or makes it available as input streams\.   
Type: Array of [Channel](API_Channel.md) objects  
Array Members: Minimum number of 1 item\. Maximum number of 8 items\.  
Required: No 

 ** [OutputDataConfig](#API_CreateTrainingJob_RequestSyntax) **   <a name="SageMaker-CreateTrainingJob-request-OutputDataConfig"></a>
Specifies the path to the S3 bucket where you want to store model artifacts\. Amazon SageMaker creates subfolders for the artifacts\.   
Type: [OutputDataConfig](API_OutputDataConfig.md) object  
Required: Yes 

 ** [ResourceConfig](#API_CreateTrainingJob_RequestSyntax) **   <a name="SageMaker-CreateTrainingJob-request-ResourceConfig"></a>
The resources, including the ML compute instances and ML storage volumes, to use for model training\.   
ML storage volumes store model artifacts and incremental states\. Training algorithms might also use ML storage volumes for scratch space\. If you want Amazon SageMaker to use the ML storage volume to store the training data, choose `File` as the `TrainingInputMode` in the algorithm specification\. For distributed training algorithms, specify an instance count greater than 1\.  
Type: [ResourceConfig](API_ResourceConfig.md) object  
Required: Yes 

 ** [RoleArn](#API_CreateTrainingJob_RequestSyntax) **   <a name="SageMaker-CreateTrainingJob-request-RoleArn"></a>
The Amazon Resource Name \(ARN\) of an IAM role that Amazon SageMaker can assume to perform tasks on your behalf\.   
During model training, Amazon SageMaker needs your permission to read input data from an S3 bucket, download a Docker image that contains training code, write model artifacts to an S3 bucket, write logs to Amazon CloudWatch Logs, and publish metrics to Amazon CloudWatch\. You grant permissions for all of these tasks to an IAM role\. For more information, see [Amazon SageMaker Roles](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html)\.   
To be able to pass this role to Amazon SageMaker, the caller of this API must have the `iam:PassRole` permission\.
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.  
Pattern: `^arn:aws[a-z\-]*:iam::\d{12}:role/?[a-zA-Z_0-9+=,.@\-_/]+$`   
Required: Yes 

 ** [StoppingCondition](#API_CreateTrainingJob_RequestSyntax) **   <a name="SageMaker-CreateTrainingJob-request-StoppingCondition"></a>
Sets a duration for training\. Use this parameter to cap model training costs\. To stop a job, Amazon SageMaker sends the algorithm the `SIGTERM` signal, which delays job termination for 120 seconds\. Algorithms might use this 120\-second window to save the model artifacts\.   
When Amazon SageMaker terminates a job because the stopping condition has been met, training algorithms provided by Amazon SageMaker save the intermediate results of the job\. This intermediate data is a valid model artifact\. You can use it to create a model using the `CreateModel` API\.   
Type: [StoppingCondition](API_StoppingCondition.md) object  
Required: Yes 

 ** [Tags](#API_CreateTrainingJob_RequestSyntax) **   <a name="SageMaker-CreateTrainingJob-request-Tags"></a>
An array of key\-value pairs\. For more information, see [Using Cost Allocation Tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html#allocation-what) in the *AWS Billing and Cost Management User Guide*\.   
Type: Array of [Tag](API_Tag.md) objects  
Array Members: Minimum number of 0 items\. Maximum number of 50 items\.  
Required: No 

 ** [TrainingJobName](#API_CreateTrainingJob_RequestSyntax) **   <a name="SageMaker-CreateTrainingJob-request-TrainingJobName"></a>
The name of the training job\. The name must be unique within an AWS Region in an AWS account\.   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes 

 ** [VpcConfig](#API_CreateTrainingJob_RequestSyntax) **   <a name="SageMaker-CreateTrainingJob-request-VpcConfig"></a>
A [VpcConfig](API_VpcConfig.md) object that specifies the VPC that you want your training job to connect to\. Control access to and from your training container by configuring the VPC\. For more information, see [Protect Training Jobs by Using an Amazon Virtual Private Cloud](https://docs.aws.amazon.com/sagemaker/latest/dg/train-vpc.html)\.  
Type: [VpcConfig](API_VpcConfig.md) object  
Required: No    Response Syntax  

```
{
   "[TrainingJobArn](#SageMaker-CreateTrainingJob-response-TrainingJobArn)": "string"
}
```   Response Elements  If the action is successful, the service sends back an HTTP 200 response\. The following data is returned in JSON format by the service\.  

 ** [TrainingJobArn](#API_CreateTrainingJob_ResponseSyntax) **   <a name="SageMaker-CreateTrainingJob-response-TrainingJobArn"></a>
The Amazon Resource Name \(ARN\) of the training job\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:training-job/.*`     Errors  For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.  

 **ResourceInUse**   
Resource being accessed is in use\.  
HTTP Status Code: 400 

 **ResourceLimitExceeded**   
 You have exceeded an Amazon SageMaker resource limit\. For example, you might have too many training jobs created\.   
HTTP Status Code: 400    See Also   For more information about using this API in one of the language\-specific AWS SDKs, see the following:    [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/CreateTrainingJob)     [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/CreateTrainingJob)     [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/CreateTrainingJob)     [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/CreateTrainingJob)     [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/CreateTrainingJob)     [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/CreateTrainingJob)     [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/CreateTrainingJob)     [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/CreateTrainingJob)     [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/CreateTrainingJob)    ](API_CreateTrainingJob.md#API_CreateTrainingJob.title) request\. While using the format, an S3 manifest file needs to be generated that contains the list of images and their corresponding annotations\. The manifest file format should be in [JSON Lines](http://jsonlines.org/) format in which each line represents one sample\. The images are specified using the `'source-ref'` tag that points to the S3 location of the image\. The annotations are provided under the `"AttributeName"` parameter value as specified in the [ CreateTrainingJob Service: Amazon SageMaker Service APIrequestsCreateTrainingJob  Starts a model training job\. After training completes, Amazon SageMaker saves the resulting model artifacts to an Amazon S3 location that you specify\.  If you choose to host your model using Amazon SageMaker hosting services, you can use the resulting model artifacts as part of the model\. You can also use the artifacts in a machine learning service other than Amazon SageMaker, provided that you know how to use them for inferences\.  In the request body, you provide the following:     `AlgorithmSpecification` \- Identifies the training algorithm to use\.     `HyperParameters` \- Specify these algorithm\-specific parameters to influence the quality of the final model\. For a list of hyperparameters for each training algorithm provided by Amazon SageMaker, see [Algorithms](https://docs.aws.amazon.com/sagemaker/latest/dg/algos.html)\.     `InputDataConfig` \- Describes the training dataset and the Amazon S3 location where it is stored\.    `OutputDataConfig` \- Identifies the Amazon S3 location where you want Amazon SageMaker to save the results of model training\.      `ResourceConfig` \- Identifies the resources, ML compute instances, and ML storage volumes to deploy for model training\. In distributed training, you specify more than one instance\.     `RoleARN` \- The Amazon Resource Number \(ARN\) that Amazon SageMaker assumes to perform tasks on your behalf during model training\. You must grant this role the necessary permissions so that Amazon SageMaker can successfully complete model training\.     `StoppingCondition` \- Sets a duration for training\. Use this parameter to cap model training costs\.     For more information about Amazon SageMaker, see [How It Works](https://docs.aws.amazon.com/sagemaker/latest/dg/how-it-works.html)\.   Request Syntax  

```
{
   "[AlgorithmSpecification](#SageMaker-CreateTrainingJob-request-AlgorithmSpecification)": { 
      "[AlgorithmName](API_AlgorithmSpecification.md#SageMaker-Type-AlgorithmSpecification-AlgorithmName)": "string",
      "[MetricDefinitions](API_AlgorithmSpecification.md#SageMaker-Type-AlgorithmSpecification-MetricDefinitions)": [ 
         { 
            "[Name](API_MetricDefinition.md#SageMaker-Type-MetricDefinition-Name)": "string",
            "[Regex](API_MetricDefinition.md#SageMaker-Type-MetricDefinition-Regex)": "string"
         }
      ],
      "[TrainingImage](API_AlgorithmSpecification.md#SageMaker-Type-AlgorithmSpecification-TrainingImage)": "string",
      "[TrainingInputMode](API_AlgorithmSpecification.md#SageMaker-Type-AlgorithmSpecification-TrainingInputMode)": "string"
   },
   "[EnableInterContainerTrafficEncryption](#SageMaker-CreateTrainingJob-request-EnableInterContainerTrafficEncryption)": boolean,
   "[EnableNetworkIsolation](#SageMaker-CreateTrainingJob-request-EnableNetworkIsolation)": boolean,
   "[HyperParameters](#SageMaker-CreateTrainingJob-request-HyperParameters)": { 
      "string" : "string" 
   },
   "[InputDataConfig](#SageMaker-CreateTrainingJob-request-InputDataConfig)": [ 
      { 
         "[ChannelName](API_Channel.md#SageMaker-Type-Channel-ChannelName)": "string",
         "[CompressionType](API_Channel.md#SageMaker-Type-Channel-CompressionType)": "string",
         "[ContentType](API_Channel.md#SageMaker-Type-Channel-ContentType)": "string",
         "[DataSource](API_Channel.md#SageMaker-Type-Channel-DataSource)": { 
            "[S3DataSource](API_DataSource.md#SageMaker-Type-DataSource-S3DataSource)": { 
               "[AttributeNames](API_S3DataSource.md#SageMaker-Type-S3DataSource-AttributeNames)": [ "string" ],
               "[S3DataDistributionType](API_S3DataSource.md#SageMaker-Type-S3DataSource-S3DataDistributionType)": "string",
               "[S3DataType](API_S3DataSource.md#SageMaker-Type-S3DataSource-S3DataType)": "string",
               "[S3Uri](API_S3DataSource.md#SageMaker-Type-S3DataSource-S3Uri)": "string"
            }
         },
         "[InputMode](API_Channel.md#SageMaker-Type-Channel-InputMode)": "string",
         "[RecordWrapperType](API_Channel.md#SageMaker-Type-Channel-RecordWrapperType)": "string",
         "[ShuffleConfig](API_Channel.md#SageMaker-Type-Channel-ShuffleConfig)": { 
            "[Seed](API_ShuffleConfig.md#SageMaker-Type-ShuffleConfig-Seed)": number
         }
      }
   ],
   "[OutputDataConfig](#SageMaker-CreateTrainingJob-request-OutputDataConfig)": { 
      "[KmsKeyId](API_OutputDataConfig.md#SageMaker-Type-OutputDataConfig-KmsKeyId)": "string",
      "[S3OutputPath](API_OutputDataConfig.md#SageMaker-Type-OutputDataConfig-S3OutputPath)": "string"
   },
   "[ResourceConfig](#SageMaker-CreateTrainingJob-request-ResourceConfig)": { 
      "[InstanceCount](API_ResourceConfig.md#SageMaker-Type-ResourceConfig-InstanceCount)": number,
      "[InstanceType](API_ResourceConfig.md#SageMaker-Type-ResourceConfig-InstanceType)": "string",
      "[VolumeKmsKeyId](API_ResourceConfig.md#SageMaker-Type-ResourceConfig-VolumeKmsKeyId)": "string",
      "[VolumeSizeInGB](API_ResourceConfig.md#SageMaker-Type-ResourceConfig-VolumeSizeInGB)": number
   },
   "[RoleArn](#SageMaker-CreateTrainingJob-request-RoleArn)": "string",
   "[StoppingCondition](#SageMaker-CreateTrainingJob-request-StoppingCondition)": { 
      "[MaxRuntimeInSeconds](API_StoppingCondition.md#SageMaker-Type-StoppingCondition-MaxRuntimeInSeconds)": number
   },
   "[Tags](#SageMaker-CreateTrainingJob-request-Tags)": [ 
      { 
         "[Key](API_Tag.md#SageMaker-Type-Tag-Key)": "string",
         "[Value](API_Tag.md#SageMaker-Type-Tag-Value)": "string"
      }
   ],
   "[TrainingJobName](#SageMaker-CreateTrainingJob-request-TrainingJobName)": "string",
   "[VpcConfig](#SageMaker-CreateTrainingJob-request-VpcConfig)": { 
      "[SecurityGroupIds](API_VpcConfig.md#SageMaker-Type-VpcConfig-SecurityGroupIds)": [ "string" ],
      "[Subnets](API_VpcConfig.md#SageMaker-Type-VpcConfig-Subnets)": [ "string" ]
   }
}
```   Request Parameters  For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\. The request accepts the following data in JSON format\.  

 ** [AlgorithmSpecification](#API_CreateTrainingJob_RequestSyntax) **   <a name="SageMaker-CreateTrainingJob-request-AlgorithmSpecification"></a>
The registry path of the Docker image that contains the training algorithm and algorithm\-specific metadata, including the input mode\. For more information about algorithms provided by Amazon SageMaker, see [Algorithms](https://docs.aws.amazon.com/sagemaker/latest/dg/algos.html)\. For information about providing your own algorithms, see [Using Your Own Algorithms with Amazon SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/your-algorithms.html)\.   
Type: [AlgorithmSpecification](API_AlgorithmSpecification.md) object  
Required: Yes 

 ** [EnableInterContainerTrafficEncryption](#API_CreateTrainingJob_RequestSyntax) **   <a name="SageMaker-CreateTrainingJob-request-EnableInterContainerTrafficEncryption"></a>
To encrypt all communications between ML compute instances in distributed training, choose `True`\. Encryption provides greater security for distributed training, but training might take longer\. How long it takes depends on the amount of communication between compute instances, especially if you use a deep learning algorithm in distributed training\. For more information, see [Protect Communications Between ML Compute Instances in a Distributed Training Job](https://docs.aws.amazon.com/sagemaker/latest/dg/train-encrypt.html)\.  
Type: Boolean  
Required: No 

 ** [EnableNetworkIsolation](#API_CreateTrainingJob_RequestSyntax) **   <a name="SageMaker-CreateTrainingJob-request-EnableNetworkIsolation"></a>
Isolates the training container\. No inbound or outbound network calls can be made, except for calls between peers within a training cluster for distributed training\. If you enable network isolation for training jobs that are configured to use a VPC, Amazon SageMaker downloads and uploads customer data and model artifacts through the specified VPC, but the training container does not have network access\.  
The Semantic Segmentation built\-in algorithm does not support network isolation\.
Type: Boolean  
Required: No 

 ** [HyperParameters](#API_CreateTrainingJob_RequestSyntax) **   <a name="SageMaker-CreateTrainingJob-request-HyperParameters"></a>
Algorithm\-specific parameters that influence the quality of the model\. You set hyperparameters before you start the learning process\. For a list of hyperparameters for each training algorithm provided by Amazon SageMaker, see [Algorithms](https://docs.aws.amazon.com/sagemaker/latest/dg/algos.html)\.   
You can specify a maximum of 100 hyperparameters\. Each hyperparameter is a key\-value pair\. Each key and value is limited to 256 characters, as specified by the `Length Constraint`\.   
Type: String to string map  
Key Length Constraints: Maximum length of 256\.  
Value Length Constraints: Maximum length of 256\.  
Required: No 

 ** [InputDataConfig](#API_CreateTrainingJob_RequestSyntax) **   <a name="SageMaker-CreateTrainingJob-request-InputDataConfig"></a>
An array of `Channel` objects\. Each channel is a named input source\. `InputDataConfig` describes the input data and its location\.   
Algorithms can accept input data from one or more channels\. For example, an algorithm might have two channels of input data, `training_data` and `validation_data`\. The configuration for each channel provides the S3 location where the input data is stored\. It also provides information about the stored data: the MIME type, compression method, and whether the data is wrapped in RecordIO format\.   
Depending on the input mode that the algorithm supports, Amazon SageMaker either copies input data files from an S3 bucket to a local directory in the Docker container, or makes it available as input streams\.   
Type: Array of [Channel](API_Channel.md) objects  
Array Members: Minimum number of 1 item\. Maximum number of 8 items\.  
Required: No 

 ** [OutputDataConfig](#API_CreateTrainingJob_RequestSyntax) **   <a name="SageMaker-CreateTrainingJob-request-OutputDataConfig"></a>
Specifies the path to the S3 bucket where you want to store model artifacts\. Amazon SageMaker creates subfolders for the artifacts\.   
Type: [OutputDataConfig](API_OutputDataConfig.md) object  
Required: Yes 

 ** [ResourceConfig](#API_CreateTrainingJob_RequestSyntax) **   <a name="SageMaker-CreateTrainingJob-request-ResourceConfig"></a>
The resources, including the ML compute instances and ML storage volumes, to use for model training\.   
ML storage volumes store model artifacts and incremental states\. Training algorithms might also use ML storage volumes for scratch space\. If you want Amazon SageMaker to use the ML storage volume to store the training data, choose `File` as the `TrainingInputMode` in the algorithm specification\. For distributed training algorithms, specify an instance count greater than 1\.  
Type: [ResourceConfig](API_ResourceConfig.md) object  
Required: Yes 

 ** [RoleArn](#API_CreateTrainingJob_RequestSyntax) **   <a name="SageMaker-CreateTrainingJob-request-RoleArn"></a>
The Amazon Resource Name \(ARN\) of an IAM role that Amazon SageMaker can assume to perform tasks on your behalf\.   
During model training, Amazon SageMaker needs your permission to read input data from an S3 bucket, download a Docker image that contains training code, write model artifacts to an S3 bucket, write logs to Amazon CloudWatch Logs, and publish metrics to Amazon CloudWatch\. You grant permissions for all of these tasks to an IAM role\. For more information, see [Amazon SageMaker Roles](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html)\.   
To be able to pass this role to Amazon SageMaker, the caller of this API must have the `iam:PassRole` permission\.
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.  
Pattern: `^arn:aws[a-z\-]*:iam::\d{12}:role/?[a-zA-Z_0-9+=,.@\-_/]+$`   
Required: Yes 

 ** [StoppingCondition](#API_CreateTrainingJob_RequestSyntax) **   <a name="SageMaker-CreateTrainingJob-request-StoppingCondition"></a>
Sets a duration for training\. Use this parameter to cap model training costs\. To stop a job, Amazon SageMaker sends the algorithm the `SIGTERM` signal, which delays job termination for 120 seconds\. Algorithms might use this 120\-second window to save the model artifacts\.   
When Amazon SageMaker terminates a job because the stopping condition has been met, training algorithms provided by Amazon SageMaker save the intermediate results of the job\. This intermediate data is a valid model artifact\. You can use it to create a model using the `CreateModel` API\.   
Type: [StoppingCondition](API_StoppingCondition.md) object  
Required: Yes 

 ** [Tags](#API_CreateTrainingJob_RequestSyntax) **   <a name="SageMaker-CreateTrainingJob-request-Tags"></a>
An array of key\-value pairs\. For more information, see [Using Cost Allocation Tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html#allocation-what) in the *AWS Billing and Cost Management User Guide*\.   
Type: Array of [Tag](API_Tag.md) objects  
Array Members: Minimum number of 0 items\. Maximum number of 50 items\.  
Required: No 

 ** [TrainingJobName](#API_CreateTrainingJob_RequestSyntax) **   <a name="SageMaker-CreateTrainingJob-request-TrainingJobName"></a>
The name of the training job\. The name must be unique within an AWS Region in an AWS account\.   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes 

 ** [VpcConfig](#API_CreateTrainingJob_RequestSyntax) **   <a name="SageMaker-CreateTrainingJob-request-VpcConfig"></a>
A [VpcConfig](API_VpcConfig.md) object that specifies the VPC that you want your training job to connect to\. Control access to and from your training container by configuring the VPC\. For more information, see [Protect Training Jobs by Using an Amazon Virtual Private Cloud](https://docs.aws.amazon.com/sagemaker/latest/dg/train-vpc.html)\.  
Type: [VpcConfig](API_VpcConfig.md) object  
Required: No    Response Syntax  

```
{
   "[TrainingJobArn](#SageMaker-CreateTrainingJob-response-TrainingJobArn)": "string"
}
```   Response Elements  If the action is successful, the service sends back an HTTP 200 response\. The following data is returned in JSON format by the service\.  

 ** [TrainingJobArn](#API_CreateTrainingJob_ResponseSyntax) **   <a name="SageMaker-CreateTrainingJob-response-TrainingJobArn"></a>
The Amazon Resource Name \(ARN\) of the training job\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:training-job/.*`     Errors  For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.  

 **ResourceInUse**   
Resource being accessed is in use\.  
HTTP Status Code: 400 

 **ResourceLimitExceeded**   
 You have exceeded an Amazon SageMaker resource limit\. For example, you might have too many training jobs created\.   
HTTP Status Code: 400    See Also   For more information about using this API in one of the language\-specific AWS SDKs, see the following:    [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/CreateTrainingJob)     [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/CreateTrainingJob)     [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/CreateTrainingJob)     [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/CreateTrainingJob)     [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/CreateTrainingJob)     [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/CreateTrainingJob)     [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/CreateTrainingJob)     [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/CreateTrainingJob)     [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/CreateTrainingJob)    ](API_CreateTrainingJob.md#API_CreateTrainingJob.title) request\. It can also contain additional metadata under the `metadata` tag, but these are ignored by the algorithm\. In the following example, the `"AttributeName"` is `"class"` and the corresponding label value is `"0"` for the first image and `“1”` for the second image:

```
{"source-ref":"s3://image/filename1.jpg", "class":"0"} 
{"source-ref":"s3://image/filename2.jpg", "class":"1", "class-metadata": {"class-name": "cat", "type" : "groundtruth/image-classification"}}
```

For more information on augmented manifest files, see [Provide Dataset Metadata to Training Jobs with an Augmented Manifest File](augmented-manifest.md)\.

### Incremental Training<a name="IC-incremental-training"></a>

You can also seed the training of a new model with the artifacts from a model that you trained previously with Amazon SageMaker\. Incremental training saves training time when you want to train a new model with the same or similar data\. Amazon SageMaker image classification models can be seeded only with another build\-in image classification model trained in Amazon SageMaker\.

To use a pretrained model, in the [CreateTrainingJob](API_CreateTrainingJob.md) request, specify the `ChannelName` as "model" in the `InputDataConfig` parameter\. Set the `ContentType` for the model channel to `application/x-sagemaker-model`\. The input hyperparameters of both the new model and the pretrained model that you upload to the model channel must have the same settings for the `num_layers`, `image_shape` and `num_classes` input parameters\. These parameters define the network architecture\. For the pretrained model file, use the compressed model artifacts \(in \.tar\.gz format\) output by Amazon SageMaker\. You can use either RecordIO or image formats for input data\.

For a sample notebook that shows how to use incremental training with the Amazon SageMaker image classification algorithm, see the [End\-to\-End Incremental Training Image Classification Example](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/imageclassification_caltech/Image-classification-incremental-training-highlevel.ipynb)\. For more information on incremental training and for instructions on how to use it, see [Incremental Training in Amazon SageMaker](incremental-training.md)\. 

### Inference with the Image Format Algorithm<a name="IC-inference"></a>

The generated models can be hosted for inference and support encoded `.jpg` and `.png` image formats as `image/png, image/jpeg`, and `application/x-image` content\-type\. The output is the probability values for all classes encoded in JSON format, or in [JSON Lines text format](http://jsonlines.org/) for batch transform\. The image classification model processes a single image per request and so outputs only one line in the JSON or JSON Lines format\. The following is an example of a response in JSON Lines format:

```
accept: application/jsonlines
 
 {"prediction": [prob_0, prob_1, prob_2, prob_3, ...]}
```

For more details on training and inference, see the image classification sample notebook instances referenced in the introduction\.

## EC2 Instance Recommendation for the Image Classification Algorithm<a name="IC-instances"></a>

For image classification, we support the following GPU instances for training: `ml.p2.xlarge`, `ml.p2.8xlarge`, `ml.p2.16xlarge`, `ml.p3.2xlarge`, `ml.p3.8xlarge`and `ml.p3.16xlarge`\. We recommend using GPU instances with more memory for training with large batch sizes\. However, both CPU \(such as C4\) and GPU \(such as P2 and P3\) instances can be used for the inference\. You can also run the algorithm on multi\-GPU and multi\-machine settings for distributed training\.

Both P2 and P3 instances are supported in the image classification algorithm\.

## Image Classification Sample Notebooks<a name="IC-sample-notebooks"></a>

For a sample notebook that uses the Amazon SageMaker image classification algorithm to train a model on the [caltech\-256 dataset](http://www.vision.caltech.edu/Image_Datasets/Caltech256/) and then to deploy it to perform inferences, see the [End\-to\-End Multiclass Image Classification Example](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/imageclassification_caltech/Image-classification-fulltraining.ipynb)\. For instructions how to create and access Jupyter notebook instances that you can use to run the example in Amazon SageMaker, see [Use Notebook Instances](nbi.md)\. Once you have created a notebook instance and opened it, select the **SageMaker Examples** tab to see a list of all the Amazon SageMaker samples\. The example image classification notebooks are located in the **Introduction to Amazon algorithms** section\. To open a notebook, click on its **Use** tab and select **Create copy**\.