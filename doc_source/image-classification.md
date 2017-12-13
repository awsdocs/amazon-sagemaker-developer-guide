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

The data needed for training has to be specified in two channels: train and validation for recordio format as part of the `CreateTrainingJob` API parameters\. The content\-type for the train and validation channels should be `application/x-recordio` for recordIO files\.

When the training data is provided as individual files \(content\-type is `application/x-image`\), the individual image data should be in the train and the validation subfolders, as specified for the recordIO file\. You also need to specify two additional channels, `train_lst` and `validation_lst`, in case of individual images\. These additional channels specify the path for the first file for training and validation data respectively\. 

A sample first file follows\. The first column is the image index, followed by the label index for the image, and then the image's path\. The three entries are separated by tabs\. 

```
5      2   your_image_directory/train_img_dog1.jpg
1000   1   your_image_directory/train_img_cat1.jpg
22     2   your_image_directory/train_img_dog2.jpg
```

The generated models can be hosted for inference and supports encoded \.jpg image formats as `application/x-image` content\-type\. The output is the probability values for all classes encoded in JSON format\.

For more details on training and inference, see the image classification sample notebook instances\.

## EC2 Instance Recommendation<a name="IC-instances"></a>

For image classification, we recommend GPU instances for training\. However, both CPU and GPU instances can be used for the inference\. The following instances would be recommended for training: `ml.p2.xlarge`, `ml.p2.8xlarge`, and `ml.p2.16xlarge`\. We recommend using GPU instances with more memory for training with large batch size\. For hosting, C4 CPU instances and P2 GPU instances can be used\. You can also run the algorithm on multi\-GPU and multi\-machine settings for distributed training\.


+ [Input/Output Interface](#IC-inputoutput)
+ [EC2 Instance Recommendation](#IC-instances)
+ [How Image Classification Works](IC-HowItWorks.md)
+ [Hyperparameters](IC-Hyperparameter.md)