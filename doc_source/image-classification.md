# Image Classification Algorithm<a name="image-classification"></a>

The Amazon SageMaker image classification algorithm is a supervised learning algorithm that takes an image as input and classifies it into one of multiple output categories\. It uses a convolutional neural network \(ResNet\) that can be trained from scratch, or trained using transfer learning when a large number of training images are not available\.

The recommended input format for the Amazon SageMaker image classification algorithms is Apache MXNet [RecordIO](https://mxnet.incubator.apache.org/architecture/note_data_loading.html)\. However, you can also use raw images in \.jpg or \.png format\.

**Note**  
To maintain better interoperability with existing deep learning frameworks, this differs from the protobuf data formats commonly used by other Amazon SageMaker algorithms\.

For more information on convolutional networks, see: 
+ He, Kaiming, et al\. "Deep residual learning for image recognition\." Proceedings of the IEEE conference on computer vision and pattern recognition\. 2016

  [Imagenet \- Image database](http://www.image-net.org/) 
+ [Image classification in MXNet](https://github.com/apache/incubator-mxnet/tree/master/example/image-classification)

## Input/Output Interface<a name="IC-inputoutput"></a>

The Amazon SageMaker Image Classification algorithm supports both RecordIO \(`application/x-recordio`\) and image \(`application/x-image`\) content types for training\. The algorithm supports only `application/x-image` for inference\.

### Training with RecordIO Format<a name="IC-recordio-training"></a>

 If you use the RecordIO format for training, specify both `train` and `validation` channels as values for the `InputDataConfig` parameter of the [CreateTrainingJob](API_CreateTrainingJob.md) request\. Specify one RecordIO \(`.rec`\) file in the `train` channel and one RecordIO file in the `validation` channel\. Set the content type for both channels to `application/x-recordio`\. 

### Training with Image Format<a name="IC-image-training"></a>

 If you use the Image format for training, specify `train`, `validation`, `train_lst`, and `validation_lst` channels as values for the `InputDataConfig` parameter of the [CreateTrainingJob](API_CreateTrainingJob.md) request\. Specify the individual image data \(`.jpg` or `.png` files\) for the `train` and `validation` channels\. Specify one `.lst` file in each of the `train_lst` and `validation_lst` channels\. Set the content type for all four channels to `application/x-image`\. 

 A `.lst` file is a tab\-separated file with three columns that contains a list of image files\. The first column specifies the image index, the second column specifies the class label index for the image, and the third column specifies the relative path of the image file\. The image index in the first column must be unique across all of the images\. The set of class label indices are numbered successively and the numbering should start with 0\. For example, 0 for the cat class, 1 for the dog class, and so on for additional classes\. 

 The following is an example of a `.lst` file: 

```
5      1   your_image_directory/train_img_dog1.jpg
1000   0   your_image_directory/train_img_cat1.jpg
22     1   your_image_directory/train_img_dog2.jpg
```

 For example, if your training images are stored in `s3://<your_bucket>/train/class_dog`, `s3://<your_bucket>/train/class_cat`, and so on, specify the path for your `train` channel as `s3://<your_bucket>/train`, which is the top\-level directory for your data\. In the `.lst` file, specify the relative path for an individual file named `train_image_dog1.jpg` in the `class_dog` class directory as `class_dog/train_image_dog1.jpg`\. You can also store all your image files under one subdirectory inside the `train` directory\. In that case, use that subdirectory for the relative path\. For example, `s3://<your_bucket>/train/your_image_directory`\. 

### Inference with Image Format<a name="IC-inference"></a>

The generated models can be hosted for inference and support encoded `.jpg` and `.png` image formats as `application/x-image` content\-type\. The output is the probability values for all classes encoded in JSON format\.

For more details on training and inference, see the image classification sample notebook instances\.

## EC2 Instance Recommendation<a name="IC-instances"></a>

 For image classification, we support the following GPU instances for training: `ml.p2.xlarge`, `ml.p2.8xlarge`, `ml.p2.16xlarge`, `ml.p3.2xlarge`, `ml.p3.8xlarge`and `ml.p3.16xlarge`\. We recommend using GPU instances with more memory for training with large batch sizes\. However, both CPU \(such as C4\) and GPU \(such as P2 and P3\) instances can be used for the inference\. You can also run the algorithm on multi\-GPU and multi\-machine settings for distributed training\.

Both P2 and P3 instances are supported in the image classification algorithm\.

**Topics**
+ [Input/Output Interface](#IC-inputoutput)
+ [EC2 Instance Recommendation](#IC-instances)
+ [How Image Classification Works](IC-HowItWorks.md)
+ [Hyperparameters](IC-Hyperparameter.md)