# Object2Vec<a name="object2vec"></a>

Object2Vec is a general\-purpose neural embedding algorithm that is highly customizable\. It can learn low\-dimensional dense embeddings of high\-dimensional objects\. The embeddings are learned in such a way that the semantics of the relationship between pairs of objects in the original space are preserved in the embedding space\. You can use the learned embeddings, for example, to efficiently compute nearest neighbors of objects and to visualize natural clusters of related objects in low\-dimensional space\. You can also use the embeddings as features of the corresponding objects in downstream supervised tasks, such as classification or regression\. 

**Topics**
+ [Object2Vec Sample Notebooks](#object2vec-sample-notebooks)
+ [Input/Output Interface](#object2vec-inputoutput)
+ [EC2 Instance Recommendation](#object2vec--instances)
+ [How Object2Vec Works](object2vec-howitworks.md)
+ [Object2Vec Hyperparameters](object2vec-hyperparameters.md)
+ [Tuning an Object2Vec Model](object2vec-tuning.md)
+ [Data Formats for Object2Vec Training](object2vec-training-formats.md)
+ [Data Formats for Object2Vec Inference](object2vec-inference-formats.md)
+ [Encoder Embeddings for Object2Vec](object2vec-encoder-embeddings.md)

## Object2Vec Sample Notebooks<a name="object2vec-sample-notebooks"></a>

For a sample notebook that uses the Amazon SageMaker Object2Vec algorithm to encode sequences into fixed\-length embeddings, see [Using Object2Vec to Encode Sentences into Fixed Length Embeddings](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/introduction_to_amazon_algorithms/object2vec_sentence_similarity/)\. For instructions how to create and access Jupyter notebook instances that you can use to run the example in Amazon SageMaker, see [Using Notebook Instances](nbi.md)\. After you have created a notebook instance and opened it, choose **SageMaker Examples** to see a list of Amazon SageMaker samples\. To open a notebook, choose its **Use** tab and choose **Create copy**\.

## Input/Output Interface<a name="object2vec-inputoutput"></a>

You can use Object2Vec on many different input data types, including the following:
+ Sentence\-sentence pairs
+ Labels\-sequence pairs
+ Customer\-customer pairs
+ Product\-product pairs
+ Item review user\-item pairs

Natively, Object2Vec currently supports two types of input: 
+ Discrete tokens, which are represented as a list consisting of a single `integer-id`\. For example, `[10]`\.
+ Sequences of discrete tokens, which are represented as lists of `integer-ids`\. For example, `[0,12,10,13]`\.

To transform the input data into the supported formats, you must preprocess it\.

The object in each pair can be asymmetric\. For example, they can be \(token, sequence\) pairs or \(token, token\) pairs or \(sequence, sequence\) pairs\. For token inputs, the algorithm supports simple embeddings as compatible encoders\. For sequences of token vectors, the algorithm supports the following as encoders:
+  Average\-pooled embeddings
+  Hierarchical convolutional neural networks \(CNNs\),
+  Multi\-layered bidirectional long short\-term memory \(BiLSTMs\) 

The input label for each pair can be a categorical label that expresses the relationship between the objects in the pair or it can be a rating/score that expresses the strength of the similarity between the two objects\. For categorical labels used in classification, the algorithm supports the cross\-entropy loss function\. For ratings/score\-based labels used in regression, the algorithm supports the mean squared error \(MSE\) loss function\. You specify these loss functions with the `output_layer` hyperparameter\.

## EC2 Instance Recommendation<a name="object2vec--instances"></a>

### Training<a name="object2vec--instances-training"></a>

To start, try running training on a CPU, using, for example, an ml\.m5\.2xlarge instance, or on a GPU using, for example, an ml\.p2\.xlarge instance\. Currently, the Object2Vec algorithm is only set up to train on a single machine\. However, it does offer support for multiple GPUs\.

### Inference<a name="object2vec--instances-inference"></a>

Inference requests from CPUs generally have a lower average latency than requests from GPUs because there is a tax on CPU\-to\-GPU communication when you use GPU hardware\. However, GPUs generally have higher throughput for larger batches\.