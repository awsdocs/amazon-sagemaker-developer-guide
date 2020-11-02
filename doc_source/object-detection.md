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

The SageMaker Object Detection algorithm supports both RecordIO \(`application/x-recordio`\) and image \(`image/png`, `image/jpeg`, and `application/x-image`\) content types for training in file mode and supports RecordIO \(`application/x-recordio`\) for training in pipe mode\. However you can also train in pipe mode using the image files \(`image/png`, `image/jpeg`, and `application/x-image`\), without creating RecordIO files, by using the augmented manifest format\. The recommended input format for the Amazon SageMaker object detection algorithms is [Apache MXNet RecordIO](https://mxnet.apache.org/api/architecture/note_data_loading)\. However, you can also use raw images in \.jpg or \.png format\. The algorithm supports only `application/x-image` for inference\.

**Note**  
To maintain better interoperability with existing deep learning frameworks, this differs from the protobuf data formats commonly used by other Amazon SageMaker algorithms\.

See the [Object Detection Sample Notebooks](#object-detection-sample-notebooks) for more details on data formats\.

### Train with the RecordIO Format<a name="object-detection-recordio-training"></a>

If you use the RecordIO format for training, specify both train and validation channels as values for the `InputDataConfig` parameter of the [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) request\. Specify one RecordIO \(\.rec\) file in the train channel and one RecordIO file in the validation channel\. Set the content type for both channels to `application/x-recordio`\. An example of how to generate RecordIO file can be found in the object detection sample notebook\. You can also use tools from the [MXNet's GluonCV](https://gluon-cv.mxnet.io/build/examples_datasets/recordio.html) to generate RecordIO files for popular datasets like the [PASCAL Visual Object Classes](http://host.robots.ox.ac.uk/pascal/VOC/) and [Common Objects in Context \(COCO\)](http://cocodataset.org/#home)\.

### Train with the Image Format<a name="object-detection-image-training"></a>

If you use the image format for training, specify `train`, `validation`, `train_annotation`, and `validation_annotation` channels as values for the `InputDataConfig` parameter of [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) request\. Specify the individual image data \(\.jpg or \.png\) files for the train and validation channels\. For annotation data, you can use the JSON format\. Specify the corresponding \.json files in the `train_annotation` and `validation_annotation` channels\. Set the content type for all four channels to `image/png` or `image/jpeg` based on the image type\. You can also use the content type `application/x-image` when your dataset contains both \.jpg and \.png images\. The following is an example of a \.json file\.

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

The augmented manifest format enables you to do training in pipe mode using image files without needing to create RecordIO files\. You need to specify both train and validation channels as values for the `InputDataConfig` parameter of the [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) request\. While using the format, an S3 manifest file needs to be generated that contains the list of images and their corresponding annotations\. The manifest file format should be in [JSON Lines](http://jsonlines.org/) format in which each line represents one sample\. The images are specified using the `'source-ref'` tag that points to the S3 location of the image\. The annotations are provided under the `"AttributeNames"` parameter value as specified in the [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) request\. It can also contain additional metadata under the `metadata` tag, but these are ignored by the algorithm\. In the following example, the `"AttributeNames` are contained in the list `["source-ref", "bounding-box"]`:

```
{"source-ref": "s3://your_bucket/image1.jpg", "bounding-box":{"image_size":[{ "width": 500, "height": 400, "depth":3}], "annotations":[{"class_id": 0, "left": 111, "top": 134, "width": 61, "height": 128}, {"class_id": 5, "left": 161, "top": 250, "width": 80, "height": 50}]}, "bounding-box-metadata":{"class-map":{"0": "dog", "5": "horse"}, "type": "groundtruth/object-detection"}}
{"source-ref": "s3://your_bucket/image2.jpg", "bounding-box":{"image_size":[{ "width": 400, "height": 300, "depth":3}], "annotations":[{"class_id": 1, "left": 100, "top": 120, "width": 43, "height": 78}]}, "bounding-box-metadata":{"class-map":{"1": "cat"}, "type": "groundtruth/object-detection"}}
```

The order of `"AttributeNames"` in the input files matters when training the Object Detection algorithm\. It accepts piped data in a specific order, with `image` first, followed by `annotations`\. So the "AttributeNames" in this example are provided with `"source-ref"` first, followed by `"bounding-box"`\. When using Object Detection with Augmented Manifest, the value of parameter `RecordWrapperType` must be set as `"RecordIO"`\.

For more information on augmented manifest files, see [Provide Dataset Metadata to Training Jobs with an Augmented Manifest File](augmented-manifest.md)\.

### Incremental Training<a name="object-detection-incremental-training"></a>

You can also seed the training of a new model with the artifacts from a model that you trained previously with SageMaker\. Incremental training saves training time when you want to train a new model with the same or similar data\. SageMaker object detection models can be seeded only with another built\-in object detection model trained in SageMaker\.

To use a pretrained model, in the [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) request, specify the `ChannelName` as "model" in the `InputDataConfig` parameter\. Set the `ContentType` for the model channel to `application/x-sagemaker-model`\. The input hyperparameters of both the new model and the pretrained model that you upload to the model channel must have the same settings for the `base_network` and `num_classes` input parameters\. These parameters define the network architecture\. For the pretrained model file, use the compressed model artifacts \(in \.tar\.gz format\) output by SageMaker\. You can use either RecordIO or image formats for input data\.

For a sample notebook that shows how to use incremental training with the SageMaker object detection algorithm, see [SageMaker Object Detection Incremental Training](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/object_detection_pascalvoc_coco/object_detection_incremental_training.ipynb) sample notebook\. For more information on incremental training and for instructions on how to use it, see [Incremental Training in Amazon SageMaker](incremental-training.md)\. 

## EC2 Instance Recommendation for the Object Detection Algorithm<a name="object-detection-instances"></a>

For object detection, we support the following GPU instances for training: `ml.p2.xlarge`, `ml.p2.8xlarge`, `ml.p2.16xlarge`, `ml.p3.2xlarge`, `ml.p3.8xlarge` and `ml.p3.16xlarge`\. We recommend using GPU instances with more memory for training with large batch sizes\. You can also run the algorithm on multi\-GPU and multi\-machine settings for distributed training\. However, both CPU \(such as C5 and M5\) and GPU \(such as P2 and P3\) instances can be used for the inference\. All the supported instance types for inference are itemized on [Amazon SageMaker ML Instance Types](https://aws.amazon.com/sagemaker/pricing/instance-types/)\.

## Object Detection Sample Notebooks<a name="object-detection-sample-notebooks"></a>

For a sample notebook that shows how to use the SageMaker Object Detection algorithm to train and host a model on the COCO dataset using the Single Shot multibox Detector algorithm, see [Object Detection using the Image and JSON format](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/object_detection_pascalvoc_coco/object_detection_image_json_format.ipynb)\. For instructions how to create and access Jupyter notebook instances that you can use to run the example in SageMaker, see [Use Amazon SageMaker Notebook Instances](nbi.md)\. Once you have created a notebook instance and opened it, select the **SageMaker Examples** tab to see a list of all the SageMaker samples\. The object detection example notebook using the Object Detection algorithm is located in the **Introduction to Amazon Algorithms** section\. To open a notebook, click on its **Use** tab and select **Create copy**\.