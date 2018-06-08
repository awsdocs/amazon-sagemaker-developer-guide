# BlazingText<a name="blazingtext"></a>

The Amazon SageMaker BlazingText algorithm is an implementation of the Word2vec algorithm, which learns high\-quality distributed vector representations of words in a large collection of documents\. Many natural language processing \(NLP\) applications use word embeddings that are trained on large collections of documents\. A word embedding represents each word in a collection of documents as a vector of numbers\. Words that are similar have vectors that are similar\. That is, their vectors have relatively short distances between them\. These pretrained vector representations provide information about word distributions that typically improves the generalization of other models that are subsequently trained on a limited amount of data\.

Most implementations of the Word2vec algorithm are optimized for multi\-core CPU architectures\. This makes it difficult to scale to large datasets\. With BlazingText, you can learn word embeddings on your own datasets\. Similar to Word2vec, it provides the Skipgram and continuous bag\-of\-words \(CBOW\) training architectures\.

 Amazon SageMaker provides the following features:
+ Acceleration of training on multiple GPUs using highly optimized CUDA kernels\. For more information, see [BlazingText: Scaling and Accelerating Word2Vec using Multiple GPUs](https://dl.acm.org/citation.cfm?doid=3146347.3146354)\.
+ A new batch\_skipgram mode that allows faster training and distributed computation across multiple CPU nodes\. Batch\_skipgram trains mini\-batches using the Negative Sample Sharing strategy to convert level\-1 Basic Linear Algebra Subprograms \(BLAS\) operations to level\-3 BLAS operations\. This conversion uses the multiply\-add instructions of modern architectures\. For more information, see [Parallelizing Word2Vec in Shared and Distributed Memory](https://arxiv.org/pdf/1604.04661.pdf)\.

## Input/Output Interface<a name="bt-inputoutput"></a>

BlazingText expects you to provide a single preprocessed text file on the train channel\. The text file must contain space\-separated tokens, and each line of the file should contain a single sentence\.

BlazingText outputs a text file named `vectors.txt`, which contains the trained word\-to\-vectors mapping\. If the value of the `evaluation` hyperparameter is `true`, BlazingText also creates a JSON file named `eval.json`\. This file contains the similarity evaluation results \(Spearman's rank correlation coefficients\) for the WordSim353 dataset\. BlazingText also reports the number of words from the WordSim353 dataset that are not present in the training document collection\.

BlazingText doesn't support model hosting or inference\.

For more detail on training formats, see the [ example notebook](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/blazingtext_word2vec_text8/blazingtext_word2vec_text8.ipynb)\.

## EC2 Instance Recommendation<a name="blazingtext-instances"></a>

For CBOW and skipgram modes, BlazingText supports single CPU and single GPU instances\. For batch\_skipgram mode, BlazingText supports single or multiple CPU instances\.

When training on multiple instances, set the value of the `S3DataDistributionType` field of the [S3DataSource](API_S3DataSource.md) object that you pass to [CreateTrainingJob](API_CreateTrainingJob.md) to *FullyReplicated*\. BlazingText takes care of distributing data across machines\.

For more information about the mathematics behind BlazingText, see [BlazingText: Scaling and Accelerating Word2Vec using Multiple GPUs](https://dl.acm.org/citation.cfm?doid=3146347.3146354)\.

**Topics**
+ [Input/Output Interface](#bt-inputoutput)
+ [EC2 Instance Recommendation](#blazingtext-instances)
+ [BlazingText Hyperparameters](blazingtext_hyperparameters.md)
+ [Tuning a BlazingText Model](blazingtext-tuning.md)