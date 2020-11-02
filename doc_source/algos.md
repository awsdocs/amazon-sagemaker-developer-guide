# Use Amazon SageMaker Built\-in Algorithms<a name="algos"></a>

Amazon SageMaker provides a suite of built\-in algorithms to help data scientists and machine learning practitioners get started on training and deploying machine learning models quickly\. For someone that is new to SageMaker, choosing the right algorithm for your particular use case can be a challenging task\. The following table provides a quick cheat sheet that shows how you can start with an example problem or use case and find an appropriate built\-in algorithm offered by SageMaker that is valid for that problem type\. Additional guidance organized by learning paradigms \(supervised and unsupervised\) and important data domains \(text and images\) is provided in the sections following the table\.


**Table: Mapping use cases to built\-in algorithms**  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/algos.html)

For important information about Docker registry paths, data formats, recommenced Amazon EC2 instance types, and CloudWatch logs common to all of the built\-in algorithms provided by SageMaker, see [Common Information About Built\-in Algorithms](common-info-all-im-models.md)\.

The following sections provide additional guidance for the Amazon SageMaker built\-in algorithms grouped by the supervised and unsupervised learning paradigms to which they belong\. For descriptions of these learning paradigms and their associated problem types, see [Choose an AlgorithmChoose an algorithm](algorithms-choose.md)\. Sections are also provided for the SageMaker built\-in algorithms available to address two important machine learning domains: textual analysis and image processing\.
+ [Supervised Learning](#algorithms-built-in-supervised-learning)
+ [Unsupervised Learning](#algorithms-built-in-unsupervised-learning)
+ [Textual Analysis](#algorithms-built-in-text-analysis)
+ [Image Processing](#algorithms-built-in-image-processing)

## Supervised Learning<a name="algorithms-built-in-supervised-learning"></a>

Amazon SageMaker provides several built\-in general purpose algorithms that can be used for either classification or regression problems\.
+ [Linear learner algorithm](linear-learner.md)—learns a linear function for regression or a linear threshold function for classification\.
+ [Factorization Machines Algorithm](fact-machines.md)—an extension of a linear model that is designed to economically capture interactions between features within high\-dimensional sparse datasets\.
+ [XGBoost Algorithm](xgboost.md)—implementation of the gradient\-boosted trees algorithm that combines an ensemble of estimates from a set of simpler and weaker models\.
+ [K\-Nearest Neighbors \(k\-NN\) Algorithm](k-nearest-neighbors.md)—a non\-parametric method that uses the k nearest labeled points to assign a label to a new data point for classification or a predicted target value from the average of the k nearest points for regression\.

Amazon SageMaker also provides several built\-in supervised learning algorithms that are used for more specialized tasks during feature engineering and forecasting from time series data\.
+ [Object2Vec Algorithm](object2vec.md)—a new highly customizable multi\-purpose algorithm used for feature engineering\. It can learn low\-dimensional dense embeddings of high\-dimensional objects to produce features that improve training efficiencies for downstream models\. While this is a supervised algorithm, as it requires labeled data for training, there are many scenarios in which the relationship labels can be obtained purely from natural clusterings in data, without any explicit human annotation\.
+ [DeepAR Forecasting Algorithm](deepar.md)—a supervised learning algorithm for forecasting scalar \(one\-dimensional\) time series using recurrent neural networks \(RNN\)\.

## Unsupervised Learning<a name="algorithms-built-in-unsupervised-learning"></a>

Amazon SageMaker provides several built\-in algorithms that can be used for a variety of unsupervised learning tasks such as clustering, dimension reduction, pattern recognition, and anomaly detection\.
+ [Principal Component Analysis \(PCA\) Algorithm](pca.md)—reduces the dimensionality \(number of features\) within a dataset by projecting data points onto the first few principal components\. The objective is to retain as much information or variation as possible\. For mathematicians, principal components are eigenvectors of the data's covariance matrix
+ [K\-Means Algorithm](k-means.md)—finds discrete groupings within data, where members of a group are as similar as possible to one another and as different as possible from members of other groups\.
+ [IP Insights](ip-insights.md)—learns the usage patterns for IPv4 addresses\. It is designed to capture associations between IPv4 addresses and various entities, such as user IDs or account numbers\.
+ [Random Cut Forest \(RCF\) Algorithm](randomcutforest.md)—detects anomalous data points within a data set that diverge from otherwise well\-structured or patterned data\.

## Textual Analysis<a name="algorithms-built-in-text-analysis"></a>

SageMaker provides algorithms that are tailored to the analysis of textual documents used in natural language processing, document classification or summarization, topic modeling or classification, and language transcription or translation\.
+ [BlazingText algorithm](blazingtext.md)—a highly optimized implementation of the Word2vec and text classification algorithms that scale to large datasets easily\. It is useful for many downstream natural language processing \(NLP\) tasks\.
+ [Sequence\-to\-Sequence Algorithm](seq-2-seq.md)—a supervised algorithm commonly used for neural machine translation\. 
+ [Latent Dirichlet Allocation \(LDA\) Algorithm](lda.md)—an algorithm suitable for determining topics in a set of documents\. It is an *unsupervised algorithm*, which means that it doesn't use example data with answers during training\.
+ [Neural Topic Model \(NTM\) Algorithm](ntm.md)—another unsupervised technique for determining topics in a set of documents, using a neural network approach\.

## Image Processing<a name="algorithms-built-in-image-processing"></a>

SageMaker also provides image processing algorithms that are used for image classification, object detection, and computer vision\.
+ [Image Classification Algorithm](image-classification.md)—uses example data with answers \(referred to as a *supervised algorithm*\)\. Use this algorithm to classify images\.
+ [Semantic Segmentation Algorithm](semantic-segmentation.md)—provides a fine\-grained, pixel\-level approach to developing computer vision applications\.
+ [Object Detection Algorithm](object-detection.md)—detects and classifies objects in images using a single deep neural network\. It is a supervised learning algorithm that takes images as input and identifies all instances of objects within the image scene\.

**Topics**
+ [Supervised Learning](#algorithms-built-in-supervised-learning)
+ [Unsupervised Learning](#algorithms-built-in-unsupervised-learning)
+ [Textual Analysis](#algorithms-built-in-text-analysis)
+ [Image Processing](#algorithms-built-in-image-processing)
+ [Common Information About Built\-in Algorithms](common-info-all-im-models.md)
+ [BlazingText algorithm](blazingtext.md)
+ [DeepAR Forecasting Algorithm](deepar.md)
+ [Factorization Machines Algorithm](fact-machines.md)
+ [Image Classification Algorithm](image-classification.md)
+ [IP Insights](ip-insights.md)
+ [K\-Means Algorithm](k-means.md)
+ [K\-Nearest Neighbors \(k\-NN\) Algorithm](k-nearest-neighbors.md)
+ [Latent Dirichlet Allocation \(LDA\) Algorithm](lda.md)
+ [Linear learner algorithm](linear-learner.md)
+ [Neural Topic Model \(NTM\) Algorithm](ntm.md)
+ [Object2Vec Algorithm](object2vec.md)
+ [Object Detection Algorithm](object-detection.md)
+ [Principal Component Analysis \(PCA\) Algorithm](pca.md)
+ [Random Cut Forest \(RCF\) Algorithm](randomcutforest.md)
+ [Semantic Segmentation Algorithm](semantic-segmentation.md)
+ [Sequence\-to\-Sequence Algorithm](seq-2-seq.md)
+ [XGBoost Algorithm](xgboost.md)