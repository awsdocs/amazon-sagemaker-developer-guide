# Train a Deep Graph Network<a name="deep-graph-library"></a>

 In this overview, you learn how to get started with a deep graph network by using one of the DGL containers in Amazon Elastic Container Registry \(Amazon ECR\)\. You can also see links to practical examples for deep graph networks\. 

## What Is a Deep Graph Network?<a name="deep-graph-library-what-is"></a>

 Deep graph networks refer to a type of neural network that is trained to solve graph problems\. A deep graph network uses an underlying deep learning framework like PyTorch or MXNet\. The potential for graph networks in practical AI applications are highlighted in the Amazon SageMaker tutorials for [Deep Graph Library](https://www.dgl.ai/) \(DGL\)\. Examples for training models on graph datasets include social networks, knowledge bases, biology, and chemistry\. 

 ![\[The DGL ecosystem.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/dgl_white_background_bold.png) 

 *Figure 1\. The DGL ecosystem* 

Several examples are provided using Amazon SageMaker’s deep learning containers that are preconfigured with DGL\. If you have special modules you want to use with DGL, you can also build your own container\. The examples involve heterographs, which are graphs that have multiple types of nodes and edges, and draw on a variety of applications across disparate scientific fields, such as bioinformatics and social network analysis\. DGL provides a wide array of [graph neural network implementations for different types models](https://docs.dgl.ai/tutorials/models/index.html)\. Some of the highlights include: 
+ GCN \- Graph convolutional network 
+ R\-GCN \- Relational graph convolutional network 
+ GAT \- Graph attention network 
+ DGMG \- Deep generative models of graphs 
+ JTNN \- Junction tree neural network 

## Get Started<a name="deep-graph-library-get-started"></a>

DGL is available as a deep learning container in Amazon ECR\. You can select deep learning containers when you write your estimator function in an Amazon SageMaker notebook\. You can also craft your own custom container with DGL by following the [Bring Your Own Container](https://docs.aws.amazon.com/sagemaker/latest/dg/your-algorithms.html) guide\. The easiest way to get started with a deep graph network uses one of the DGL containers in Amazon ECR\.  

**Note**  
 Backend framework support is limited to PyTorch and MXNet\. 

**Setup**  
If you are using Amazon SageMaker Studio, you need to clone the examples repo first\. If you are using a notebook instance, you can find the examples choosing the SageMaker icon at bottom of the left toolbar\. 

**To clone the Amazon SageMaker SDK and notebook examples repository**

1. From the Jupyter Lab view in Amazon SageMaker, go to the File Browser at the top of the left toolbar\. From the file browser panel you can see a new navigation at the top of the panel\. 

1. Choose the icon on the far right to clone a git repository\. 

1. Add the repository URL: [https://github\.com/awslabs/amazon\-sagemaker\-examples\.git](https://github.com/awslabs/amazon-sagemaker-examples.git) 

1. Browse the newly added folder and its contents\. The DGL examples are stored in the sagemaker\-python\-sdk folder\. 

## Run a Graph Network Training Example<a name="deep-graph-library-run-a-graph-network-training-example"></a>

**To train a deep graph network**

1. From the Jupyter Lab view in Amazon SageMaker, browse the [example notebooks](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-python-sdk) and look for dgl folders\. Several files may be included to support an example\. Examine the README for any prerequisites\. 

1. Run the \.ipynb notebook example\.  

1. Find the estimator function, and note the line where it is using an Amazon ECR container for DGL and a specific instance type\. You may want to update this to use a container in your preferred Region\. 

1. Run the function to launch the instance and use the DGL container for training a graph network\. Charges are incurred for launching this instance\. The instance self\-terminates when the training is complete\. 

### Examples<a name="deep-graph-library-examples"></a>

 An example of knowledge graph embedding \(KGE\) is provided\. It uses the Freebase dataset, a knowledge base of general facts\. An example use case would be to graph the relationships of persons and predict their nationality\.  

 An example implementation of a graph convolutional network \(GCN\) shows how you can train a graph network to predict toxicity\. A physiology dataset, Tox21, provides toxicity measurements for how substances affect biological responses\.  

 Another GCN example shows you how to train a graph network on a scientific publications bibliography dataset, known as Cora\. You can use it to find relationships between authors, topics, and conferences\. 

 The last example is a recommender system for movie reviews\. It uses a graph convolutional matrix completion \(GCMC\) network trained on the MovieLens datasets\. These datasets consist of movie titles, genres, and ratings by users\. 

### Use a Deep Learning Container with DGL<a name="deep-graph-library-deep-learning-container"></a>

 The following examples use preconfigured deep learning containers\. These are the easiest to try since they work out\-of\-the\-box on Amazon SageMaker\. 
+ [Semi\-supervised classification of a knowledge base using a GCN](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-python-sdk/dgl_gcn) 
+ [Learning embeddings of large\-scale knowledge graphs using a dataset of scientific publications](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-python-sdk/dgl_kge)  

### Bring Your Own Container with DGL<a name="deep-graph-library-bring-your-own-container"></a>

The following examples enable you to bring your own container \(BYOC\)\. Read the [BYOC guide](https://docs.aws.amazon.com/sagemaker/latest/dg/your-algorithms.html) and familiarize yourself with that process before trying these\. Configuration is required\. 
+ [Molecular property prediction of toxicity using a GCN](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-python-sdk/dgl_gcn_tox21) 
+ [Recommender system for movies using a GCMC implementation](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-python-sdk/dgl_gcmc) 