# Text Classification \- TensorFlow<a name="text-classification-tensorflow"></a>

The Amazon SageMaker Text Classification \- TensorFlow algorithm is a supervised learning algorithm that supports transfer learning with many pretrained models from the [TensorFlow Hub](https://tfhub.dev/)\. Use transfer learning to fine\-tune one of the available pretrained models on your own dataset, even if a large amount of text data is not available\. The text classification algorithm takes a text string as input and outputs a probability for each of the class labels\. Training datasets must be in CSV format\.

**Topics**
+ [How to use the SageMaker Text Classification \- TensorFlow algorithm](#text-classification-tensorflow-how-to-use)
+ [Input and output interface for the Text Classification \- TensorFlow algorithm](#text-classification-tensorflow-inputoutput)
+ [Amazon EC2 instance recommendation for the Text Classification \- TensorFlow algorithm](#text-classification-tensorflow-instances)
+ [Text Classification \- TensorFlow sample notebooks](#text-classification-tensorflow-sample-notebooks)
+ [How Text Classification \- TensorFlow Works](text-classification-tensorflow-HowItWorks.md)
+ [TensorFlow Hub Models](text-classification-tensorflow-Models.md)
+ [Text Classification \- TensorFlow Hyperparameters](text-classification-tensorflow-Hyperparameter.md)
+ [Tune a Text Classification \- TensorFlow model](text-classification-tensorflow-tuning.md)

## How to use the SageMaker Text Classification \- TensorFlow algorithm<a name="text-classification-tensorflow-how-to-use"></a>

You can use Text Classification \- TensorFlow as an Amazon SageMaker built\-in algorithm\. The following section describes how to use Text Classification \- TensorFlow with the SageMaker Python SDK\. For information on how to use Text Classification \- TensorFlow from the Amazon SageMaker Studio UI, see [SageMaker JumpStart](studio-jumpstart.md)\.

The Text Classification \- TensorFlow algorithm supports transfer learning using any of the compatible pretrained TensorFlow models\. For a list of all available pretrained models, see [TensorFlow Hub Models](text-classification-tensorflow-Models.md)\. Every pretrained model has a unique `model_id`\. The following example uses BERT Base Uncased \(`model_id`: `tensorflow-tc-bert-en-uncased-L-12-H-768-A-12-2`\) to fine\-tune on a custom dataset\. The pretrained models are all pre\-downloaded from the TensorFlow Hub and stored in Amazon S3 buckets so that training jobs can run in network isolation\. Use these pre\-generated model training artifacts to construct a SageMaker Estimator\.

First, retrieve the Docker image URI, training script URI, and pretrained model URI\. Then, change the hyperparameters as you see fit\. You can see a Python dictionary of all available hyperparameters and their default values with `hyperparameters.retrieve_default`\. For more information, see [Text Classification \- TensorFlow Hyperparameters](text-classification-tensorflow-Hyperparameter.md)\. Use these values to construct a SageMaker Estimator\.

**Note**  
Default hyperparameter values are different for different models\. For example, for larger models, the default batch size is smaller\. 

This example uses the [https://www.tensorflow.org/datasets/catalog/glue#gluesst2](https://www.tensorflow.org/datasets/catalog/glue#gluesst2) dataset, which contains positive and negative movie reviews\. We pre\-downloaded the dataset and made it available with Amazon S3\. To fine\-tune your model, call `.fit` using the Amazon S3 location of your training dataset\.

```
from sagemaker import image_uris, model_uris, script_uris, hyperparameters
from sagemaker.estimator import Estimator

model_id, model_version = "tensorflow-tc-bert-en-uncased-L-12-H-768-A-12-2", "*"
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
training_data_prefix = "training-datasets/SST2/"

training_dataset_s3_path = f"s3://{training_data_bucket}/{training_data_prefix}"

output_bucket = sess.default_bucket()
output_prefix = "jumpstart-example-tc-training"
s3_output_location = f"s3://{output_bucket}/{output_prefix}/output"

# Create an Estimator instance
tf_tc_estimator = Estimator(
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
tf_tc_estimator.fit({"training": training_dataset_s3_path}, logs=True)
```

For more information about how to use the SageMaker Text Classification \- TensorFlow algorithm for transfer learning on a custom dataset, see the [Introduction to JumpStart \- Text Classification](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/jumpstart_text_classification/Amazon_JumpStart_Text_Classification.ipynb) notebook\.

## Input and output interface for the Text Classification \- TensorFlow algorithm<a name="text-classification-tensorflow-inputoutput"></a>

Each of the pretrained models listed in TensorFlow Hub Models can be fine\-tuned to any dataset made up of text sentences with any number of classes\. The pretrained model attaches a classification layer to the Text Embedding model and initializes the layer parameters to random values\. The output dimension of the classification layer is determined based on the number of classes detected in the input data\. 

Be mindful of how to format your training data for input to the Text Classification \- TensorFlow model\.
+ **Training data input format:** A directory containing a `data.csv` file\. Each row of the first column should have integer class labels between 0 and the number of classes\. Each row of the second column should have the corresponding text data\.

The following is an example of an input CSV file\. Note that the file should not have any header\. The file should be hosted in an Amazon S3 bucket with a path similar to the following: `s3://bucket_name/input_directory/`\. Note that the trailing `/` is required\.

```
|   |  |
|---|---|
|0 |hide new secretions from the parental units|
|0 |contains no wit , only labored gags|
|1 |that loves its characters and communicates something rather beautiful about human nature|
|...|...|
```

### Incremental training<a name="text-classification-tensorflow-incremental-training"></a>

You can seed the training of a new model with artifacts from a model that you trained previously with SageMaker\. Incremental training saves training time when you want to train a new model with the same or similar data\.

**Note**  
You can only seed a SageMaker Text Classification \- TensorFlow model with another Text Classification \- TensorFlow model trained in SageMaker\. 

You can use any dataset for incremental training, as long as the set of classes remains the same\. The incremental training step is similar to the fine\-tuning step, but instead of starting with a pretrained model, you start with an existing fine\-tuned model\. 

For more information on using incremental training with the SageMaker Text Classification \- TensorFlow algorithm, see the [Introduction to JumpStart \- Text Classification](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/jumpstart_text_classification/Amazon_JumpStart_Text_Classification.ipynb) sample notebook\.

### Inference with the Text Classification \- TensorFlow algorithm<a name="text-classification-tensorflow-inference"></a>

You can host the fine\-tuned model that results from your TensorFlow Text Classification training for inference\. Any raw text formats for inference must be content type `application/x-text`\.

Running inference results in probability values, class labels for all classes, and the predicted label corresponding to the class index with the highest probability encoded in JSON format\. The Text Classification \- TensorFlow model processes a single string per request and outputs only one line\. The following is an example of a JSON format response:

```
accept: application/json;verbose

{"probabilities": [prob_0, prob_1, prob_2, ...],
"labels": [label_0, label_1, label_2, ...],
"predicted_label": predicted_label}
```

If `accept` is set to `application/json`, then the model only outputs probabilities\. 

## Amazon EC2 instance recommendation for the Text Classification \- TensorFlow algorithm<a name="text-classification-tensorflow-instances"></a>

The Text Classification \- TensorFlow algorithm supports all CPU and GPU instances for training, including:
+ `ml.p2.xlarge`
+ `ml.p2.16xlarge`
+ `ml.p3.2xlarge`
+ `ml.p3.16xlarge`
+ `ml.g4dn.xlarge`
+ `ml.g4dn.16.xlarge`
+ `ml.g5.xlarge`
+ `ml.g5.48xlarge`

We recommend GPU instances with more memory for training with large batch sizes\. Both CPU \(such as M5\) and GPU \(P2, P3, G4dn, or G5\) instances can be used for inference\. For a comprehensive list of SageMaker training and inference instances across AWS Regions, see [Amazon SageMaker Pricing](http://aws.amazon.com/sagemaker/pricing/)\.

## Text Classification \- TensorFlow sample notebooks<a name="text-classification-tensorflow-sample-notebooks"></a>

For more information about how to use the SageMaker Text Classification \- TensorFlow algorithm for transfer learning on a custom dataset, see the [Introduction to JumpStart \- Text Classification](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/jumpstart_text_classification/Amazon_JumpStart_Text_Classification.ipynb) notebook\.

For instructions how to create and access Jupyter notebook instances that you can use to run the example in SageMaker, see [Use Amazon SageMaker Notebook Instances](nbi.md)\. After you have created a notebook instance and opened it, select the **SageMaker Examples** tab to see a list of all the SageMaker samples\. To open a notebook, choose its **Use** tab and choose **Create copy**\.