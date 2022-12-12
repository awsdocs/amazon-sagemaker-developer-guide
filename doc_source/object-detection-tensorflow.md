# Object Detection \- TensorFlow<a name="object-detection-tensorflow"></a>

The Amazon SageMaker Object Detection \- TensorFlow algorithm is a supervised learning algorithm that supports transfer learning with many pretrained models from the [TensorFlow Model Garden](https://github.com/tensorflow/models)\. Use transfer learning to fine\-tune one of the available pretrained models on your own dataset, even if a large amount of image data is not available\. The object detection algorithm takes an image as input and outputs a list of bounding boxes\. Training datasets must consist of images in \.`jpg`, `.jpeg`, or `.png` format\.

**Topics**
+ [How to use the SageMaker Object Detection \- TensorFlow algorithm](#object-detection-tensorflow-how-to-use)
+ [Input and output interface for the Object Detection \- TensorFlow algorithm](#object-detection-tensorflow-inputoutput)
+ [Amazon EC2 instance recommendation for the Object Detection \- TensorFlow algorithm](#object-detection-tensorflow-instances)
+ [Object Detection \- TensorFlow sample notebooks](#object-detection-tensorflow-sample-notebooks)
+ [How Object Detection \- TensorFlow Works](object-detection-tensorflow-HowItWorks.md)
+ [TensorFlow Models](object-detection-tensorflow-Models.md)
+ [Object Detection \- TensorFlow Hyperparameters](object-detection-tensorflow-Hyperparameter.md)
+ [Tune an Object Detection \- TensorFlow model](object-detection-tensorflow-tuning.md)

## How to use the SageMaker Object Detection \- TensorFlow algorithm<a name="object-detection-tensorflow-how-to-use"></a>

You can use Object Detection \- TensorFlow as an Amazon SageMaker built\-in algorithm\. The following section describes how to use Object Detection \- TensorFlow with the SageMaker Python SDK\. For information on how to use Object Detection \- TensorFlow from the Amazon SageMaker Studio UI, see [SageMaker JumpStart](studio-jumpstart.md)\.

The Object Detection \- TensorFlow algorithm supports transfer learning using any of the compatible pretrained TensorFlow models\. For a list of all available pretrained models, see [TensorFlow Models](object-detection-tensorflow-Models.md)\. Every pretrained model has a unique `model_id`\. The following example uses ResNet50 \(`model_id`: `tensorflow-od1-ssd-resnet50-v1-fpn-640x640-coco17-tpu-8`\) to fine\-tune on a custom dataset\. The pretrained models are all pre\-downloaded from the TensorFlow Hub and stored in Amazon S3 buckets so that training jobs can run in network isolation\. Use these pre\-generated model training artifacts to construct a SageMaker Estimator\.

First, retrieve the Docker image URI, training script URI, and pretrained model URI\. Then, change the hyperparameters as you see fit\. You can see a Python dictionary of all available hyperparameters and their default values with `hyperparameters.retrieve_default`\. For more information, see [Object Detection \- TensorFlow Hyperparameters](object-detection-tensorflow-Hyperparameter.md)\. Use these values to construct a SageMaker Estimator\.

**Note**  
Default hyperparameter values are different for different models\. For example, for larger models, the default number of epochs is smaller\. 

This example uses the [https://www.cis.upenn.edu/~jshi/ped_html/#pub1](https://www.cis.upenn.edu/~jshi/ped_html/#pub1) dataset, which contains images of pedestriants in the street\. We pre\-downloaded the dataset and made it available with Amazon S3\. To fine\-tune your model, call `.fit` using the Amazon S3 location of your training dataset\.

```
from sagemaker import image_uris, model_uris, script_uris, hyperparameters
from sagemaker.estimator import Estimator

model_id, model_version = "tensorflow-od1-ssd-resnet50-v1-fpn-640x640-coco17-tpu-8", "*"
training_instance_type = "ml.p3.2xlarge"

# Retrieve the Docker image
train_image_uri = image_uris.retrieve(model_id=model_id,model_version=model_version,image_scope="training",instance_type=training_instance_type,region=None,framework=None)

# Retrieve the training script
train_source_uri = script_uris.retrieve(model_id=model_id, model_version=model_version, script_scope="training")

# Retrieve the pretrained model tarball for transfer learning
train_model_uri = model_uris.retrieve(model_id=model_id, model_version=model_version, model_scope="training")

# Retrieve the default hyperparameters for fine-tuning the model
hyperparameters = hyperparameters.retrieve_default(model_id=model_id, model_version=model_version)

# [Optional] Override default hyperparameters with custom values
hyperparameters["epochs"] = "5"

# Sample training data is available in this bucket
training_data_bucket = f"jumpstart-cache-prod-{aws_region}"
training_data_prefix = "training-datasets/PennFudanPed_COCO_format/"

training_dataset_s3_path = f"s3://{training_data_bucket}/{training_data_prefix}"

output_bucket = sess.default_bucket()
output_prefix = "jumpstart-example-od-training"
s3_output_location = f"s3://{output_bucket}/{output_prefix}/output"

# Create an Estimator instance
tf_od_estimator = Estimator(
    role=aws_role,
    image_uri=train_image_uri,
    source_dir=train_source_uri,
    model_uri=train_model_uri,
    entry_point="transfer_learning.py",
    instance_count=1,
    instance_type=training_instance_type,
    max_run=360000,
    hyperparameters=hyperparameters,
    output_path=s3_output_location,
)

# Launch a training job
tf_od_estimator.fit({"training": training_dataset_s3_path}, logs=True)
```

For more information about how to use the SageMaker Object Detection \- TensorFlow algorithm for transfer learning on a custom dataset, see the [Introduction to SageMaker TensorFlow \- Object Detection](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/object_detection_tensorflow/Amazon_Tensorflow_Object_Detection.ipynb) notebook\.

## Input and output interface for the Object Detection \- TensorFlow algorithm<a name="object-detection-tensorflow-inputoutput"></a>

Each of the pretrained models listed in TensorFlow Models can be fine\-tuned to any dataset with any number of image classes\. Be mindful of how to format your training data for input to the Object Detection \- TensorFlow model\.
+ **Training data input format:** Your training data should be a directory with an `images` subdirectory and an `annotations.json` file\. 

The following is an example of an input directory structure\. The input directory should be hosted in an Amazon S3 bucket with a path similar to the following: `s3://bucket_name/input_directory/`\. Note that the trailing `/` is required\.

```
input_directory
    |--images
        |--abc.png
        |--def.png
    |--annotations.json
```

The `annotations.json` file should contain information for bounding boxes and their class labels in the form of a dictionary `"images"` and `"annotations"` keys\. The value for the `"images"` key should be a list of dictionaries\. There should be one dictionary for each image with the following information: `{"file_name": image_name, "height": height, "width": width, "id": image_id}`\. The value for the `"annotations"` key should also be a list of dictionaries\. There should be one dictionary for each bounding box with the following information: `{"image_id": image_id, "bbox": [xmin, ymin, xmax, ymax], "category_id": bbox_label}`\.

After training, a label mapping file and trained model are saved to your Amazon S3 bucket\.

### Incremental training<a name="object-detection-tensorflow-incremental-training"></a>

You can seed the training of a new model with artifacts from a model that you trained previously with SageMaker\. Incremental training saves training time when you want to train a new model with the same or similar data\.

**Note**  
You can only seed a SageMaker Object Detection \- TensorFlow model with another Object Detection \- TensorFlow model trained in SageMaker\. 

You can use any dataset for incremental training, as long as the set of classes remains the same\. The incremental training step is similar to the fine\-tuning step, but instead of starting with a pretrained model, you start with an existing fine\-tuned model\. For more information about how to use incremental training with the SageMaker Object Detection \- TensorFlow, see the [Introduction to SageMaker TensorFlow \- Object Detection](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/object_detection_tensorflow/Amazon_Tensorflow_Object_Detection.ipynb) notebook\.

### Inference with the Object Detection \- TensorFlow algorithm<a name="object-detection-tensorflow-inference"></a>

You can host the fine\-tuned model that results from your TensorFlow Object Detection training for inference\. Any input image for inference must be in `.jpg`, \.`jpeg`, or `.png` format and be content type `application/x-image`\. The Object Detection \- TensorFlow algorithm resizes input images automatically\. 

Running inference results in bounding boxes, predicted classes, and the scores of each prediction encoded in JSON format\. The Object Detection \- TensorFlow model processes a single image per request and outputs only one line\. The following is an example of a JSON format response:

```
accept: application/json;verbose

{"normalized_boxes":[[xmin1, xmax1, ymin1, ymax1],....], 
    "classes":[classidx1, class_idx2,...], 
    "scores":[score_1, score_2,...], 
    "labels": [label1, label2, ...], 
    "tensorflow_model_output":<original output of the model>}
```

If `accept` is set to `application/json`, then the model only outputs normalized boxes, classes, and scores\. 

## Amazon EC2 instance recommendation for the Object Detection \- TensorFlow algorithm<a name="object-detection-tensorflow-instances"></a>

The Object Detection \- TensorFlow algorithm supports all GPU instances for training, including:
+ `ml.p2.xlarge`
+ `ml.p2.16xlarge`
+ `ml.p3.2xlarge`
+ `ml.p3.16xlarge`

We recommend GPU instances with more memory for training with large batch sizes\. Both CPU \(such as M5\) and GPU \(P2 or P3\) instances can be used for inference\. For a comprehensive list of SageMaker training and inference instances across AWS Regions, see [Amazon SageMaker Pricing](http://aws.amazon.com/sagemaker/pricing/)\.

## Object Detection \- TensorFlow sample notebooks<a name="object-detection-tensorflow-sample-notebooks"></a>

For more information about how to use the SageMaker Object Detection \- TensorFlow algorithm for transfer learning on a custom dataset, see the [Introduction to SageMaker TensorFlow \- Object Detection](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/object_detection_tensorflow/Amazon_Tensorflow_Object_Detection.ipynb) notebook\.

For instructions how to create and access Jupyter notebook instances that you can use to run the example in SageMaker, see [Use Amazon SageMaker Notebook Instances](nbi.md)\. After you have created a notebook instance and opened it, select the **SageMaker Examples** tab to see a list of all the SageMaker samples\. To open a notebook, choose its **Use** tab and choose **Create copy**\.