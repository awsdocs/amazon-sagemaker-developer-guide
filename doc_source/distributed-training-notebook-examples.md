# Distributed Training Jupyter Notebok Examples<a name="distributed-training-notebook-examples"></a>

The following notebooks provide example SageMaker Distributed implementations for popular deep learning frameworks and models\. For vision \(image\) models, try MNIST or MaskRCNN\. For language \(text\) models, try BERT\.

These notebooks are provided in the [ SageMaker examples repository](https://github.com/aws/amazon-sagemaker-examples/tree/master/training/distributed_training/)\. You can also browse them on the [SageMaker examples website](https://sagemaker-examples.readthedocs.io/en/latest/training/distributed_training/index.html)\.

The examples are setup to use `p3.16xlarge` instances for the worker nodes, but you may choose other p3 instance types if you wish\. You can test a notebook using a cluster with only 1 node, but to see any performance benefit, you should use a cluster of multiple nodes \(2 or more\)\. The examples call out the section where you modify this configuration\.

## PyTorch Examples<a name="distributed-training-notebook-examples-pytorch"></a>

**SageMaker Distributed Data Parallel \(SDP\)**
+ [MNIST with PyTorch 1\.6 and SDP](https://sagemaker-examples.readthedocs.io/en/latest/training/distributed_training/pytorch/data_parallel/mnist/pytorch_smdataparallel_mnist_demo.html)
+ [MaskRCNN with PyTorch 1\.6 and SDP](https://sagemaker-examples.readthedocs.io/en/latest/training/distributed_training/pytorch/data_parallel/maskrcnn/pytorch_smdataparallel_maskrcnn_demo.html)
+ [BERT with PyTorch 1\.6 and SDP](https://sagemaker-examples.readthedocs.io/en/latest/training/distributed_training/pytorch/data_parallel/bert/pytorch_smdataparallel_bert_demo.html)

**SageMaker Distributed Model Parallel \(SMP\)**
+ [MNIST with PyTorch 1\.6 and SMP](https://sagemaker-examples.readthedocs.io/en/latest/training/distributed_training/pytorch/model_parallel/mnist/pytorch_smmodelparallel_mnist.html)
+ [BERT with PyTorch 1\.6 and SMP](https://sagemaker-examples.readthedocs.io/en/latest/training/distributed_training/pytorch/model_parallel/bert/smp_bert_tutorial.html)

## TensorFlow Examples<a name="distributed-training-notebook-examples-tensorflow"></a>

**SageMaker Distributed Data Parallel \(SDP\)**
+ [MNIST with TensorFlow 2\.3\.1 and SDP](https://sagemaker-examples.readthedocs.io/en/latest/training/distributed_training/tensorflow/data_parallel/mnist/tensorflow2_smdataparallel_mnist_demo.html)
+ [MaskRCNN with TensorFlow 2\.3\.1 and SDP](https://sagemaker-examples.readthedocs.io/en/latest/training/distributed_training/pytorch/data_parallel/maskrcnn/tensorflow2_smdataparallel_maskrcnn_demo.html)
+ [BERT with TensorFlow 2\.3\.1 and SDP](https://sagemaker-examples.readthedocs.io/en/latest/training/distributed_training/pytorch/data_parallel/bert/tensorflow2_smdataparallel_bert_demo.html)

**SageMaker Distributed Model Parallel \(SMP\)**
+ [MNIST with TensorFlow 2\.3\.1 and SMP](https://sagemaker-examples.readthedocs.io/en/latest/training/distributed_training/tensorflow/model_parallel/mnist/tensorflow_smmodelparallel_mnist.html)

## Use a SageMaker Notebook Instance<a name="distributed-training-notebook-examples-setup"></a>

To use the provided examples, we recommend that you use an Amazon SageMaker notebook instance\. A notebook instance is a machine learning \(ML\) optimized EC2 instance running the Jupyter Notebook and JupyterServer apps\. If you do not have an active notebook instance, follow the instructions in [Create a Notebook Instance](howitworks-create-ws.md)\. in the Amazon SageMaker developer guide to create one\.

After you have created an instance, in the **Notebook instances** area of the Amazon SageMaker console, and do the following:

1. Open **JupyterLab**\.

1. Select the examples icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/distributed/sm_examples_icon.png)\) in the left tray\. 

1. Browse the examples for **Training** and look for notebooks titled Distributed Data Parallel or Distributed Model Parallel\. 

## Use SageMaker Studio<a name="distributed-training-notebook-examples-studio"></a>

You can run these example Jupyter notebooks in SageMaker studio\. To download and use an example notebook, do the following in Studio: 

1. Open a terminal\.

1. In the command line, navigate to the SageMaker folder\.

   ```
   $ cd SageMaker
   ```

1. Clone the SageMaker examples repo\.

   ```
   git clone https://github.com/aws/amazon-sagemaker-examples.git
   ```

1. In the JupyterLab interface, navigate into the `amazon-sagemaker-examples` folder\.

1. In the `training/distributed_training` folder, you’ll see folders for frameworks, and in each of these, you will find folders for `data_parallel` and `model_parallel`\. Choose the example of your choice and follow the instructions to launch distributed model training with SDP on SageMaker\.