# Image Classification \- TensorFlow<a name="image-classification-tensorflow"></a>

The Amazon SageMaker Image Classification \- TensorFlow algorithm is a supervised learning algorithm that supports transfer learning with many pretrained models from the [TensorFlow Hub](https://tfhub.dev/s?fine-tunable=yes&module-type=image-classification&subtype=module,placeholder&tf-version=tf2)\. Use transfer learning to fine\-tune one of the available pretrained models on your own dataset, even if a large amount of image data is not available\. The image classification algorithm takes an image as input and outputs a probability for each provided class label\. Training datasets must consist of images in \.jpg, \.jpeg, or \.png format\.

**Topics**
+ [How to use the SageMaker Image Classification \- TensorFlow algorithm](#IC-TF-how-to-use)
+ [Input and output interface for the Image Classification \- TensorFlow algorithm](#IC-TF-inputoutput)
+ [Amazon EC2 instance recommendation for the Image Classification \- TensorFlow algorithm](#IC-TF-instances)
+ [Image Classification \- TensorFlow sample notebooks](#IC-TF-sample-notebooks)
+ [How Image Classification \- TensorFlow Works](IC-TF-HowItWorks.md)
+ [TensorFlow Hub Models](IC-TF-Models.md)
+ [Image Classification \- TensorFlow Hyperparameters](IC-TF-Hyperparameter.md)
+ [Tune an Image Classification \- TensorFlow model](IC-TF-tuning.md)

## How to use the SageMaker Image Classification \- TensorFlow algorithm<a name="IC-TF-how-to-use"></a>

You can use Image Classification \- TensorFlow as an Amazon SageMaker built\-in algorithm\. The following section describes how to use Image Classification \- TensorFlow with the SageMaker Python SDK\. For information on how to use Image Classification \- TensorFlow from the Amazon SageMaker Studio UI, see [SageMaker JumpStart](studio-jumpstart.md)\.

The Image Classification \- TensorFlow algorithm supports transfer learning using any of the compatible pretrained TensorFlow Hub models\. For a list of all available pretrained models, see [TensorFlow Hub Models](IC-TF-Models.md)\. Every pretrained model has a unique `model_id`\. The following example uses MobileNet V2 1\.00 224 \(`model_id`: `tensorflow-ic-imagenet-mobilenet-v2-100-224-classification-4`\) to fine\-tune on a custom dataset\. The pretrained models are all pre\-downloaded from the TensorFlow Hub and stored in Amazon S3 buckets so that training jobs can run in network isolation\. Use these pre\-generated model training artifacts to construct a SageMaker Estimator\.

First, retrieve the Docker image URI, training script URI, and pretrained model URI\. Then, change the hyperparameters as you see fit\. You can see a Python dictionary of all available hyperparameters and their default values with `hyperparameters.retrieve_default`\. For more information, see [Image Classification \- TensorFlow Hyperparameters](IC-TF-Hyperparameter.md)\. Use these values to construct a SageMaker Estimator\.

**Note**  
Default hyperparameter values are different for different models\. For larger models, the default batch size is smaller and the `train_only_on_top` hyperparameter is set to `"True"`\.

This example uses the [https://www.tensorflow.org/datasets/catalog/tf_flowers](https://www.tensorflow.org/datasets/catalog/tf_flowers) dataset, which contains five classes of flower images\. We pre\-downloaded the dataset from TensorFlow under the Apache 2\.0 license and made it available with Amazon S3\. To fine\-tune your model, call `.fit` using the Amazon S3 location of your training dataset\.

```
from sagemaker import image_uris, model_uris, script_uris, hyperparameters
from sagemaker.estimator import Estimator

model_id, model_version = "tensorflow-ic-imagenet-mobilenet-v2-100-224-classification-4", "*"
training_instance_type = "ml.p3.2xlarge"

# Retrieve the Docker image
train_image_uri = image_uris.retrieve(model_id=model_id,model_version=model_version,image_scope="training",instance_type=training_instance_type,region=None,framework=None)

# Retrieve the training script
train_source_uri = script_uris.retrieve(model_id=model_id, model_version=model_version, script_scope="training")

# Retrieve the pretrained model tarball for transfer learning
train_model_uri = model_uris.retrieve(model_id=model_id, model_version=model_version, model_scope="training")

# Retrieve the default hyper-parameters for fine-tuning the model
hyperparameters = hyperparameters.retrieve_default(model_id=model_id, model_version=model_version)

# [Optional] Override default hyperparameters with custom values
hyperparameters["epochs"] = "5"

# The sample training data is available in the following S3 bucket
training_data_bucket = f"jumpstart-cache-prod-{aws_region}"
training_data_prefix = "training-datasets/tf_flowers/"

training_dataset_s3_path = f"s3://{training_data_bucket}/{training_data_prefix}"

output_bucket = sess.default_bucket()
output_prefix = "jumpstart-example-ic-training"
s3_output_location = f"s3://{output_bucket}/{output_prefix}/output"

# Create SageMaker Estimator instance
tf_ic_estimator = Estimator(
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

# Use S3 path of the training data to launch SageMaker TrainingJob
tf_ic_estimator.fit({"training": training_dataset_s3_path}, logs=True)
```

## Input and output interface for the Image Classification \- TensorFlow algorithm<a name="IC-TF-inputoutput"></a>

Each of the pretrained models listed in TensorFlow Hub Models can be fine\-tuned to any dataset with any number of image classes\. Be mindful of how to format your training data for input to the Image Classification \- TensorFlow model\.
+ **Training data input format:** Your training data should be a directory with as many subdirectories as the number of classes\. Each subdirectory should contain images belonging to that class in \.jpg, \.jpeg, or \.png format\.

The following is an example of an input directory structure\. This example dataset has two classes: `roses` and `dandelion`\. The image files in each class folder can have any name\. The input directory should be hosted in an Amazon S3 bucket with a path similar to the following: `s3://bucket_name/input_directory/`\. Note that the trailing `/` is required\.

```
input_directory
    |--roses
        |--abc.jpg
        |--def.jpg
    |--dandelion
        |--ghi.jpg
        |--jkl.jpg
```

Trained models output label mapping files that map class folder names to the indices in the list of output class probabilities\. This mapping is in alphabetical order\. For example, in the preceding example, the dandelion class is index 0 and the roses class is index 1\. 

After training, you have a fine\-tuned model that you can further train using incremental training or deploy for inference\. The Image Classification \- TensorFlow algorithm automatically adds a pre\-processing and post\-processing signature to the fine\-tuned model so that it can take in images as input and return class probabilities\. The file mapping class indices to class labels is saved along with the models\. 

### Incremental training<a name="IC-TF-incremental-training"></a>

You can seed the training of a new model with artifacts from a model that you trained previously with SageMaker\. Incremental training saves training time when you want to train a new model with the same or similar data\.

**Note**  
You can only seed a SageMaker Image Classification \- TensorFlow model with another Image Classification \- TensorFlow model trained in SageMaker\. 

You can use any dataset for incremental training, as long as the set of classes remains the same\. The incremental training step is similar to the fine\-tuning step, but instead of starting with a pretrained model, you start with an existing fine\-tuned model\. For an example of incremental training with the SageMaker Image Classification \- TensorFlow algorithm, see the [Introduction to SageMaker TensorFlow \- Image Classification](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/image_classification_tensorflow/Amazon_TensorFlow_Image_Classification.ipynb) sample notebook\.

### Inference with the Image Classification \- TensorFlow algorithm<a name="IC-TF-inference"></a>

You can host the fine\-tuned model that results from your TensorFlow Image Classification training for inference\. Any input image for inference must be in `.jpg`, \.`jpeg`, or `.png` format and be content type `application/x-image`\. The Image Classification \- TensorFlow algorithm resizes input images automatically\. 

Running inference results in probability values, class labels for all classes, and the predicted label corresponding to the class index with the highest probability encoded in JSON format\. The Image Classification \- TensorFlow model processes a single image per request and outputs only one line\. The following is an example of a JSON format response:

```
accept: application/json;verbose

 {"probabilities": [prob_0, prob_1, prob_2, ...],
  "labels":        [label_0, label_1, label_2, ...],
  "predicted_label": predicted_label}
```

If `accept` is set to `application/json`, then the model only outputs probabilities\. For more information on training and inference with the Image Classification \- TensorFlow algorithm, see the [Introduction to SageMaker TensorFlow \- Image Classification](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/image_classification_tensorflow/Amazon_TensorFlow_Image_Classification.ipynb) sample notebook\.

## Amazon EC2 instance recommendation for the Image Classification \- TensorFlow algorithm<a name="IC-TF-instances"></a>

The Image Classification \- TensorFlow algorithm supports all CPU and GPU instances for training, including:
+ `ml.p2.xlarge`
+ `ml.p2.16xlarge`
+ `ml.p3.2xlarge`
+ `ml.p3.16xlarge`
+ `ml.g4dn.xlarge`
+ `ml.g4dn.16.xlarge`
+ `ml.g5.xlarge`
+ `ml.g5.48xlarge`

We recommend GPU instances with more memory for training with large batch sizes\. Both CPU \(such as M5\) and GPU \(P2, P3, G4dn, or G5\) instances can be used for inference\.

## Image Classification \- TensorFlow sample notebooks<a name="IC-TF-sample-notebooks"></a>

For more information about how to use the SageMaker Image Classification \- TensorFlow algorithm for transfer learning on a custom dataset, see the [Introduction to SageMaker TensorFlow \- Image Classification](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/image_classification_tensorflow/Amazon_TensorFlow_Image_Classification.ipynb) notebook\.

For instructions how to create and access Jupyter notebook instances that you can use to run the example in SageMaker, see [Amazon SageMaker Notebook Instances](nbi.md)\. After you have created a notebook instance and opened it, select the **SageMaker Examples** tab to see a list of all the SageMaker samples\. To open a notebook, choose its **Use** tab and choose **Create copy**\.