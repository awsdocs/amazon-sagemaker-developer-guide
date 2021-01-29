# Choose an Algorithm<a name="algorithms-choose"></a>

Machine leaning can help you accomplish empirical tasks that require some sort of inductive inference\. This task involves induction as it uses data to train algorithms to make generalizable inferences\. This means that the algorithms can make statistically reliable predictions or decisions, or complete other tasks when applied to new data that was not used to train them\. 

To help you select the best algorithm for your task, we classify these tasks on various levels of abstraction\. At the highest level of abstraction, machine learning attempts to find patterns or relationships between features or less structured items, such as text in a data set\. Pattern recognition techniques can be classified into distinct machine learning paradigms, each of which address specific problem types\. There are currently three basic paradigms for machine learning used to address various problem types: 
+ [Supervised learning](#algorithms-choose-supervised-learning)
+ [Unsupervised learning](#algorithms-choose-unsupervised-learning)
+ [Reinforcement learning](#algorithms-choose-reinforcement-learning)

The types of problems that each learning paradigm can address are identified by considering the inferences \(or predictions, decisions, or other tasks\) you want to make from the type of data that you have or could collect\. Machine learning paradigms use algorithmic methods to address their various problem types\. The algorithms provide recipes for solving these problems\. 

However, many algorithms, such as neural networks, can be deployed with different learning paradigms and on different types of problems\. Multiple algorithms can also address a specific problem type\. Some algorithms are more generally applicable and others are quite specific for certain kinds of objectives and data\. So the mapping between machine learning algorithms and problem types is many\-to\-many\. Also, there are various implementation options available for algorithms\. 

The following sections provide guidance concerning implementation options, machine learning paradigms, and algorithms appropriate for different problem types\.

**Topics**
+ [Choose an algorithm implementation](#algorithms-choose-implementation)
+ [Problem types for the basic machine learning paradigms](#basic-machine-learning-paradigms)
+ [Use Amazon SageMaker Built\-in Algorithms](algos.md)
+ [Use reinforcement learning with Amazon SageMaker](reinforcement-learning.md)

## Choose an algorithm implementation<a name="algorithms-choose-implementation"></a>

After choosing an algorithm, you must decide which implementation of it you want to use\. Amazon SageMaker supports three implementation options that require increasing levels of effort\. 
+ **Built\-in algorithms** require the least effort and scale if the data set is large and significant resources are needed to train and deploy the model\.
+ If there is no built\-in solution that works, try to develop one that uses **pre\-made images for machine and deep learning frameworks** for supported frameworks such as Scikit\-Learn, TensorFlow, PyTorch, MXNet, or Chainer\.
+ If you need to run custom packages or use any code which isn’t a part of a supported framework or available via PyPi, then you need to build **your own custom Docker image** that is configured to install the necessary packages or software\. The custom image must also be pushed to an online repository like the Amazon Elastic Container Service\.

**Topics**
+ [Use a built\-in algorithm](#built-in-algorithms-benefits)
+ [Use script mode in a supported framework](#supported-frameworks-benefits)
+ [Use a custom Docker image](#custom-image-use-case)


**Algorithm implementation guidance**  

| Implementation | Requires code | Pre\-coded algorithms | Support for third party packages | Support for custom code | Level of effort | 
| --- | --- | --- | --- | --- | --- | 
| Built\-in | No | Yes | No | No | Low | 
| Scikit\-learn | Yes | Yes | PyPi only | Yes | Medium | 
| Spark ML | Yes | Yes | PyPi only | Yes | Medium | 
| XGBoost \(open source\) | Yes | Yes | PyPi only | Yes | Medium | 
| TensorFlow | Yes | No | PyPi only | Yes | Medium\-high | 
| PyTorch | Yes | No | PyPi only | Yes | Medium\-high | 
| MXNet | Yes | No | PyPi only | Yes | Medium\-high | 
| Chainer | Yes | No | PyPi only | Yes | Medium\-high | 
| Custom image | Yes | No | Yes, from any source | Yes | High | 

### Use a built\-in algorithm<a name="built-in-algorithms-benefits"></a>

When choosing an algorithm for your type of problem and data, the easiest option is to use one of Amazon SageMaker's built\-in algorithms\. These built\-in algorithms come with two major benefits\.
+ The built\-in algorithms require no coding to start running experiments\. The only inputs you need to provide are the data, hyperparameters, and compute resources\. This allows you to run experiments more quickly, with less overhead for tracking results and code changes\.
+ The built\-in algorithms come with parallelization across multiple compute instances and GPU support right out of the box for all applicable algorithms \(some algorithms may not be included due to inherent limitations\)\. If you have a lot of data with which to train your model, most built\-in algorithms can easily scale to meet the demand\. Even if you already have a pre\-trained model, it may still be easier to use its corollary in SageMaker and input the hyper\-parameters you already know than to port it over, using script mode on a supported framework\.

For more information on the built\-in algorithms provided by SageMaker, see [Use Amazon SageMaker Built\-in Algorithms](algos.md)\.

For important information about docker registry paths, data formats, recommended EC2 instance types, and &CW; logs common to all of the built\-in algorithms provided by SageMaker, see [Common Information About Built\-in Algorithms](common-info-all-im-models.md)\.

### Use script mode in a supported framework<a name="supported-frameworks-benefits"></a>

If the algorithm you want to use for your model is not supported by a built\-in choice and you are comfortable coding your own solution, then you should consider using an Amazon SageMaker supported framework\. This is referred to as "script mode" because you write your custom code \(script\) in a text file with a `.py` extension\. As the table above indicates, SageMaker supports most of the popular machine learning frameworks\. These frameworks come preloaded with the corresponding framework and some additional Python packages, such as Pandas and NumPy, so you can write your own code for training an algorithm\. These frameworks also allow you to install any Python package hosted on PyPi by including a requirements\.txt file with your training code or to include your own code directories\. R is also supported natively in SageMaker notebook kernels\. Some frameworks, like scikit\-learn and Spark ML, have pre\-coded algorithms you can use easily, while other frameworks like TensorFlow and PyTorch may require you to implement the algorithm yourself\. The only limitation when using a supported framework image is that you cannot import any software packages that are not hosted on PyPi or that are not already included with the framework’s image\.

For more information on the frameworks supported by SageMaker, see [Use Machine Learning Frameworks, Python, and R with Amazon SageMaker](frameworks.md)\.

### Use a custom Docker image<a name="custom-image-use-case"></a>

Amazon SageMaker's built\-in algorithms and supported frameworks should cover most use cases, but there are times when you may need to use an algorithm from a package not included in any of the supported frameworks\. You may also already have a pre\-trained model pickled or persisted somewhere which you need to deploy\. SageMaker uses Docker images to host the training and serving of all models, so you can supply your own custom Docker image if the package or software you need is not included in a supported framework\. This may be your own Python package or an algorithm coded in a language like Stan or Julia\. For these images you must also configure the training of the algorithm and serving of the model properly in your Dockerfile\. This requires intermediate knowledge of Docker and is not recommended unless you are comfortable writing your own machine learning algorithm\. Your Docker image must be uploaded to an online repository, such as the Amazon Elastic Container Service \(ECS\) before you can train and serve your model properly\.

 For more information on custom Docker images in SageMaker, see [Using Docker containers with SageMaker ](docker-containers.md)\.

## Problem types for the basic machine learning paradigms<a name="basic-machine-learning-paradigms"></a>

The following three sections describe the main problem types addressed by the three basic paradigms for machine learning\. For a list of the built\-in algorithms that SageMaker provides to address these problem types, see [Use Amazon SageMaker Built\-in Algorithms](algos.md)\.

**Topics**
+ [Supervised learning](#algorithms-choose-supervised-learning)
+ [Unsupervised learning](#algorithms-choose-unsupervised-learning)
+ [Reinforcement learning](#algorithms-choose-reinforcement-learning)

### Supervised learning<a name="algorithms-choose-supervised-learning"></a>

If your data set consists of features or attributes \(inputs\) that contain target values \(outputs\), then you have a supervised learning problem\. If your target values are categorical \(mathematically discrete\), then you have a **classification problem**\. It is a standard practice to distinguish binary from multiclass classification\. 
+ **Binary classification** is a type of supervised learning that assigns an individual to one of two predefined and mutually exclusive classes based on the individual's attributes\. It is supervised because the models are trained using examples in which the attributes are provided with correctly labeled objects\. A medical diagnosis for whether an individual has a disease or not based on the results of diagnostic tests is an example of binary classification\.
+ **Multiclass classification** is a type of supervised learning that assigns an individual to one of several classes based on the individual's attributes\. It is supervised because the models are trained using examples in which the attributes are provided with correctly labeled objects\. An example is the prediction of the topic most relevant to a text document\. A document may be classified as being about religion, politics, or finance, or as about one of several other predefined topic classes\.

If the target values you are trying to predict are mathematically continuous, then you have a **regression** problem\. Regression estimates the values of a dependent target variable based on one or more other variables or attributes that are correlated with it\. An example is the prediction of house prices using features like the number of bathrooms and bedrooms and the square footage of the house and garden\. Regression analysis can create a model that takes one or more of these features as an input and predicts the price of a house\.

For more information on the built\-in supervised learning algorithms provided by SageMaker, see [Supervised Learning](algos.md#algorithms-built-in-supervised-learning)\.

### Unsupervised learning<a name="algorithms-choose-unsupervised-learning"></a>

If your data set consists of features or attributes \(inputs\) that do not contain labels or target values \(outputs\), then you have an unsupervised learning problem\. In this type of problem, the output must be predicted based on the pattern discovered in the input data\. The goal in unsupervised learning problems is to discover patterns such groupings within the data\. There are a large variety of tasks or problem types to which unsupervised learning can be applied\. Principal component and cluster analyses are two of the main methods commonly deployed for preprocessing data\. Here is a short list of problem types that can be addressed by unsupervised learning:
+ **Dimension reduction** is typically part of a data exploration step used to determine the most relevant features to use for model construction\. The idea is to transform data from a high\-dimensional, sparsely populated space into a low\-dimensional space that retains most significant properties of the original data\. This provides relief for the curse of dimensionality that can arise with sparsely populated, high\-dimensional data on which statistical analysis becomes problematic\. It can also be used to help understand data, reducing high\-dimensional data to a lower dimensionality that can be visualized\.
+ **Cluster analysis** is a class of techniques that are used to classify objects or cases into groups called clusters\. It attempts to find discrete groupings within data, where members of a group are as similar as possible to one another and as different as possible from members of other groups\. You define the features or attributes that you want the algorithm to use to determine similarity, select a distance function to measure similarity, and specify the number of clusters to use in the analysis\.
+ **Anomaly detection** is the identification of rare items, events, or observations in a data set which raise suspicions because they differ significantly from the rest of the data\. The identification of anomalous items can be used, for example, to detect bank fraud or medical errors\. Anomalies are also referred to as outliers, novelties, noise, deviations, and exceptions\.
+ **Density estimation** is the construction of estimates of unobservable underlying probability density functions based on observed data\. A natural use of density estimates is for data exploration\. Density estimates can discover features such as skewness and multimodality in the data\. The most basic form of density estimation is a rescaled histogram\.

SageMaker provides several built\-in machine learning algorithms that you can use for these unsupervised learning tasks\. For more information on the built\-in unsupervised algorithms provided by SageMaker, see [Unsupervised Learning](algos.md#algorithms-built-in-unsupervised-learning)\.

### Reinforcement learning<a name="algorithms-choose-reinforcement-learning"></a>

Reinforcement learning is a type of learning that is based on interaction with the environment\. This type of learning is used by an agent that must learn behavior through trial\-and\-error interactions with a dynamic environment in which the goal is to maximize the long\-term rewards that the agent receives as a result of its actions\. Rewards are maximized by trading off exploring actions that have uncertain rewards with exploiting actions that have known rewards\. 

For more information on SageMaker's frameworks, toolkits, and environments for reinforcement learning, see [Use reinforcement learning with Amazon SageMaker](reinforcement-learning.md)\.