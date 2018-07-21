# BlazingText Hyperparameters<a name="blazingtext_hyperparameters"></a>

When you start a training job with a `CreateTrainingJob` request, you specify a training algorithm\. You can also specify algorithm\-specific hyperparameters as string\-to\-string maps\. As BlazingText can be used in two different modes, *Word2Vec \(unsupervised\)* and *Text Classification \(supervised\)*, the hyper\-parameters are itemized in separate sections\.

## Word2Vec Hyperparameters<a name="blazingtext_hyperparameters_word2vec"></a>

The following table lists the hyperparameters for the BlazingText training algorithm provided by Amazon SageMaker\.


| Parameter Name | Description | 
| --- | --- | 
| mode |  The Word2vec architecture used for training\. Required\. Valid values: `batch_skipgram`, `skipgram`, or `cbow` Default value: \-  | 
| min\_count |  Discard words that appear less than `min_count` times\. Valid values: Non\-negative integer Default value: 5  | 
| learning\_rate |  Step size used for parameter updates\.  Valid values: Positive float Default value: 0\.05  | 
| window\_size |  The size of the context window\. The context window is the number of words surrounding the target word used for training\. Valid values: Positive integer Default value: 5  | 
| vector\_dim |  The dimension of the word vectors that the algorithm learns\. Valid values: Positive integer Default value: 100  | 
| epochs |  The number of complete passes through the training data\. Valid values: Positive integer Default value: 5  | 
| negative\_samples | Number of negative samples for the negative sample sharing strategy\.Valid values: Positive integerDefault value: 5 | 
| batch\_size |  The size of each batch when `mode` is set to `batch_skipgram`\. Set to a number between 10 and 20\. Valid values: Positive integer Default value: 11  | 
| sampling\_threshold |  Threshold for occurrence of words\. Those that appear with higher frequency in the training data are randomly down\-sampled\. Valid values: Positive fraction\. Recommended range is \(0, 1e\-3\] Default value: 0\.0001  | 
| subwords |  Whether to learn subword embeddings on not\. Valid values: \(Boolean\) `True` or `False` Default value: `False`  | 
| buckets |  Number of hash buckets to use for subwords\. Valid values: positive integer Default value: 2000000  | 
| min\_char |  Minimum number of characters to use for subwords/character n\-grams\. Valid values: positive integer Default value: 3  | 
| max\_char |  Maximum number of characters to use for subwords/character n\-grams Valid values: positive integer Default value: 6  | 
| evaluation |  Whether the trained model is evaluated using the [WordSimilarity\-353 Test](https://www.cs.technion.ac.il/~gabr/resources/data/wordsim353/)\. Valid values: \(Boolean\) `True` or `False` Default value: `True`  | 

## Text Classification Hyperparameters<a name="blazingtext_hyperparameters_text_class"></a>

The following table lists the hyperparameters for the Text Classification training algorithm provided by Amazon SageMaker\.

**Note**  
Although some of the parameters are common between the Text Classification and Word2Vec modes, they might have different meanings depending on the context\.


| Parameter Name | Description | 
| --- | --- | 
| mode |  Training mode\. Required\. Valid values: `supervised` Default value: \-  | 
| min\_count |  Discard words that appear less than `min_count` times\. Valid values: Non\-negative integer Default value: 5  | 
| learning\_rate |  Step size used for parameter updates\.  Valid values: Positive float Default value: 0\.05  | 
| vector\_dim |  Dimension of embedding layer\. Valid values: Positive integer Default value: 100  | 
| epochs |  The maximum number of complete passes through the training data\. Valid values: positive integer Default value: 5  | 
| word\_ngrams |  Number of word n\-grams features to use\. Valid values: positive integer Default value: 2  | 
| buckets |  Number of hash buckets to use for word n\-grams\. Valid values: positive integer Default value: 2000000  | 
| early\_stopping |  Whether to stop training if validation accuracy doesn't improve after an `patience` number of epochs\. Valid values: \(Boolean\) `True` or `False` Default value: `False`  | 
| min\_epochs |  Minimum number of epochs to train before early stopping logic is invoked\. Valid values: positive integer Default value: 5  | 
| patience |  Number of epochs to wait before early stop if no progress is made on the validation set\. Used only when `early_stopping` is `True`\. Valid values: positive integer Default value: 4  | 