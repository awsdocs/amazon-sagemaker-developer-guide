# BlazingText Hyperparameters<a name="blazingtext_hyperparameters"></a>

When you start a training job with a `CreateTrainingJob` request, you specify a training algorithm\. You can also specify algorithm\-specific hyperparameters as string\-to\-string maps\. 

The following table lists the hyperparameters for the BlazingText training algorithm provided by Amazon SageMaker\.


| Parameter Name | Description | 
| --- | --- | 
| mode |  The Word2vec architecture used for training\. Required\. Valid values: `batch_skipgram`, `skipgram`, or `cbow` Default value: \-  | 
| min\_count |  The minimum number of times a word must appear in the collection to be included in training and output\. Valid values: Non\-negative integer Default value: 5  | 
| learning\_rate |  The initial learning rate\. Valid values: Positive float Default value: 0\.05  | 
| window\_size |  The size of the context window\. The context window is the number of words surrounding the target word used in training\. Valid values: Positive integer Default value: 5  | 
| vector\_dim |  The dimension of the word vectors that the algorithm learns\. Valid values: Positive integer Default value: 100  | 
| epochs |  The number of complete passes through the training data\. Valid values: Positive integer Default value: 5  | 
| negative\_samples |  The number of noise words in the output layer for which the algorithm  updates weights\. Valid values: Positive integer Default value: 5  | 
| batch\_size |  The size of each batch when `mode` is set to `batch_skipgram`\. Set to a number between 10 and 20\. Valid values: Positive integer Default value: 11  | 
| sampling\_threshold |  To counter the imbalance between rare and very frequent words, words with frequency higher than the value of this parameter are discarded with some probability\. Valid values: Positive fraction\. Suggested values are less than \.001\. Default value: \.0001  | 
| evaluation |  Whether the trained model is evaluated using the WordSim353 test\. Valid values: *true* or *false*\. Default value: true  | 