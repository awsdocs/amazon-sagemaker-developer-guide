# Writing Custom TensorFlow Model Training and Inference Code<a name="tf-training-inference-code-template"></a>

To train a model on Amazon SageMaker using custom TensorFlow code and deploy it on Amazon SageMaker, you need to implement training and inference code interfaces in your code\. 

Your TensorFlow training script must be a Python 2\.7 source file\. The current default TensorFlow version is 1\.5\. This training/inference script must contain the following functions:

+ `model_fn`: Defines the model that will be trained\.

+ `train_input_fn`: Preprocess and load training data\.

+ `eval_input_fn`: Preprocess and load evaluation data\.

+ `serving_input_fn`: Defines the features to be passed to the model during prediction\.

For more information, see [https://github\.com/aws/sagemaker\-python\-sdk\#tensorflow\-sagemaker\-estimators](https://github.com/aws/sagemaker-python-sdk#tensorflow-sagemaker-estimators)\. 

Implement the following training code interface:

```
 def model_fn(features, labels, mode, hyperparameters):
    """
    Implement code to do the following:
    1. Configure the model with TensorFlow operations
    2. Define the loss function for training/evaluation
    3. Define the training operation/optimizer
    4. Generate predictions
    5. Return predictions/loss/train_op/eval_metric_ops in EstimatorSpec object 

    For more information on how to create a model_fn, see 
    https://www.tensorflow.org/extend/estimators#constructing_the_model_fn.
    
    Args:
        features:  A dict containing the features passed to the model with train_input_fn in 
           training mode, with eval_input_fn in evaluation mode, and with serving_input_fn 
           in predict mode.
        labels:    Tensor containing the labels passed to the model with train_input_fn in 
           training mode and eval_input_fn in evaluation mode. It is empty for 
           predict mode.
        mode:     One of the following tf.estimator.ModeKeys string values indicating the context 
           in which the model_fn was invoked: 
           - TRAIN: The model_fn was invoked in training mode. 
           - EVAL: The model_fn was invoked in evaluation mode. 
           - PREDICT: The model_fn was invoked in predict mode:

        hyperparameters: The hyperparameters passed to your Amazon SageMaker TrainingJob that 
           runs your TensorFlow training script. You can use this to pass hyperparameters 
           to your training script. 
                          
    Returns: An EstimatorSpec, which contains evaluation and loss function. 
    """
 
 
def train_input_fn(training_dir, hyperparameters):
    """
    Implement code to do the following:
    1. Read the **training** dataset files located in training_dir
    2. Preprocess the dataset
    3. Return 1) a mapping of feature columns to Tensors with
    the corresponding feature data, and 2) a Tensor containing labels
 
    For more information on how to create a input_fn, see https://www.tensorflow.org/get_started/input_fn.

    Args:
        training_dir:    Directory where the dataset is located inside the container.
        hyperparameters: The hyperparameters passed to your Amazon SageMaker TrainingJob that 
           runs your TensorFlow training script. You can use this to pass hyperparameters 
           to your training script.
 
    Returns: (data, labels) tuple
    """
 
 
def eval_input_fn(training_dir, hyperparameters):
    """
   Implement code to do the following:
    1. Read the **evaluation** dataset files located in training_dir
    2. Preprocess the dataset
    3. Return 1) a mapping of feature columns to Tensors with
    the corresponding feature data, and 2) a Tensor containing labels
 
    For more information on how to create a input_fn, see https://www.tensorflow.org/get_started/input_fn.

    Args:
     training_dir: The directory where the dataset is located inside the container.
     hyperparameters: The hyperparameters passed to your Amazon SageMaker TrainingJob that 
           runs your TensorFlow training script. You can use this to pass hyperparameters 
           to your training script.
 
    Returns: (data, labels) tuple
    """
 
def serving_input_fn(hyperparameters):
    """
    During training, a train_input_fn() ingests data and prepares it for use by the model. 
    At the end of training, similarly, a serving_input_fn() is called to create the model that 
    will be exported for Tensorflow Serving.

	Use this function to do the following:

		- Add placeholders to the graph that the serving system will feed with inference requests.
		- Add any additional operations needed to convert data from the input format into the 
         feature Tensors expected by the model.

	The function returns a tf.estimator.export.ServingInputReceiver object, which packages the placeholders
      and the resulting feature Tensors together.

	Typically, inference requests arrive in the form of serialized tf.Examples, so the 
      serving_input_receiver_fn() creates a single string placeholder to receive them. The serving_input_receiver_fn() 
      is then also responsible for parsing the tf.Examples by adding a tf.parse_example operation to the graph.

	For more information on how to create a serving_input_fn, see 
      https://github.com/tensorflow/tensorflow/blob/18003982ff9c809ab8e9b76dd4c9b9ebc795f4b8/tensorflow/docs_src/programmers_guide/saved_model.md#preparing-serving-inputs.
	
    Args:	
     hyperparameters: The hyperparameters passed to your TensorFlow Amazon SageMaker estimator that 
           deployed your TensorFlow inference script. You can use this to pass hyperparameters 
           to your inference script.

	"""
```

Optionally implement the following inference code interface:

```
def input_fn(data, content_type):
  """
  [Optional]
  Prepares data for transformation. Amazon SageMaker invokes your input_fn definition in response to
  an InvokeEndpoint operation on an Amazon SageMaker endpoint containing this script. Amazon SageMaker passes in the 
  data in the InvokeEndpoint request, and the
  InvokeEndpoint ContentType argument.
  If you omit this function, Amazon SageMaker provides a default input_fn for you. The default
  input_fn supports protobuf, CSV, or JSON-encoded array data. It returns the input in the format expected by
  TensorFlow serving. For more information about the default input_fn, see the Amazon SageMaker Python SDK
  GitHub documentation.
  
  Args:
     data: An Amazon SageMaker InvokeEndpoint request data
     content_type: An Amazon SageMaker InvokeEndpoint ContentType value for data.
  Returns:
     object: A deserialized object that will be used by TensorFlow serving as input. Must be in the format
        defined by the placeholders in your serving_input_fn.
  """

def output_fn(data, accepts):
  """
  [Optional]
  Serializes the result of prediction in response to an InvokeEndpoint request. This
  function should return a serialized object. This serialized object is returned in the response to an
  InvokeEndpoint request. If you omit this function, Amazon SageMaker provides a default output_fn for you. 
  The default function works with protobuf, CSV, and JSON accept types and serializes data to either, 
  depending on the specified accepts.
         
  Args:
    data: A result from TensorFlow Serving
    accepts: The Amazon SageMaker InvokeEndpoint Accept value. The content type the response
      object should be serialized to.
  Returns:
     object: The serialized object that will be send to back to the client.
```