# Semantic Segmentation Algorithm<a name="semantic-segmentation"></a>

The SageMaker semantic segmentation algorithm provides a fine\-grained, pixel\-level approach to developing computer vision applications\. It tags every pixel in an image with a class label from a predefined set of classes\. Tagging is fundamental for understanding scenes, which is critical to an increasing number of computer vision applications, such as self\-driving vehicles, medical imaging diagnostics, and robot sensing\. 

For comparison, the SageMaker [Image Classification Algorithm](image-classification.md) is a supervised learning algorithm that analyzes only whole images, classifying them into one of multiple output categories\. The [Object Detection Algorithm](object-detection.md) is a supervised learning algorithm that detects and classifies all instances of an object in an image\. It indicates the location and scale of each object in the image with a rectangular bounding box\. 

Because the semantic segmentation algorithm classifies every pixel in an image, it also provides information about the shapes of the objects contained in the image\. The segmentation output is represented as an RGB or grayscale image, called a *segmentation mask*\. A segmentation mask is an RGB \(or grayscale\) image with the same shape as the input image\.

The SageMaker semantic segmentation algorithm is built using the [MXNet Gluon framework and the Gluon CV toolkit](https://github.com/dmlc/gluon-cv), and provides you with a choice of three build\-in algorithms to train a deep neural network\. You can use the [Fully\-Convolutional Network \(FCN\) algorithm ](https://arxiv.org/abs/1605.06211), [Pyramid Scene Parsing \(PSP\) algorithm](https://arxiv.org/abs/1612.01105), or [DeepLabV3](https://arxiv.org/abs/1706.05587)\. 

Each of the three algorithms has two distinct components: 
+ The *backbone* \(or *encoder*\)—A network that produces reliable activation maps of features\.
+ The *decoder*—A network that constructs the segmentation mask from the encoded activation maps\.

You also have a choice of backbones for the FCN, PSP, and DeepLabV3 algorithms: [ResNet50 or ResNet101](https://arxiv.org/abs/1512.03385)\. These backbones include pretrained artifacts that were originally trained on the [ImageNet](http://www.image-net.org/) classification task\. You can fine\-tune these backbones for segmentation using your own data\. Or, you can initialize and train these networks from scratch using only your own data\. The decoders are never pretrained\. 

To deploy the trained model for inference, use the SageMaker hosting service\. During inference, you can request the segmentation mask either as a PNG image or as a set of probabilities for each class for each pixel\. You can use these masks as part of a larger pipeline that includes additional downstream image processing or other applications\.

**Topics**
+ [Semantic Segmentation Sample Notebooks](#semantic-segmentation-sample-notebooks)
+ [Input/Output Interface for the Semantic Segmentation Algorithm](#semantic-segmentation-inputoutput)
+ [EC2 Instance Recommendation for the Semantic Segmentation Algorithm](#semantic-segmentation-instances)
+ [Semantic Segmentation Hyperparameters](segmentation-hyperparameters.md)

## Semantic Segmentation Sample Notebooks<a name="semantic-segmentation-sample-notebooks"></a>

For a sample Jupyter notebook that uses the SageMaker semantic segmentation algorithm to train a model and deploy it to perform inferences, see the [Semantic Segmentation Example](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/semantic_segmentation_pascalvoc/semantic_segmentation_pascalvoc.ipynb)\. For instructions on how to create and access Jupyter notebook instances that you can use to run the example in SageMaker, see [Use Amazon SageMaker Notebook Instances](nbi.md)\. 

To see a list of all of the SageMaker samples, create and open a notebook instance, and choose the **SageMaker Examples** tab\. The example semantic segmentation notebooks are located under **Introduction to Amazon algorithms**\. To open a notebook, choose its **Use** tab, and choose **Create copy**\.

## Input/Output Interface for the Semantic Segmentation Algorithm<a name="semantic-segmentation-inputoutput"></a>

SageMaker semantic segmentation expects the customer's training dataset to be on [Amazon Simple Storage Service \(Amazon S3\)](https://aws.amazon.com/s3/)\. Once trained, it produces the resulting model artifacts on Amazon S3\. The input interface format for the SageMaker semantic segmentation is similar to that of most standardized semantic segmentation benchmarking datasets\. The dataset in Amazon S3 is expected to be presented in two channels, one for `train` and one for `validation` using four directories, two for images and two for annotations\. Annotations are expected to be uncompressed PNG images\. The dataset might also have a label map that describes how the annotation mappings are established\. If not, the algorithm uses a default\. It also supports the augmented manifest image format \(`application/x-image`\) for training in Pipe input mode straight from Amazon S3\. For inference, an endpoint accepts images with an `image/jpeg` content type\. 

### How Training Works<a name="semantic-segmentation-inputoutput-training"></a>

The training data is split into four directories: `train`, `train_annotation`, `validation`, and `validation_annotation`\. There is a channel for each of these directories\. The dataset also expected to have one `label_map.json` file per channel for `train_annotation` and `validation_annotation` respectively\. If you don't provide these JSON files, SageMaker provides the default set label map\.

The dataset specifying these files should look similar to the following example:

```
s3://bucket_name
    |
    |- train
                 |
                 | - 0000.jpg
                 | - coffee.jpg
    |- validation
                 |
                 | - 00a0.jpg
                 | - bananna.jpg
    |- train_annotation
                 |
                 | - 0000.png
                 | - coffee.png
    |- validation_annotation
                 |
                 | - 00a0.png
                 | - bananna.png
    |- label_map
                 | - train_label_map.json
                 | - validation_label_map.json
```

Every JPG image in the train and validation directories has a corresponding PNG label image with the same name in the `train_annotation` and `validation_annotation` directories\. This naming convention helps the algorithm to associate a label with its corresponding image during training\. The `train`, `train_annotation`, `validation`, and `validation_annotation` channels are mandatory\. The annotations are single\-channel PNG images\. The format works as long as the metadata \(modes\) in the image helps the algorithm read the annotation images into a single\-channel 8\-bit unsigned integer\. For more information on our support for modes, see the [Python Image Library documentation](https://pillow.readthedocs.io/en/3.0.x/handbook/concepts.html#modes)\. We recommend using the 8\-bit pixel, true color `P` mode\. 

The image that is encoded is a simple 8\-bit integer when using modes\. To get from this mapping to a map of a label, the algorithm uses one mapping file per channel, called the *label map*\. The label map is used to map the values in the image with actual label indices\. In the default label map, which is provided by default if you don’t provide one, the pixel value in an annotation matrix \(image\) directly index the label\. These images can be grayscale PNG files or 8\-bit indexed PNG files\. The label map file for the unscaled default case is the following: 

```
{
  "scale": "1"
}
```

To provide some contrast for viewing, some annotation software scales the label images by a constant amount\. To support this, the SageMaker semantic segmentation algorithm provides a rescaling option to scale down the values to actual label values\. When scaling down doesn’t convert the value to an appropriate integer, the algorithm defaults to the greatest integer less than or equal to the scale value\. The following code shows how to set the scale value to rescale the label values:

```
{
  "scale": "3"
}
```

The following example shows how this `"scale"` value is used to rescale the `encoded_label` values of the input annotation image when they are mapped to the `mapped_label` values to be used in training\. The label values in the input annotation image are 0, 3, 6, with scale 3, so they are mapped to 0, 1, 2 for training:

```
encoded_label = [0, 3, 6]
mapped_label = [0, 1, 2]
```

In some cases, you might need to specify a particular color mapping for each class\. Use the map option in the label mapping as shown in the following example of a `label_map` file:

```
{
    "map": {
        "0": 5,
        "1": 0,
        "2": 2
    }
}
```

This label mapping for this example is:

```
encoded_label = [0, 5, 2]
mapped_label = [1, 0, 2]
```

With label mappings, you can use different annotation systems and annotation software to obtain data without a lot of preprocessing\. You can provide one label map per channel\. The files for a label map in the `label_map` channel must follow the naming conventions for the four directory structure\. If you don't provide a label map, the algorithm assumes a scale of 1 \(the default\)\.

### Training with the Augmented Manifest Format<a name="semantic-segmentation-inputoutput-training-augmented-manifest"></a>

The augmented manifest format enables you to do training in Pipe mode using image files without needing to create RecordIO files\. The augmented manifest file contains data objects and should be in [JSON Lines](http://jsonlines.org/) format, as described in the [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) request\. Each line in the manifest is an entry containing the Amazon S3 URI for the image and the URI for the annotation image\.

Each JSON object in the manifest file must contain a `source-ref` key\. The `source-ref` key should contain the value of the Amazon S3 URI to the image\. The labels are provided under the `AttributeNames` parameter value as specified in the [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) request\. It can also contain additional metadata under the metadata tag, but these are ignored by the algorithm\. In the example below, the `AttributeNames` are contained in the list of image and annotation references `["source-ref", "city-streets-ref"]`\. These names must have `-ref` appended to them\. When using the Semantic Segmentation algorithm with Augmented Manifest, the value of the `RecordWrapperType` parameter must be `"RecordIO"` and value of the `ContentType` parameter must be `application/x-recordio`\.

```
{"source-ref": "S3 bucket location", "city-streets-ref": "S3 bucket location", "city-streets-metadata": {"job-name": "label-city-streets", }}
```

For more information on augmented manifest files, see [Provide Dataset Metadata to Training Jobs with an Augmented Manifest File](augmented-manifest.md)\.

### Incremental Training<a name="semantic-segmentation-inputoutput-incremental-training"></a>

You can also seed the training of a new model with a model that you trained previously using SageMaker\. This incremental training saves training time when you want to train a new model with the same or similar data\. Currently, incremental training is supported only for models trained with the built\-in SageMaker Semantic Segmentation\.

To use your own pre\-trained model, specify the `ChannelName` as "model" in the `InputDataConfig` for the [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) request\. Set the `ContentType` for the model channel to `application/x-sagemaker-model`\. The `backbone`, `algorithm`, `crop_size`, and `num_classes` input parameters that define the network architecture must be consistently specified in the input hyperparameters of the new model and the pre\-trained model that you upload to the model channel\. For the pretrained model file, you can use the compressed \(\.tar\.gz\) artifacts from SageMaker outputs\. You can use either RecordIO or Image formats for input data\. For more information on incremental training and for instructions on how to use it, see [Incremental Training in Amazon SageMaker](incremental-training.md)\. 

### Produce Inferences<a name="semantic-segmentation-inputoutput-inference"></a>

To query a trained model that is deployed to an endpoint, you need to provide an image and an `AcceptType` that denotes the type of output required\. The endpoint takes JPEG images with an `image/jpeg` content type\. If you request an `AcceptType` of `image/png`, the algorithm outputs a PNG file with a segmentation mask in the same format as the labels themselves\. If you request an accept type of`application/x-recordio-protobuf`, the algorithm returns class probabilities encoded in recordio\-protobuf format\. The latter format outputs a 3D tensor where the third dimension is the same size as the number of classes\. This component denotes the probability of each class label for each pixel\.

## EC2 Instance Recommendation for the Semantic Segmentation Algorithm<a name="semantic-segmentation-instances"></a>

The SageMaker semantic segmentation algorithm only supports GPU instances for training, and we recommend using GPU instances with more memory for training with large batch sizes\. The algorithm can be trained using [P2/P3 EC2 Amazon Elastic Compute Cloud \(Amazon EC2\)](https://aws.amazon.com/ec2/) instances in single machine configurations\. It supports the following GPU instances for training:
+ `ml.p2.xlarge`
+ `ml.p2.8xlarge`
+ `ml.p2.16xlarge`
+ `ml.p3.2xlarge`
+ `ml.p3.8xlarge`
+ `ml.p3.16xlarge`

For inference, you can use either CPU instances \(such as c5 and m5\) and GPU instances \(such as p2 and p3\) or both\. For information about the instance types that provide varying combinations of CPU, GPU, memory, and networking capacity for inference, see [Amazon SageMaker ML Instance Types](https://aws.amazon.com/sagemaker/pricing/instance-types/)\.