# Object Detection Algorithm<a name="object-detection"></a>

The Amazon SageMaker Object Detection algorithm detects and classifies objects in images using a single deep neural network\. It is a supervised learning algorithm that takes images as input and identifies all instances of objects within the image scene\. The object is categorized into one of the classes in a specified collection with a confidence score that it belongs to the class\. Its location and scale in the image are indicated by a rectangular bounding box\. It uses the [Single Shot multibox Detector \(SSD\)](https://arxiv.org/pdf/1512.02325.pdf) framework and supports two base networks: [VGG](https://arxiv.org/pdf/1409.1556.pdf) and [ResNet](https://arxiv.org/pdf/1603.05027.pdf)\. The network can be trained from scratch, or trained with models that have been pre\-trained on the [ImageNet](http://www.image-net.org/) dataset\.

**Topics**
+ [Input/Output Interface for the Object Detection Algorithm](#object-detection-inputoutput)
+ [EC2 Instance Recommendation for the Object Detection Algorithm](#object-detection-instances)
+ [Object Detection Sample Notebooks](#object-detection-sample-notebooks)
+ [How Object Detection Works](algo-object-detection-tech-notes.md)
+ [Object Detection Hyperparameters](object-detection-api-config.md)
+ [Tune an Object Detection Model](object-detection-tuning.md)
+ [Object Detection Request and Response Formats](object-detection-in-formats.md)

## Input/Output Interface for the Object Detection Algorithm<a name="object-detection-inputoutput"></a>

The Amazon SageMaker Object Detection algorithm supports both RecordIO \(`application/x-recordio`\) and image \(`image/png`, `image/jpeg`, and `application/x-image`\) content types for training in file mode and supports RecordIO \(`application/x-recordio`\) for training in pipe mode\. However you can also train in pipe mode using the image files \(`image/png`, `image/jpeg`, and `application/x-image`\), without creating RecordIO files, by using the augmented manifest format\. The recommended input format for the Amazon SageMaker object detection algorithms is [Apache MXNet RecordIO](https://mxnet.incubator.apache.org/architecture/note_data_loading.html)\. However, you can also use raw images in \.jpg or \.png format\. The algorithm supports only `application/x-image` for inference\.

**Note**  
To maintain better interoperability with existing deep learning frameworks, this differs from the protobuf data formats commonly used by other Amazon SageMaker algorithms\.

See the [Object Detection Sample Notebooks](#object-detection-sample-notebooks) for more details on data formats\.

### Train with the RecordIO Format<a name="object-detection-recordio-training"></a>

If you use the RecordIO format for training, specify both train and validation channels as values for the `InputDataConfig` parameter of the [CreateTrainingJob](API_CreateTrainingJob.md) request\. Specify one RecordIO \(\.rec\) file in the train channel and one RecordIO file in the validation channel\. Set the content type for both channels to `application/x-recordio`\. An example of how to generate RecordIO file can be found in the object detection sample notebook\. You can also use tools from the [MXNet](https://github.com/apache/incubator-mxnet/tree/master/example/ssd) example to generate RecordIO files for popular datasets like the [PASCAL Visual Object Classes](http://host.robots.ox.ac.uk/pascal/VOC/) and [Common Objects in Context \(COCO\)](http://cocodataset.org/#home)\.

### Train with the Image Format<a name="object-detection-image-training"></a>

If you use the image format for training, specify `train`, `validation`, `train_annotation`, and `validation_annotation` channels as values for the `InputDataConfig` parameter of [CreateTrainingJob](API_CreateTrainingJob.md) request\. Specify the individual image data \(\.jpg or \.png\) files for the train and validation channels\. For annotation data, you can use the JSON format\. Specify the corresponding \.json files in the `train_annotation` and `validation_annotation` channels\. Set the content type for all four channels to `image/png` or `image/jpeg` based on the image type\. You can also use the content type `application/x-image` when your dataset contains both \.jpg and \.png images\. The following is an example of a \.json file\.

```
{
   "file": "your_image_directory/sample_image1.jpg",
   "image_size": [
      {
         "width": 500,
         "height": 400,
         "depth": 3
      }
   ],
   "annotations": [
      {
         "class_id": 0,
         "left": 111,
         "top": 134,
         "width": 61,
         "height": 128
      },
      {
         "class_id": 0,
         "left": 161,
         "top": 250,
         "width": 79,
         "height": 143
      },
      {
         "class_id": 1,
         "left": 101,
         "top": 185,
         "width": 42,
         "height": 130
      }
   ],
   "categories": [
      {
         "class_id": 0,
         "name": "dog"
      },
      {
         "class_id": 1,
         "name": "cat"
      }
   ]
}
```

Each image needs a \.json file for annotation, and the \.json file should have the same name as the corresponding image\. The name of above \.json file should be "sample\_image1\.json"\. There are four properties in the annotation \.json file\. The property "file" specifies the relative path of the image file\. For example, if your training images and corresponding \.json files are stored in s3://*your\_bucket*/train/sample\_image and s3://*your\_bucket*/train\_annotation, specify the path for your train and train\_annotation channels as s3://*your\_bucket*/train and s3://*your\_bucket*/train\_annotation, respectively\. 

In the \.json file, the relative path for an image named sample\_image1\.jpg should be sample\_image/sample\_image1\.jpg\. The `"image_size"` property specifies the overall image dimensions\. The SageMaker object detection algorithm currently only supports 3\-channel images\. The `"annotations"` property specifies the categories and bounding boxes for objects within the image\. Each object is annotated by a `"class_id"` index and by four bounding box coordinates \(`"left"`, `"top"`, `"width"`, `"height"`\)\. The `"left"` \(x\-coordinate\) and `"top"` \(y\-coordinate\) values represent the upper\-left corner of the bounding box\. The `"width"` \(x\-coordinate\) and `"height"` \(y\-coordinate\) values represent the dimensions of the bounding box\. The origin \(0, 0\) is the upper\-left corner of the entire image\. If you have multiple objects within one image, all the annotations should be included in a single \.json file\. The `"categories"` property stores the mapping between the class index and class name\. The class indices should be numbered successively and the numbering should start with 0\. The `"categories"` property is optional for the annotation \.json file

### Train with Augmented Manifest Image Format<a name="object-detection-augmented-manifest-training"></a>

The augmented manifest format enables you to do training in pipe mode using image files without needing to create RecordIO files\. You need to specify both train and validation channels as values for the `InputDataConfig` parameter of the [ CreateTrainingJob Service: Amazon SageMaker Service APIrequestsCreateTrainingJob  Starts a model training job\. After training completes, Amazon SageMaker saves the resulting model artifacts to an Amazon S3 location that you specify\.  If you choose to host your model using Amazon SageMaker hosting services, you can use the resulting model artifacts as part of the model\. You can also use the artifacts in a machine learning service other than Amazon SageMaker, provided that you know how to use them for inferences\.  In the request body, you provide the following:     `AlgorithmSpecification` \- Identifies the training algorithm to use\.     `HyperParameters` \- Specify these algorithm\-specific parameters to enable the estimation of model parameters during training\. Hyperparameters can be tuned to optimize this learning process\. For a list of hyperparameters for each training algorithm provided by Amazon SageMaker, see [Algorithms](https://docs.aws.amazon.com/sagemaker/latest/dg/algos.html)\.     `InputDataConfig` \- Describes the training dataset and the Amazon S3 location where it is stored\.    `OutputDataConfig` \- Identifies the Amazon S3 location where you want Amazon SageMaker to save the results of model training\.      `ResourceConfig` \- Identifies the resources, ML compute instances, and ML storage volumes to deploy for model training\. In distributed training, you specify more than one instance\.     `RoleARN` \- The Amazon Resource Number \(ARN\) that Amazon SageMaker assumes to perform tasks on your behalf during model training\. You must grant this role the necessary permissions so that Amazon SageMaker can successfully complete model training\.     `StoppingCondition` \- Sets a time limit for training\. Use this parameter to cap model training costs\.     For more information about Amazon SageMaker, see [How It Works](https://docs.aws.amazon.com/sagemaker/latest/dg/how-it-works.html)\.   Request Syntax  

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
Key Pattern: `.*`   
Value Length Constraints: Maximum length of 256\.  
Value Pattern: `.*`   
Required: No 

 ** [InputDataConfig](#API_CreateTrainingJob_RequestSyntax) **   <a name="SageMaker-CreateTrainingJob-request-InputDataConfig"></a>
An array of `Channel` objects\. Each channel is a named input source\. `InputDataConfig` describes the input data and its location\.   
Algorithms can accept input data from one or more channels\. For example, an algorithm might have two channels of input data, `training_data` and `validation_data`\. The configuration for each channel provides the S3 location where the input data is stored\. It also provides information about the stored data: the MIME type, compression method, and whether the data is wrapped in RecordIO format\.   
Depending on the input mode that the algorithm supports, Amazon SageMaker either copies input data files from an S3 bucket to a local directory in the Docker container, or makes it available as input streams\.   
Type: Array of [Channel](API_Channel.md) objects  
Array Members: Minimum number of 1 item\. Maximum number of 20 items\.  
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
Specifies a limit to how long a model training job can run\. When the job reaches the time limit, Amazon SageMaker ends the training job\. Use this API to cap model training costs\.  
To stop a job, Amazon SageMaker sends the algorithm the `SIGTERM` signal, which delays job termination for 120 seconds\. Algorithms can use this 120\-second window to save the model artifacts, so the results of training are not lost\.   
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
HTTP Status Code: 400    See Also   For more information about using this API in one of the language\-specific AWS SDKs, see the following:    [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/CreateTrainingJob)     [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/CreateTrainingJob)     [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/CreateTrainingJob)     [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/CreateTrainingJob)     [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/CreateTrainingJob)     [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/CreateTrainingJob)     [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/CreateTrainingJob)     [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/CreateTrainingJob)     [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/CreateTrainingJob)     [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/CreateTrainingJob)    ](API_CreateTrainingJob.md#API_CreateTrainingJob.title) request\. While using the format, an S3 manifest file needs to be generated that contains the list of images and their corresponding annotations\. The manifest file format should be in [JSON Lines](http://jsonlines.org/) format in which each line represents one sample\. The images are specified using the `'source-ref'` tag that points to the S3 location of the image\. The annotations are provided under the `"AttributeNames"` parameter value as specified in the [ CreateTrainingJob Service: Amazon SageMaker Service APIrequestsCreateTrainingJob  Starts a model training job\. After training completes, Amazon SageMaker saves the resulting model artifacts to an Amazon S3 location that you specify\.  If you choose to host your model using Amazon SageMaker hosting services, you can use the resulting model artifacts as part of the model\. You can also use the artifacts in a machine learning service other than Amazon SageMaker, provided that you know how to use them for inferences\.  In the request body, you provide the following:     `AlgorithmSpecification` \- Identifies the training algorithm to use\.     `HyperParameters` \- Specify these algorithm\-specific parameters to enable the estimation of model parameters during training\. Hyperparameters can be tuned to optimize this learning process\. For a list of hyperparameters for each training algorithm provided by Amazon SageMaker, see [Algorithms](https://docs.aws.amazon.com/sagemaker/latest/dg/algos.html)\.     `InputDataConfig` \- Describes the training dataset and the Amazon S3 location where it is stored\.    `OutputDataConfig` \- Identifies the Amazon S3 location where you want Amazon SageMaker to save the results of model training\.      `ResourceConfig` \- Identifies the resources, ML compute instances, and ML storage volumes to deploy for model training\. In distributed training, you specify more than one instance\.     `RoleARN` \- The Amazon Resource Number \(ARN\) that Amazon SageMaker assumes to perform tasks on your behalf during model training\. You must grant this role the necessary permissions so that Amazon SageMaker can successfully complete model training\.     `StoppingCondition` \- Sets a time limit for training\. Use this parameter to cap model training costs\.     For more information about Amazon SageMaker, see [How It Works](https://docs.aws.amazon.com/sagemaker/latest/dg/how-it-works.html)\.   Request Syntax  

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
Key Pattern: `.*`   
Value Length Constraints: Maximum length of 256\.  
Value Pattern: `.*`   
Required: No 

 ** [InputDataConfig](#API_CreateTrainingJob_RequestSyntax) **   <a name="SageMaker-CreateTrainingJob-request-InputDataConfig"></a>
An array of `Channel` objects\. Each channel is a named input source\. `InputDataConfig` describes the input data and its location\.   
Algorithms can accept input data from one or more channels\. For example, an algorithm might have two channels of input data, `training_data` and `validation_data`\. The configuration for each channel provides the S3 location where the input data is stored\. It also provides information about the stored data: the MIME type, compression method, and whether the data is wrapped in RecordIO format\.   
Depending on the input mode that the algorithm supports, Amazon SageMaker either copies input data files from an S3 bucket to a local directory in the Docker container, or makes it available as input streams\.   
Type: Array of [Channel](API_Channel.md) objects  
Array Members: Minimum number of 1 item\. Maximum number of 20 items\.  
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
Specifies a limit to how long a model training job can run\. When the job reaches the time limit, Amazon SageMaker ends the training job\. Use this API to cap model training costs\.  
To stop a job, Amazon SageMaker sends the algorithm the `SIGTERM` signal, which delays job termination for 120 seconds\. Algorithms can use this 120\-second window to save the model artifacts, so the results of training are not lost\.   
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
HTTP Status Code: 400    See Also   For more information about using this API in one of the language\-specific AWS SDKs, see the following:    [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/CreateTrainingJob)     [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/CreateTrainingJob)     [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/CreateTrainingJob)     [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/CreateTrainingJob)     [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/CreateTrainingJob)     [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/CreateTrainingJob)     [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/CreateTrainingJob)     [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/CreateTrainingJob)     [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/CreateTrainingJob)     [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/CreateTrainingJob)    ](API_CreateTrainingJob.md#API_CreateTrainingJob.title) request\. It can also contain additional metadata under the `metadata` tag, but these are ignored by the algorithm\. In the following example, the `"AttributeNames` are contained in the list `["source-ref", "bounding-box"]`:

```
{"source-ref": "s3://your_bucket/image1.jpg", "bounding-box":{"image_size":[{ "width": 500, "height": 400, "depth":3}], "annotations":[{"class_id": 0, "left": 111, "top": 134, "width": 61, "height": 128}, {"class_id": 5, "left": 161, "top": 250, "width": 80, "height": 50}]}, "bounding-box-metadata":{"class-map":{"0": "dog", "5": "horse"}, "type": "groundtruth/object_detection"}}
{"source-ref": "s3://your_bucket/image2.jpg", "bounding-box":{"image_size":[{ "width": 400, "height": 300, "depth":3}], "annotations":[{"class_id": 1, "left": 100, "top": 120, "width": 43, "height": 78}]}, "bounding-box-metadata":{"class-map":{"1": "cat"}, "type": "groundtruth/object_detection"}}
```

The order of `"AttributeNames"` in the input files matters when training the Object Detection algorithm\. It accepts piped data in a specific order, with `image` first, followed by `annotations`\. So the "AttributeNames" in this example are provided with `"source-ref"` first, followed by `"bounding-box"`\. When using Object Detection with Augmented Manifest, the value of parameter `RecordWrapperType` must be set as `"RecordIO"`\.

For more information on augmented manifest files, see [Provide Dataset Metadata to Training Jobs with an Augmented Manifest File](augmented-manifest.md)\.

### Incremental Training<a name="object-detection-incremental-training"></a>

You can also seed the training of a new model with the artifacts from a model that you trained previously with Amazon SageMaker\. Incremental training saves training time when you want to train a new model with the same or similar data\. Amazon SageMaker object detection models can be seeded only with another build\-in object detection model trained in Amazon SageMaker\.

To use a pretrained model, in the [CreateTrainingJob](API_CreateTrainingJob.md) request, specify the `ChannelName` as "model" in the `InputDataConfig` parameter\. Set the `ContentType` for the model channel to `application/x-sagemaker-model`\. The input hyperparameters of both the new model and the pretrained model that you upload to the model channel must have the same settings for the `base_network` and `num_classes` input parameters\. These parameters define the network architecture\. For the pretrained model file, use the compressed model artifacts \(in \.tar\.gz format\) output by Amazon SageMaker\. You can use either RecordIO or image formats for input data\.

For a sample notebook that shows how to use incremental training with the Amazon SageMaker object detection algorithm, see [Amazon SageMaker Object Detection Incremental Training](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/object_detection_pascalvoc_coco/object_detection_incremental_training.ipynb) sample notebook\. For more information on incremental training and for instructions on how to use it, see [Incremental Training in Amazon SageMaker](incremental-training.md)\. 

## EC2 Instance Recommendation for the Object Detection Algorithm<a name="object-detection-instances"></a>

For object detection, we support the following GPU instances for training: `ml.p2.xlarge`, `ml.p2.8xlarge`, `ml.p2.16xlarge`, `ml.p3.2xlarge`, `ml.p3.8xlarge` and `ml.p3.16xlarge`\. We recommend using GPU instances with more memory for training with large batch sizes\. You can also run the algorithm on multi\-GPU and multi\-machine settings for distributed training\. However, both CPU \(such as C5 and M5\) and GPU \(such as P2 and P3\) instances can be used for the inference\. All the supported instance types for inference are itemized on [Amazon SageMaker ML Instance Types](https://aws.amazon.com/sagemaker/pricing/instance-types/)\.

## Object Detection Sample Notebooks<a name="object-detection-sample-notebooks"></a>

For a sample notebook that shows how to use the Amazon SageMaker Object Detection algorithm to train and host a model on the COCO dataset using the Single Shot multibox Detector algorithm, see [Object Detection using the Image and JSON format](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/object_detection_pascalvoc_coco/object_detection_image_json_format.ipynb)\. For instructions how to create and access Jupyter notebook instances that you can use to run the example in Amazon SageMaker, see [Use Notebook Instances](nbi.md)\. Once you have created a notebook instance and opened it, select the **SageMaker Examples** tab to see a list of all the Amazon SageMaker samples\. The topic modeling example notebooks using the NTM algorithms are located in the **Introduction to Amazon algorithms** section\. To open a notebook, click on its **Use** tab and select **Create copy**\.