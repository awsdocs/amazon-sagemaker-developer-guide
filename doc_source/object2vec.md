# Object2Vec Algorithm<a name="object2vec"></a>

The Amazon SageMaker Object2Vec algorithm is a general\-purpose neural embedding algorithm that is highly customizable\. It can learn low\-dimensional dense embeddings of high\-dimensional objects\. The embeddings are learned in a way that preserves the semantics of the relationship between pairs of objects in the original space in the embedding space\. You can use the learned embeddings to efficiently compute nearest neighbors of objects and to visualize natural clusters of related objects in low\-dimensional space, for example\. You can also use the embeddings as features of the corresponding objects in downstream supervised tasks, such as classification or regression\. 

Object2Vec generalizes the well\-known Word2Vec embedding technique for words that is optimized in the SageMaker [BlazingText algorithm](blazingtext.md)\. For a blog post that discusses how to apply Object2Vec to some practical use cases, see [Introduction to Amazon SageMaker Object2Vec](https://aws.amazon.com/blogs/machine-learning/introduction-to-amazon-sagemaker-object2vec/)\. 

**Topics**
+ [I/O Interface for the Object2Vec Algorithm](#object2vec-inputoutput)
+ [EC2 Instance Recommendation for the Object2Vec Algorithm](#object2vec--instances)
+ [Object2Vec Sample Notebooks](#object2vec-sample-notebooks)
+ [How Object2Vec Works](object2vec-howitworks.md)
+ [Object2Vec Hyperparameters](object2vec-hyperparameters.md)
+ [Tune an Object2Vec Model](object2vec-tuning.md)
+ [Data Formats for Object2Vec Training](object2vec-training-formats.md)
+ [Data Formats for Object2Vec Inference](object2vec-inference-formats.md)
+ [Encoder Embeddings for Object2Vec](object2vec-encoder-embeddings.md)

## I/O Interface for the Object2Vec Algorithm<a name="object2vec-inputoutput"></a>

You can use Object2Vec on many input data types, including the following examples\.


| Input Data Type | Example | 
| --- | --- | 
|  Sentence\-sentence pairs  | "A soccer game with multiple males playing\." and "Some men are playing a sport\." | 
|  Labels\-sequence pairs  | The genre tags of the movie "Titanic", such as "Romance" and "Drama", and its short description: "James Cameron's Titanic is an epic, action\-packed romance set against the ill\-fated maiden voyage of the R\.M\.S\. Titanic\. She was the most luxurious liner of her era, a ship of dreams, which ultimately carried over 1,500 people to their death in the ice cold waters of the North Atlantic in the early hours of April 15, 1912\." | 
|  Customer\-customer pairs  |  The customer ID of Jane and customer ID of Jackie\.  | 
|  Product\-product pairs  |  The product ID of football and product ID of basketball\.  | 
|  Item review user\-item pairs  |  A user's ID and the items she has bought, such as apple, pear, and orange\.  | 

To transform the input data into the supported formats, you must preprocess it\. Currently, Object2Vec natively supports two types of input: 
+ A discrete token, which is represented as a list of a single `integer-id`\. For example, `[10]`\.
+ A sequences of discrete tokens, which is represented as a list of `integer-ids`\. For example, `[0,12,10,13]`\.

The object in each pair can be asymmetric\. For example, the pairs can be \(token, sequence\) or \(token, token\) or \(sequence, sequence\)\. For token inputs, the algorithm supports simple embeddings as compatible encoders\. For sequences of token vectors, the algorithm supports the following as encoders:
+  Average\-pooled embeddings
+  Hierarchical convolutional neural networks \(CNNs\),
+  Multi\-layered bidirectional long short\-term memory \(BiLSTMs\) 

The input label for each pair can be one of the following:
+ A categorical label that expresses the relationship between the objects in the pair 
+ A score that expresses the strength of the similarity between the two objects 

For categorical labels used in classification, the algorithm supports the cross\-entropy loss function\. For ratings/score\-based labels used in regression, the algorithm supports the mean squared error \(MSE\) loss function\. Specify these loss functions with the `output_layer` hyperparameter when you create the model training job\.

## EC2 Instance Recommendation for the Object2Vec Algorithm<a name="object2vec--instances"></a>

The type of Amazon Elastic Compute Cloud \(Amazon EC2\) instance that you use depends on whether you are training or running inferences\. 

### Instance Recommendation for Training<a name="object2vec--instances-training"></a>

When training a model using the Object2Vec algorithm on a CPU, start with an ml\.m5\.2xlarge instance\. For training on a GPU, start with an ml\.p2\.xlarge instance\. If the training takes too long on this instance, you can use a larger instance, such as an ml\.m5\.4xlarge or an ml\.m5\.12xlarge instance Currently, the Object2Vec algorithm can train only on a single machine\. However, it does offer support for multiple GPUs\.

### Instance Recommendation for Inference<a name="object2vec--instances-inference"></a>

For inference with a trained Object2Vec model that has a deep neural network, we recommend using ml\.p3\.2xlarge GPU instance\. Due to GPU memory scarcity, the `INFERENCE_PREFERRED_MODE` environment variable can be specified to optimize on whether the [GPU optimization: Classification or Regression](object2vec-inference-formats.md#object2vec-inference-gpu-optimize-classification) or [GPU optimization: Encoder Embeddings](object2vec-encoder-embeddings.md#object2vec-inference-gpu-optimize-encoder-embeddings) inference network is loaded into GPU\.

## Object2Vec Sample Notebooks<a name="object2vec-sample-notebooks"></a>

For a sample notebook that uses the SageMaker Object2Vec algorithm to encode sequences into fixed\-length embeddings, see [Using Object2Vec to Encode Sentences into Fixed Length Embeddings](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/introduction_to_amazon_algorithms/object2vec_sentence_similarity/)\. For a sample notebook that uses the Object2Vec algorithm in a multi\-label prediction setting to predict the genre of a movie from its plot description, see [Movie genre prediction with Object2Vec Algorithm](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/object2vec_multilabel_genre_classification/object2vec_multilabel_genre_classification.ipynb)\. For a sample notebook that uses the Object2Vec algorithm to make movie recommendations, see [An Introduction to SageMaker ObjectToVec model for MovieLens recommendation](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/object2vec_movie_recommendation/object2vec_movie_recommendation.ipynb)\. For a sample notebook that uses the Object2Vec algorithm to learn document embeddings, see [Using Object2Vec to learn document embeddings](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/introduction_to_applying_machine_learning/object2vec_document_embedding/object2vec_document_embedding.ipynb)\. For instructions on how to create and access Jupyter notebook instances that you can use to run the example in SageMaker, see [Use Amazon SageMaker Notebook Instances](nbi.md)\. After you have created a notebook instance and opened it, choose **SageMaker Examples** to see a list of SageMaker samples\. To open a notebook, choose its **Use** tab and choose **Create copy**\.