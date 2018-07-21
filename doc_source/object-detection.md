# Object Detection Algorithm<a name="object-detection"></a>

The Amazon SageMaker Object Detection algorithm detects and classifies objects in images using a single deep neural network\. It is a supervised learning algorithm that takes images as input and identifies all instances of objects within the image scene\. The object is categorized into one of the classes in a specified collection with a confidence score that it belongs to the class\. Its location and scale in the image are indicated by a rectangular bounding box\. It uses the [Single Shot multibox Detector \(SSD\)](https://arxiv.org/pdf/1512.02325.pdf) framework and supports two base networks: [VGG](https://arxiv.org/pdf/1409.1556.pdf) and [ResNet](https://arxiv.org/pdf/1603.05027.pdf)\. The network can be trained from scratch, or trained with models that have been pre\-trained on the [ImageNet](http://www.image-net.org/) dataset\.

## Input/Output Interface<a name="object-detection-inputoutput"></a>

The Amazon SageMaker Object Detection algorithm supports both RecordIO \(`application/x-recordio`\) and image \(`image/png`,`image/jpeg`, and `application/x-image`\) content types for training\. The algorithm supports only image format for inference\. The recommended input format for the Amazon SageMaker object detection algorithms is [Apache MXnet RecordIO](https://mxnet.incubator.apache.org/architecture/note_data_loading.html)\. However, you can also use raw images in \.jpg or \.png format\. 

**Note**  
To maintain better interoperability with existing deep learning frameworks, this differs from the protobuf data formats commonly used by other Amazon SageMaker algorithms\.

See the example notebooks for more details on data formats\.

### Training with RecordIO Format<a name="object-detection-recordio-training"></a>

If you use the RecordIO format for training, specify both train and validation channels as values for the `InputDataConfig` parameter of the [CreateTrainingJob](API_CreateTrainingJob.md) request\. Specify one RecordIO \(\.rec\) file in the train channel and one RecordIO file in the validation channel\. Set the content type for both channels to `application/x-recordio`\. An example of how to generate RecordIO file can be found in the object detection sample notebook\. You can also use tools from the [MXnet](https://github.com/apache/incubator-mxnet/tree/master/example/ssd) example to generate RecordIO files for popular datasets like the [PASCAL Visual Object Classes](http://host.robots.ox.ac.uk/pascal/VOC/) and [Common Objects in Context \(COCO\)](http://cocodataset.org/#home)\.

### Training with Image Format<a name="object-detection-image-training"></a>

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

## EC2 Instance Recommendation<a name="object-detection-instances"></a>

For object detection, we support the following GPU instances for training: `ml.p2.xlarge`, `ml.p2.8xlarge`, `ml.p2.16xlarge`, `ml.p3.2xlarge`, `ml.p3.8xlarge` and `ml.p3.16xlarge`\. We recommend using GPU instances with more memory for training with large batch sizes\. You can also run the algorithm on multi\-GPU and multi\-machine settings for distributed training\. However, both CPU \(such as C5 and M5\) and GPU \(such as P2 and P3\) instances can be used for the inference\. All the supported instance types for inference are itemized on [Amazon SageMaker ML Instance Types](https://aws.amazon.com/sagemaker/pricing/instance-types/)\.

**Topics**
+ [Input/Output Interface](#object-detection-inputoutput)
+ [EC2 Instance Recommendation](#object-detection-instances)
+ [How Object Detection Works](algo-object-detection-tech-notes.md)
+ [Object Detection Hyperparameters](object-detection-api-config.md)
+ [Tuning an Object Detection Model](object-detection-tuning.md)
+ [Object Detection Request and Response Formats](object-detection-in-formats.md)