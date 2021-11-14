# Prepare Model for Compilation<a name="neo-compilation-preparing-model"></a>

SageMaker Neo requires machine learning models satisfy specific input data shape\. The input shape required for compilation depends on the deep learning framework you use\. Once your model input shape is correctly formatted, save your model according to the requirements below\. Once you have a saved model, compress the model artifacts\.

**Topics**
+ [What input data shapes does SageMaker Neo expect?](#neo-job-compilation-expected-inputs)
+ [Saving Models for SageMaker Neo](#neo-job-compilation-how-to-save-model)

## What input data shapes does SageMaker Neo expect?<a name="neo-job-compilation-expected-inputs"></a>

 Before you compile your model, make sure your model is formatted correctly\. Neo expects the name and shape of the expected data inputs for your trained model with JSON format or list format\. The expected inputs are framework specific\. 

Below are the input shapes SageMaker Neo expects:

### Keras<a name="collapsible-section-2"></a>

Specify the name and shape \(NCHW format\) of the expected data inputs using a dictionary format for your trained model\. Note that while Keras model artifacts should be uploaded in NHWC \(channel\-last\) format, DataInputConfig should be specified in NCHW \(channel\-first\) format\. The dictionary formats required are as follows: 
+ For one input: `{'input_1':[1,3,224,224]}`
+ For two inputs: `{'input_1': [1,3,224,224], 'input_2':[1,3,224,224]}`

### MXNet/ONNX<a name="collapsible-section-3"></a>

Specify the name and shape \(NCHW format\) of the expected data inputs using a dictionary format for your trained model\. The dictionary formats required are as follows:
+ For one input: `{'data':[1,3,1024,1024]}`
+ For two inputs: `{'var1': [1,1,28,28], 'var2':[1,1,28,28]}`

### PyTorch<a name="collapsible-section-4"></a>

Specify the name and shape \(NCHW format\) of the expected data inputs using a dictionary format for your trained model\. Alternatively, you can specify the shape only using a list format\. The dictionary formats required are are as follows:
+ For one input in dictionary format: `{'input0':[1,3,224,224]}`
+ For one input in list format: `[[1,3,224,224]]`
+ For two inputs in dictionary format: `{'input0':[1,3,224,224], 'input1':[1,3,224,224]}`
+ For two inputs in list format: `[[1,3,224,224], [1,3,224,224]]`

### TensorFlow<a name="collapsible-section-1"></a>

Specify the name and shape \(NHWC format\) of the expected data inputs using a dictionary format for your trained model\. The dictionary formats required are as follows:
+ For one input: `{'input':[1,1024,1024,3]}`
+ For two inputs: `{'data1': [1,28,28,1], 'data2':[1,28,28,1]}`

### TFLite<a name="collapsible-section-6"></a>

Specify the name and shape \(NHWC format\) of the expected data inputs using a dictionary format for your trained model\. The dictionary formats required are as follows:
+ For one input: `{'input':[1,224,224,3]}`

**Note**  
SageMaker Neo only supports TensorFlow Lite for edge device targets\. For a list of supported SageMaker Neo edge device targets, see the SageMaker Neo [Devices](neo-supported-devices-edge-devices.md#neo-supported-edge-devices) page\. For a list of supported SageMaker Neo cloud instance targets, see the SageMaker Neo [Supported Instance Types and Frameworks](neo-supported-cloud.md) page\.

### XGBoost<a name="collapsible-section-5"></a>

An input data name and shape are not needed\.

## Saving Models for SageMaker Neo<a name="neo-job-compilation-how-to-save-model"></a>

The following code examples show how to save your model to make it compatible with Neo\. Models must be packaged as compressed tar files \(`*.tar.gz`\)\.

### Keras<a name="how-to-save-tf-keras"></a>

Keras models require one model definition file \(`.h5`\)\.

There are two options for saving your Keras model in order to make it compatible for SageMaker Neo:

1. Export to `.h5` format with `model.save("<model-name>", save_format="h5")`\.

1. Freeze the `SavedModel` after exporting\.

Below is an example of how to export a `tf.keras` model as a frozen graph \(option two\):

```
import os
import tensorflow as tf
from tensorflow.keras.applications.resnet50 import ResNet50
from tensorflow.keras import backend

tf.keras.backend.set_learning_phase(0)
model = tf.keras.applications.ResNet50(weights='imagenet', include_top=False, input_shape=(224, 224, 3), pooling='avg')
model.summary()

# Save as a SavedModel
export_dir = 'saved_model/'
model.save(export_dir, save_format='tf')

# Freeze saved model
input_node_names = [inp.name.split(":")[0] for inp in model.inputs]
output_node_names = [output.name.split(":")[0] for output in model.outputs]
print("Input names: ", input_node_names)
with tf.Session() as sess:
    loaded = tf.saved_model.load(sess, export_dir=export_dir, tags=["serve"]) 
    frozen_graph = tf.graph_util.convert_variables_to_constants(sess,
                                                                sess.graph.as_graph_def(),
                                                                output_node_names)
    tf.io.write_graph(graph_or_graph_def=frozen_graph, logdir=".", name="frozen_graph.pb", as_text=False)

import tarfile
tar = tarfile.open("frozen_graph.tar.gz", "w:gz")
tar.add("frozen_graph.pb")
tar.close()
```

**Warning**  
Do not export your model with the `SavedModel` class using `model.save(<path>, save_format='tf')`\. This format is suitable for training, but it is not suitable for inference\.

### MXNet<a name="how-to-save-mxnet"></a>

MXNet models must be saved as a single symbol file `*-symbol.json` and a single parameter `*.params files`\.

------
#### [ Gluon Models ]

Define the neural network using the `HybridSequential` Class\. This will run the code in the style of symbolic programming \(as opposed to imperative programming\)\.

```
from mxnet import nd, sym
from mxnet.gluon import nn

def get_net():
    net = nn.HybridSequential()  # Here we use the class HybridSequential.
    net.add(nn.Dense(256, activation='relu'),
            nn.Dense(128, activation='relu'),
            nn.Dense(2))
    net.initialize()
    return net

# Define an input to compute a forward calculation. 
x = nd.random.normal(shape=(1, 512))
net = get_net()

# During the forward calculation, the neural network will automatically infer
# the shape of the weight parameters of all the layers based on the shape of
# the input.
net(x)
                        
# hybridize model
net.hybridize()
net(x)

# export model
net.export('<model_name>') # this will create model-symbol.json and model-0000.params files

import tarfile
tar = tarfile.open("<model_name>.tar.gz", "w:gz")
for name in ["<model_name>-0000.params", "<model_name>-symbol.json"]:
    tar.add(name)
tar.close()
```

For more information about hybridizing models, see the [MXNet hybridize documentation](https://mxnet.apache.org/versions/1.7.0/api/python/docs/tutorials/packages/gluon/blocks/hybridize.html)\.

------
#### [ Gluon Model Zoo \(GluonCV\) ]

GluonCV model zoo models come pre\-hybridized\. So you can just export them\.

```
import numpy as np
import mxnet as mx
import gluoncv as gcv
from gluoncv.utils import export_block
import tarfile

net = gcv.model_zoo.get_model('<model_name>', pretrained=True) # For example, choose <model_name> as resnet18_v1
export_block('<model_name>', net, preprocess=True, layout='HWC')

tar = tarfile.open("<model_name>.tar.gz", "w:gz")

for name in ["<model_name>-0000.params", "<model_name>-symbol.json"]:
    tar.add(name)
tar.close()
```

------
#### [ Non Gluon Models ]

All non\-Gluon models when saved to disk use `*-symbol` and `*.params` files\. They are therefore already in the correct format for Neo\.

```
# Pass the following 3 parameters: sym, args, aux
mx.model.save_checkpoint('<model_name>',0,sym,args,aux) # this will create <model_name>-symbol.json and <model_name>-0000.params files

import tarfile
tar = tarfile.open("<model_name>.tar.gz", "w:gz")

for name in ["<model_name>-0000.params", "<model_name>-symbol.json"]:
    tar.add(name)
tar.close()
```

------

### PyTorch<a name="how-to-save-pytorch"></a>

PyTorch models must be saved as a definition file \(`.pt` or `.pth`\) with input datatype of `float32`\.

To save your model, use `torch.jit.trace` followed by `torch.save`\. This will save an object to a disk file and by default uses python pickle \(`pickle_module=pickle`\) to save the objects and some metadata\. Next, convert the saved model to a compressed tar file\.

```
import torchvision
import torch

model = torchvision.models.resnet18(pretrained=True)
model.eval()
inp = torch.rand(1, 3, 224, 224)
model_trace = torch.jit.trace(model, inp)

# Save your model. The following code saves it with the .pth file extension
model_trace.save('model.pth')

# Save as a compressed tar file
import tarfile
with tarfile.open('model.tar.gz', 'w:gz') as f:
    f.add('model.pth')
f.close()
```

### TensorFlow<a name="how-to-save-tf"></a>

TensorFlow requires one `.pb` or one `.pbtxt` file and a variables directory that contains variables\. For frozen models, only one `.pb` or `.pbtxt` file is required\.

------
#### [ Pre\-Trained Model ]

The following code example shows how to use the tar Linux command to compress your model\. Run the following in your terminal or in a Jupyter notebook \(if you use a Jupyter notebook, insert the `!` magic command at the beginning of the statement\):

```
# Download SSD_Mobilenet trained model
!wget http://download.tensorflow.org/models/object_detection/ssd_mobilenet_v2_coco_2018_03_29.tar.gz

# unzip the compressed tar file
!tar xvf ssd_mobilenet_v2_coco_2018_03_29.tar.gz

# Compress the tar file and save it in a directory called 'model.tar.gz'
!tar czvf model.tar.gz ssd_mobilenet_v2_coco_2018_03_29/frozen_inference_graph.pb
```

The command flags used in this example accomplish the following:
+ `c`: Create an archive
+ `z`: Compress the archive with gzip
+ `v`: Display archive progress
+ `f`: Specify the filename of the archive

------

### Built\-In Estimators<a name="how-to-save-built-in"></a>

Built\-in estimators are either made by framework\-specific containers or algorithm\-specific containers\. Estimator objects for both the built\-in algorithm and framework\-specific estimator saves the model in the correct format for you when you train the model using the built\-in `.fit` method\.

For example, you can use a `sagemaker.TensorFlow` to define a TensorFlow estimator:

```
from sagemaker.tensorflow import TensorFlow

estimator = TensorFlow(entry_point='mnist.py',
                        role=role,  #param role can be arn of a sagemaker execution role
                        framework_version='1.15.3',
                        py_version='py3',
                        training_steps=1000, 
                        evaluation_steps=100,
                        instance_count=2,
                        instance_type='ml.c4.xlarge')
```

Then train the model with `.fit` built\-in method:

```
estimator.fit(inputs)
```

Before finally compiling model with the build in `compile_model` method:

```
# Specify output path of the compiled model
output_path = '/'.join(estimator.output_path.split('/')[:-1])

# Compile model
optimized_estimator = estimator.compile_model(target_instance_family='ml_c5', 
                              input_shape={'data':[1, 784]},  # Batch size 1, 3 channels, 224x224 Images.
                              output_path=output_path,
                              framework='tensorflow', framework_version='1.15.3')
```

You can also use the `sagemaker.estimator.Estimator` Class to initialize an estimator object for training and compiling a built\-in algorithm with the `compile_model` method from the SageMaker Python SDK:

```
import sagemaker
from sagemaker.image_uris import retrieve
sagemaker_session = sagemaker.Session()
aws_region = sagemaker_session.boto_region_name

# Specify built-in algorithm training image
training_image = retrieve(framework='image-classification', 
                          region=aws_region, image_scope='training')

training_image = retrieve(framework='image-classification', region=aws_region, image_scope='training')

# Create estimator object for training
estimator = sagemaker.estimator.Estimator(image_uri=training_image,
                                          role=role,  #param role can be arn of a sagemaker execution role
                                          instance_count=1,
                                          instance_type='ml.p3.8xlarge',
                                          volume_size = 50,
                                          max_run = 360000,
                                          input_mode= 'File',
                                          output_path=s3_training_output_location,
                                          base_job_name='image-classification-training'
                                          )
                                          
# Setup the input data_channels to be used later for training.                                          
train_data = sagemaker.inputs.TrainingInput(s3_training_data_location,
                                            content_type='application/x-recordio',
                                            s3_data_type='S3Prefix')
validation_data = sagemaker.inputs.TrainingInput(s3_validation_data_location,
                                                content_type='application/x-recordio',
                                                s3_data_type='S3Prefix')
data_channels = {'train': train_data, 'validation': validation_data}


# Train model
estimator.fit(inputs=data_channels, logs=True)

# Compile model with Neo                                                                                  
optimized_estimator = estimator.compile_model(target_instance_family='ml_c5',
                                          input_shape={'data':[1, 3, 224, 224], 'softmax_label':[1]},
                                          output_path=s3_compilation_output_location,
                                          framework='mxnet',
                                          framework_version='1.7')
```

For more information about compiling models with the SageMaker Python SDK, see [Compile a Model \(Amazon SageMaker SDK\)](neo-job-compilation-sagemaker-sdk.md)\.