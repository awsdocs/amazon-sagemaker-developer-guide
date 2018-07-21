# BlazingText<a name="blazingtext"></a>

The Amazon SageMaker BlazingText algorithm provides highly optimized implementations of the Word2vec and text classification algorithms\. Word2vec is useful for many downstream natural language processing \(NLP\) tasks such as sentiment analysis, named entity recognition, machine translation etc\. whereas text classification is an important task for applications like web search, information retrieval, ranking and document classification\.

The Word2vec algorithm maps words to high\-quality distributed vectors\. The resulting vector representation of a word is called a *word embedding*\. Words that are semantically similar correspond to vectors that are close together\. Word embeddings thus capture the semantic relationships between words\. Many natural language processing \(NLP\) applications learn word embeddings by training on large collections of documents\. These pretrained vector representations provide information about semantics and word distributions that typically improves the generalizability of other models that are subsequently trained on a more limited amount of data\. Most implementations of the Word2vec algorithm are optimized for multi\-core CPU architectures\. This makes it difficult to scale to large datasets\. But with BlazingText, you can scale to large datasets easily\. Similar to Word2vec, it provides the Skip\-gram and continuous bag\-of\-words \(CBOW\) training architectures\.

The implementation of supervised multi class/label text classification algorithm by BlazingText extends the fastText text classifier to leverage GPU acceleration using custom [CUDA ](https://docs.nvidia.com/cuda/index.html) kernels \.The model can be trained on more than a billion words in a couple of minutes using a multi\-core CPU or a GPU, while achieving performance on par with the state\-of\-the\-art deep learning text classification algorithms\.

 Amazon SageMaker BlazingText provides the following features:
+ Accelerated training of fastText text classifier on multi\-core CPUs or a GPU and Word2Vec on GPUs using highly optimized CUDA kernels\. For more information, see [BlazingText: Scaling and Accelerating Word2Vec using Multiple GPUs](https://dl.acm.org/citation.cfm?doid=3146347.3146354)\.
+ [Enriched Word Vectors with Subword Information](https://arxiv.org/abs/1607.04606) by learning vector representations for character n\-grams\. This approach enables BlazingText to generate meaningful vectors for out\-of\-vocabulary \(OOV\) words by representing their vectors as the sum of the character n\-gram \(subword\) vectors\.
+ A `batch_skipgram` `mode` for the Word2Vec algorithm that allows faster training and distributed computation across multiple CPU nodes\. The `batch_skipgram` `mode` does mini\-batching using Negative Sample Sharing strategy to convert level\-1 BLAS operations into level\-3 BLAS operations\. This efficiently leverages the multiply\-add instructions of modern architectures\. For more information, see [Parallelizing Word2Vec in Shared and Distributed Memory](https://arxiv.org/pdf/1604.04661.pdf)\.

To summarize, the following modes are supported by BlazingText on different types instances:


| Modes |  Word2Vec \(Unsupervised Learning\)  |  Text Classification \(Supervised Learning\)  | 
| --- | --- | --- | 
|  Single CPU instance  |  cbow Skip\-gram Batch Skip\-gram  |  supervised  | 
|  Single GPU instance \(with 1 or more GPUs\)  |  cbow Skip\-gram  |  supervised with one GPU  | 
|  Multiple CPU instances  |  Batch Skip\-gram  |  None  | 

For more information about the mathematics behind BlazingText, see [BlazingText: Scaling and Accelerating Word2Vec using Multiple GPUs](https://dl.acm.org/citation.cfm?doid=3146347.3146354)\.

**Topics**
+ [Input/Output Interface](#bt-inputoutput)
+ [EC2 Instance Recommendation](#blazingtext-instances)
+ [BlazingText Hyperparameters](blazingtext_hyperparameters.md)
+ [Tuning a BlazingText Model](blazingtext-tuning.md)

## Input/Output Interface<a name="bt-inputoutput"></a>

BlazingText expects a single preprocessed text file with space separated tokens and each line of the file should contain a single sentence\. If you need to train on multiple text files, concatenate them into one file and upload the file in the respective channel\.

### Training and Validation Data Format<a name="blazingtext-data-formats"></a>

#### Word2Vec algorithm \(`skipgram`, `cbow`, and `batch_skipgram` modes\)<a name="blazingtext-data-formats-word2vec"></a>

For Word2Vec training, upload the file under the *train* channel\. No other channels are supported\. The file should contain a training sentence per line\.

#### Text Classification algorithm \(`supervised` mode\)<a name="blazingtext-data-formats-text-class"></a>

For `supervised` mode, the training/validation file should contain a training sentence per line along with the labels\. Labels are words that are prefixed by the string *\_\_label\_\_* \. Here is an example of a training/validation file:

```
__label__4  linux ready for prime time , intel says , despite all the linux hype , the open-source movement has yet to make a huge splash in the desktop market . that may be about to change , thanks to chipmaking giant intel corp .

__label__2  bowled by the slower one again , kolkata , november 14 the past caught up with sourav ganguly as the indian skippers return to international cricket was shortlived .
```

**Note**  
The order of labels within the sentence does not matter\. 

Upload the training file under train channel and \(optionally\) the validation file under the validation channel\.

### Model artifacts and Inference<a name="blazingtext-artifacts-inference"></a>

#### Word2Vec algorithm \(`skipgram`, `cbow`, and `batch_skipgram` modes\)<a name="blazingtext--artifacts-inference-word2vec"></a>

For Word2Vec training, the model artifacts consist of *vectors\.txt* which contains words to vectors mapping and *vectors\.bin*, a binary used by BlazingText for hosting/inference\. *vectors\.txt* stores the vectors in a format that is compatible with other tools like Gensim and Spacy\. For example, a Gensim user is able to execute the following commands to load the vectors\.txt file:

```
from gensim.models import KeyedVectors
word_vectors = KeyedVectors.load_word2vec_format('vectors.txt', binary=False)
word_vectors.most_similar(positive=['woman', 'king'], negative=['man'])
word_vectors.doesnt_match("breakfast cereal dinner lunch".split())
```

If evaluation parameter is set `True`, then an additional file *eval\.json* is created, which contains the similarity evaluation results \(using Spearman’s rank correlation coefficients\) on [WS\-353 dataset](https://www.cs.technion.ac.il/~gabr/resources/data/wordsim353/)\. The number of words from the WS\-353 dataset that are not there in the training corpus are reported\.

For inference requests, the model accepts a JSON file containing a list of strings and returns a list of vectors\. If the word is not found in vocabulary, inference returns a vectors of zeros\. If subwords is set to `True` during training, the model is able to generate vectors for out\-of\-vocabulary \(OOV\) words\.

##### Sample JSON request<a name="word2vec-json-request"></a>

Mime\-type:` application/json`

```
{
"instances": ["word1", "word2", "word3"]
}
```

#### Text Classification algorithm \(`supervised` mode\)<a name="blazingtext-artifacts-inference-text-class"></a>

Training with supervised will output a *model\.bin* file that can be consumed by BlazingText hosting\. For inference, the BlazingText model accepts a JSON file containing a list of sentences and returns a list of corresponding predicted labels and probability scores\. Each sentence is expected to be a string with space separated tokens/words\.

##### Sample JSON request<a name="text-class-json-request"></a>

Mime\-type:` application/json`

```
{
 "instances": ["the movie was excellent", "i did not like the plot ."]
}
```

By default, the server will return only one prediction, the one with the highest probability\. For retrieving the top k predictions, you can set k in configuration as shown below:

```
{
 "instances": ["the movie was excellent", "i did not like the plot ."],
 "configuration": {"k": 2}
}
```

For BlazingText, the content\-type and accept parameters must be equal\. For batch transform, they both need to be `application/jsonlines`\. If they differ, the `Accept` field is ignored\. The format for input is as follows:

```
content-type: application/jsonlines

{"source": "source_0"}
{"source": "source_1"}

if you need to pass the value of k for top-k, then you can do it in the following way:

{"source": "source_0", "k": 2}
{"source": "source_1", "k": 3}
```

The format for output is as follows:

```
accept: application/jsonlines


{"prob": [prob_1], "label": ["__label__1"]}
{"prob": [prob_1], "label": ["__label__1"]}
                    
If you have passed the value of k to be more than 1, then response will be in this format:

{"prob": [prob_1, prob_2], "label": ["__label__1", "__label__2"]}
{"prob": [prob_1, prob_2], "label": ["__label__1", "__label__2"]}
```

For both supervised \(text classification\) and unsupervised \(Word2Vec\) modes, the binaries \(*\*\.bin*\) produced by BlazingText can be cross consumed by fastText and vice\-versa\. Binaries produced by BlazingText can by used by fastText and the model binaries created with fastText can be hosted using BlazingText\.

For more details on dataset formats and model hosting, see the example notebooks [here](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/blazingtext_text_classification_dbpedia/blazingtext_text_classification_dbpedia.ipynb), [here](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/blazingtext_hosting_pretrained_fasttext/blazingtext_hosting_pretrained_fasttext.ipynb), and [here](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/blazingtext_word2vec_subwords_text8/blazingtext_word2vec_subwords_text8.ipynb)\.

## EC2 Instance Recommendation<a name="blazingtext-instances"></a>

For `cbow` and `skipgram` modes, BlazingText supports single CPU and single GPU instances\. Both of these modes support learning of `subwords` embeddings\. To achieve the highest speed without compromising on accuracy, we recommend using a ml\.p3\.2xlarge instance\. 

For `batch_skipgram` mode, BlazingText supports single or multiple CPU instances\. When training on multiple instances, set the value of the `S3DataDistributionType` field of the [S3DataSource](API_S3DataSource.md) object that you pass to [CreateTrainingJob](API_CreateTrainingJob.md) to `FullyReplicated`\. BlazingText takes care of distributing data across machines\.

For the supervised text classification mode, a C5 instance is recommended if the training dataset is less than 2GB\. For larger datasets, use an instance with a single GPU \(ml\.p2\.xlarge or ml\.p3\.2xlarge\)\.

**Topics**
+ [Input/Output Interface](#bt-inputoutput)
+ [EC2 Instance Recommendation](#blazingtext-instances)
+ [BlazingText Hyperparameters](blazingtext_hyperparameters.md)
+ [Tuning a BlazingText Model](blazingtext-tuning.md)