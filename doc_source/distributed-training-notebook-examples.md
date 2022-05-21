# Amazon SageMaker Distributed Training Notebook Examples<a name="distributed-training-notebook-examples"></a>

The following case studies and notebooks provide examples of implementing the SageMaker distributed training libraries for the supported deep learning frameworks \(PyTorch, TensorFlow, and HuggingFace\) and models, such as CNN and MaskRCNN for vision, and BERT for natural language processing\.

These notebooks are provided in the [SageMaker examples GitHub repository](https://github.com/aws/amazon-sagemaker-examples/tree/master/training/distributed_training/)\. You can also browse them on the [SageMaker examples website](https://sagemaker-examples.readthedocs.io/en/latest/training/distributed_training/index.html)\.

The examples are set up to use `p3.16xlarge` instances for the worker nodes, but you may choose `ml.p3dn.24xlarge` or `ml.p4d.24xlarge` instance types for which the SageMaker distributed training libraries are optimized\. You can test the notebooks using a cluster of a single node; however, to see a performance improvement as shown in the [Training Benchmarks](https://docs.aws.amazon.com/sagemaker/latest/dg/data-parallel-intro.html#data-parallel-benchmarks) section, use a cluster of multiple nodes \(two or more\)\. The examples call out the section in which you modify this configuration\.

## Blogs and Case Studies<a name="distributed-training-notebook-examples-blog"></a>

The following blogs discuss case studies about using the SageMaker distributed training libraries\.

**SageMaker Distributed Data Parallel**
+ [Hyundai reduces ML model training time for autonomous driving models using Amazon SageMaker](https://aws.amazon.com/blogs/machine-learning/hyundai-reduces-training-time-for-autonomous-driving-models-using-amazon-sagemaker/)
+ [Distributed Training: Train BART/T5 for Summarization using Transformers and Amazon SageMaker](https://huggingface.co/blog/sagemaker-distributed-training-seq2seq) in the *HuggingFace documentation*
+ [Training YOLOv5 on AWS with PyTorch and the SageMaker distributed data parallel library](https://medium.com/@sitecao/training-yolov5-on-aws-with-pytorch-and-sagemaker-distributed-data-parallel-library-a196ab01409b) in *Medium*
+ [Speed up EfficientNet model training on SageMaker with PyTorch and the SageMaker distributed data parallel library](https://medium.com/@dangmz/speed-up-efficientnet-model-training-on-amazon-sagemaker-with-pytorch-and-sagemaker-distributed-dae4b048c01a) in *Medium*
+ [Speed up EfficientNet training on AWS with the SageMaker distributed data parallel library](https://towardsdatascience.com/speed-up-efficientnet-training-on-aws-by-up-to-30-with-sagemaker-distributed-data-parallel-library-2dbf6d1e18e8) in *Towards Data Science*

## PyTorch Examples<a name="distributed-training-notebook-examples-pytorch"></a>

**SageMaker Distributed Data Parallel**
+ [CNN with PyTorch 1\.6 and SageMaker Data Parallel](https://sagemaker-examples.readthedocs.io/en/latest/training/distributed_training/pytorch/data_parallel/mnist/pytorch_smdataparallel_mnist_demo.html)
+ [MaskRCNN with PyTorch 1\.6 and SageMaker Data Parallel](https://sagemaker-examples.readthedocs.io/en/latest/training/distributed_training/pytorch/data_parallel/maskrcnn/pytorch_smdataparallel_maskrcnn_demo.html)
+ [BERT with PyTorch 1\.6 and SageMaker Data Parallel](https://sagemaker-examples.readthedocs.io/en/latest/training/distributed_training/pytorch/data_parallel/bert/pytorch_smdataparallel_bert_demo.html)

**SageMaker Distributed Model Parallel**
+ [Train GPT\-2 with PyTorch 1\.8\.1 and Tensor Parallelism Using the SageMaker Model Parallelism Library](https://sagemaker-examples.readthedocs.io/en/latest/training/distributed_training/pytorch/model_parallel/gpt2/smp-train-gpt-simple.html)
+ [BERT with PyTorch 1\.6 and SageMaker Model Parallel](https://sagemaker-examples.readthedocs.io/en/latest/training/distributed_training/pytorch/model_parallel/bert/smp_bert_tutorial.html)

## TensorFlow Examples<a name="distributed-training-notebook-examples-tensorflow"></a>

**SageMaker Distributed Data Parallel **
+ [CNN with TensorFlow 2\.3\.1 and SageMaker Data Parallel](https://sagemaker-examples.readthedocs.io/en/latest/training/distributed_training/tensorflow/data_parallel/mnist/tensorflow2_smdataparallel_mnist_demo.html)
+ [MaskRCNN with TensorFlow 2\.3\.1 and SageMaker Data Parallel](https://sagemaker-examples.readthedocs.io/en/latest/training/distributed_training/tensorflow/data_parallel/maskrcnn/tensorflow2_smdataparallel_maskrcnn_demo.html)
+ [BERT with TensorFlow 2\.3\.1 and SageMaker Data Parallel](https://sagemaker-examples.readthedocs.io/en/latest/training/distributed_training/tensorflow/data_parallel/bert/tensorflow2_smdataparallel_bert_demo.html)

**SageMaker Distributed Model Parallel**
+ [CNN with TensorFlow 2\.3\.1 and SageMaker Model Parallel](https://sagemaker-examples.readthedocs.io/en/latest/training/distributed_training/tensorflow/model_parallel/mnist/tensorflow_smmodelparallel_mnist.html)

## HuggingFace Examples<a name="distributed-training-notebook-examples-huggingface"></a>

The following HuggingFace on SageMaker examples are available in the [HuggingFace notebooks repository](https://github.com/huggingface/notebooks/tree/master/sagemaker)\.

**SageMaker Distributed Data Parallel **
+ [HuggingFace Distributed Data Parallel Training in PyTorch on SageMaker \- Distributed Question Answering](https://github.com/huggingface/notebooks/blob/master/sagemaker/03_distributed_training_data_parallelism/sagemaker-notebook.ipynb)
+ [HuggingFace Distributed Data Parallel Training in PyTorch on SageMaker \- Distributed Text Summarization](https://github.com/huggingface/notebooks/blob/master/sagemaker/08_distributed_summarization_bart_t5/sagemaker-notebook.ipynb)
+ [HuggingFace Distributed Data Parallel Training in TensorFlow on SageMaker](https://github.com/huggingface/notebooks/blob/master/sagemaker/07_tensorflow_distributed_training_data_parallelism/sagemaker-notebook.ipynb)

**SageMaker Distributed Model Parallel**
+ [HuggingFace with TensorFlow Distributed Model Parallel Training on SageMaker](https://github.com/huggingface/notebooks/blob/master/sagemaker/04_distributed_training_model_parallelism/sagemaker-notebook.ipynb)

## How to Access or Download the SageMaker Distributed Training Notebook Examples<a name="distributed-training-notebook-examples-setup"></a>

Follow instructions to access or download the SageMaker distributed training example notebooks\.

### Option 1: Use a SageMaker notebook instance<a name="distributed-training-notebook-examples-ni"></a>

To use the aforementioned examples, we recommend that you use an Amazon SageMaker notebook instance\. A notebook instance runs Jupyter Notebook and JupyterServer apps on Amazon EC2 instances, which are optimized for machine learning\. If you do not have an active notebook instance, follow the instructions in [Create a Notebook Instance](howitworks-create-ws.md) in the SageMaker developer guide to create one\.

After you have created an instance, in the **Notebook instances** page of the SageMaker console, do the following:

1. Open **JupyterLab**\.

1. Select the examples icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/distributed/sm_examples_icon.png)\) in the left tray\. 

1. Browse the examples for **Training** and look for notebooks titled *Distributed Data Parallel* or *Distributed Model Parallel*\. 

### Option 2: Clone the SageMaker example repository to SageMaker Studio or notebook instance<a name="distributed-training-notebook-examples-studio"></a>

To download and use the aforementioned example notebooks, do the following to clone the example GitHub repositories: 

1. Open a terminal\.

1. In the command line, navigate to the SageMaker folder\.

   ```
   cd SageMaker
   ```

1. Clone the [SageMaker examples GitHub repository](https://github.com/aws/amazon-sagemaker-examples.git)\.

   ```
   git clone https://github.com/aws/amazon-sagemaker-examples.git
   ```
**Note**  
To download the [HuggingFace example notebooks](#distributed-training-notebook-examples-huggingface), clone the [HuggingFace notebooks GitHub repository](https://github.com/huggingface/notebooks):  

   ```
   git clone https://github.com/huggingface/notebooks huggingface-notebooks
   ```

1. In the JupyterLab interface, navigate into the `amazon-sagemaker-examples` folder\.

1. In the `training/distributed_training` folder, there are folders for frameworks, and in each of these, there are folders for `data_parallel` and `model_parallel`\. Choose the example of your choice and follow the instructions to launch distributed training with an SageMaker distributed training library\.