# Image Classification Algorithm<a name="image-classification"></a>

The Amazon SageMaker image classification algorithm is a supervised learning algorithm that supports multi\-label classification\. It takes an image as input and outputs one or more labels assigned to that image\. It uses a convolutional neural network \(ResNet\) that can be trained from scratch or trained using transfer learning when a large number of training images are not available\. 

The recommended input format for the Amazon SageMaker image classification algorithms is Apache MXNet [RecordIO](https://mxnet.apache.org/api/faq/recordio)\. However, you can also use raw images in \.jpg or \.png format\. Refer to [this discussion](https://mxnet.apache.org/api/architecture/note_data_loading) for a broad overview of efficient data preparation and loading for machine learning systems\. 

**Note**  
To maintain better interoperability with existing deep learning frameworks, this differs from the protobuf data formats commonly used by other Amazon SageMaker algorithms\.

For more information on convolutional networks, see: 
+ [Deep residual learning for image recognition](https://arxiv.org/abs/1512.03385) Kaiming He, et al\., 2016 IEEE Conference on Computer Vision and Pattern Recognition
+ [ImageNet image database](http://www.image-net.org/)
+ [Image classification with Gluon\-CV and MXNet](https://gluon-cv.mxnet.io/build/examples_classification/index.html)

**Topics**
+ [Input/Output Interface for the Image Classification Algorithm](#IC-inputoutput)
+ [EC2 Instance Recommendation for the Image Classification Algorithm](#IC-instances)
+ [Image Classification Sample Notebooks](#IC-sample-notebooks)
+ [How Image Classification Works](IC-HowItWorks.md)
+ [Image Classification Hyperparameters](IC-Hyperparameter.md)
+ [Tune an Image Classification Model](IC-tuning.md)

## Input/Output Interface for the Image Classification Algorithm<a name="IC-inputoutput"></a>

The SageMaker Image Classification algorithm supports both RecordIO \(`application/x-recordio`\) and image \(`image/png`, `image/jpeg`, and `application/x-image`\) content types for training in file mode, and supports the RecordIO \(`application/x-recordio`\) content type for training in pipe mode\. However, you can also train in pipe mode using the image files \(`image/png`, `image/jpeg`, and `application/x-image`\), without creating RecordIO files, by using the augmented manifest format\.

Distributed training is supported for file mode and pipe mode\. When using the RecordIO content type in pipe mode, you must set the `S3DataDistributionType` of the `S3DataSource` to `FullyReplicated`\.

The algorithm supports `image/png`, `image/jpeg`, and `application/x-image` for inference\.

### Train with RecordIO Format<a name="IC-recordio-training"></a>

If you use the RecordIO format for training, specify both `train` and `validation` channels as values for the `InputDataConfig` parameter of the [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) request\. Specify one RecordIO \(`.rec`\) file in the `train` channel and one RecordIO file in the `validation` channel\. Set the content type for both channels to `application/x-recordio`\. 

### Train with Image Format<a name="IC-image-training"></a>

If you use the Image format for training, specify `train`, `validation`, `train_lst`, and `validation_lst` channels as values for the `InputDataConfig` parameter of the [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) request\. Specify the individual image data \(`.jpg` or `.png` files\) for the `train` and `validation` channels\. Specify one `.lst` file in each of the `train_lst` and `validation_lst` channels\. Set the content type for all four channels to `application/x-image`\. 

**Note**  
SageMaker reads the training and validation data separately from different channels, so you must store the training and validation data in different folders\.

A `.lst` file is a tab\-separated file with three columns that contains a list of image files\. The first column specifies the image index, the second column specifies the class label index for the image, and the third column specifies the relative path of the image file\. The image index in the first column must be unique across all of the images\. The set of class label indices are numbered successively and the numbering should start with 0\. For example, 0 for the cat class, 1 for the dog class, and so on for additional classes\. 

 The following is an example of a `.lst` file: 

```
5      1   your_image_directory/train_img_dog1.jpg
1000   0   your_image_directory/train_img_cat1.jpg
22     1   your_image_directory/train_img_dog2.jpg
```

For example, if your training images are stored in `s3://<your_bucket>/train/class_dog`, `s3://<your_bucket>/train/class_cat`, and so on, specify the path for your `train` channel as `s3://<your_bucket>/train`, which is the top\-level directory for your data\. In the `.lst` file, specify the relative path for an individual file named `train_image_dog1.jpg` in the `class_dog` class directory as `class_dog/train_image_dog1.jpg`\. You can also store all your image files under one subdirectory inside the `train` directory\. In that case, use that subdirectory for the relative path\. For example, `s3://<your_bucket>/train/your_image_directory`\. 

### Train with Augmented Manifest Image Format<a name="IC-augmented-manifest-training"></a>

The augmented manifest format enables you to do training in Pipe mode using image files without needing to create RecordIO files\. You need to specify both train and validation channels as values for the `InputDataConfig` parameter of the [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) request\. While using the format, an S3 manifest file needs to be generated that contains the list of images and their corresponding annotations\. The manifest file format should be in [JSON Lines](http://jsonlines.org/) format in which each line represents one sample\. The images are specified using the `'source-ref'` tag that points to the S3 location of the image\. The annotations are provided under the `"AttributeNames"` parameter value as specified in the [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) request\. It can also contain additional metadata under the `metadata` tag, but these are ignored by the algorithm\. In the following example, the `"AttributeNames"` are contained in the list of image and annotation references `["source-ref", "class"]`\. The corresponding label value is `"0"` for the first image and `“1”` for the second image:

```
{"source-ref":"s3://image/filename1.jpg", "class":"0"}
{"source-ref":"s3://image/filename2.jpg", "class":"1", "class-metadata": {"class-name": "cat", "type" : "groundtruth/image-classification"}}
```

The order of `"AttributeNames"` in the input files matters when training the ImageClassification algorithm\. It accepts piped data in a specific order, with `image` first, followed by `label`\. So the "AttributeNames" in this example are provided with `"source-ref"` first, followed by `"class"`\. When using the ImageClassification algorithm with Augmented Manifest, the value of the `RecordWrapperType` parameter must be `"RecordIO"`\.

Multi\-label training is also supported by specifying a JSON array of values\. The `num_classes` hyperparameter must be set to match the total number of classes\. There are two valid label formats: multi\-hot and class\-id\. 

In the multi\-hot format, each label is a multi\-hot encoded vector of all classes, where each class takes the value of 0 or 1\. In the following example, there are three classes\. The first image is labeled with classes 0 and 2, while the second image is labeled with class 2 only: 

```
{"image-ref": "s3://mybucket/sample01/image1.jpg", "class": [1, 0, 1]}
{"image-ref": "s3://mybucket/sample02/image2.jpg", "class": [0, 0, 1]}
```

In the class\-id format, each label is a list of the class ids, from \[0, `num_classes`\), which apply to the data point\. The previous example would instead look like this:

```
{"image-ref": "s3://mybucket/sample01/image1.jpg", "class": [0, 2]}
{"image-ref": "s3://mybucket/sample02/image2.jpg", "class": [2]}
```

The multi\-hot format is the default, but can be explicitly set in the content type with the `label-format` parameter: `"application/x-recordio; label-format=multi-hot".` The class\-id format, which is the format outputted by GroundTruth, must be set explicitly: `"application/x-recordio; label-format=class-id".`

For more information on augmented manifest files, see [Provide Dataset Metadata to Training Jobs with an Augmented Manifest File](augmented-manifest.md)\.

### Incremental Training<a name="IC-incremental-training"></a>

You can also seed the training of a new model with the artifacts from a model that you trained previously with SageMaker\. Incremental training saves training time when you want to train a new model with the same or similar data\. SageMaker image classification models can be seeded only with another build\-in image classification model trained in SageMaker\.

To use a pretrained model, in the [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) request, specify the `ChannelName` as "model" in the `InputDataConfig` parameter\. Set the `ContentType` for the model channel to `application/x-sagemaker-model`\. The input hyperparameters of both the new model and the pretrained model that you upload to the model channel must have the same settings for the `num_layers`, `image_shape` and `num_classes` input parameters\. These parameters define the network architecture\. For the pretrained model file, use the compressed model artifacts \(in \.tar\.gz format\) output by SageMaker\. You can use either RecordIO or image formats for input data\.

For a sample notebook that shows how to use incremental training with the SageMaker image classification algorithm, see the [End\-to\-End Incremental Training Image Classification Example](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/imageclassification_caltech/Image-classification-incremental-training-highlevel.ipynb)\. For more information on incremental training and for instructions on how to use it, see [Incremental Training in Amazon SageMaker](incremental-training.md)\. 

### Inference with the Image Classification Algorithm<a name="IC-inference"></a>

The generated models can be hosted for inference and support encoded `.jpg` and `.png` image formats as `image/png, image/jpeg`, and `application/x-image` content\-type\. The input image is resized automatically\. The output is the probability values for all classes encoded in JSON format, or in [JSON Lines text format](http://jsonlines.org/) for batch transform\. The image classification model processes a single image per request and so outputs only one line in the JSON or JSON Lines format\. The following is an example of a response in JSON Lines format:

```
accept: application/jsonlines

 {"prediction": [prob_0, prob_1, prob_2, prob_3, ...]}
```

For more details on training and inference, see the image classification sample notebook instances referenced in the introduction\.

## EC2 Instance Recommendation for the Image Classification Algorithm<a name="IC-instances"></a>

For image classification, we support the following GPU instances for training: `ml.p2.xlarge`, `ml.p2.8xlarge`, `ml.p2.16xlarge`, `ml.p3.2xlarge`, `ml.p3.8xlarge`and `ml.p3.16xlarge`\. We recommend using GPU instances with more memory for training with large batch sizes\. However, both CPU \(such as C4\) and GPU \(such as P2 and P3\) instances can be used for the inference\. You can also run the algorithm on multi\-GPU and multi\-machine settings for distributed training\.

Both P2 and P3 instances are supported in the image classification algorithm\.

## Image Classification Sample Notebooks<a name="IC-sample-notebooks"></a>

For a sample notebook that uses the SageMaker image classification algorithm to train a model on the [caltech\-256 dataset](http://www.vision.caltech.edu/Image_Datasets/Caltech256/) and then to deploy it to perform inferences, see the [End\-to\-End Multiclass Image Classification Example](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/imageclassification_caltech/Image-classification-fulltraining.ipynb)\. For instructions how to create and access Jupyter notebook instances that you can use to run the example in SageMaker, see [Use Amazon SageMaker Notebook Instances](nbi.md)\. Once you have created a notebook instance and opened it, select the **SageMaker Examples** tab to see a list of all the SageMaker samples\. The example image classification notebooks are located in the **Introduction to Amazon algorithms** section\. To open a notebook, click on its **Use** tab and select **Create copy**\.