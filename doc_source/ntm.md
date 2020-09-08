# Neural Topic Model \(NTM\) Algorithm<a name="ntm"></a>

Amazon SageMaker NTM is an unsupervised learning algorithm that is used to organize a corpus of documents into *topics* that contain word groupings based on their statistical distribution\. Documents that contain frequent occurrences of words such as "bike", "car", "train", "mileage", and "speed" are likely to share a topic on "transportation" for example\. Topic modeling can be used to classify or summarize documents based on the topics detected or to retrieve information or recommend content based on topic similarities\. The topics from documents that NTM learns are characterized as a *latent representation* because the topics are inferred from the observed word distributions in the corpus\. The semantics of topics are usually inferred by examining the top ranking words they contain\. Because the method is unsupervised, only the number of topics, not the topics themselves, are prespecified\. In addition, the topics are not guaranteed to align with how a human might naturally categorize documents\.

Topic modeling provides a way to visualize the contents of a large document corpus in terms of the learned topics\. Documents relevant to each topic might be indexed or searched for based on their soft topic labels\. The latent representations of documents might also be used to find similar documents in the topic space\. You can also use the latent representations of documents that the topic model learns for input to another supervised algorithm such as a document classifier\. Because the latent representations of documents are expected to capture the semantics of the underlying documents, algorithms based in part on these representations are expected to perform better than those based on lexical features alone\.

Although you can use both the Amazon SageMaker NTM and LDA algorithms for topic modeling, they are distinct algorithms and can be expected to produce different results on the same input data\.

For more information on the mathematics behind NTM, see [Neural Variational Inference for Text Processing](https://arxiv.org/pdf/1511.06038.pdf)\.

**Topics**
+ [Input/Output Interface for the NTM Algorithm](#NTM-inputoutput)
+ [EC2 Instance Recommendation for the NTM Algorithm](#NTM-instances)
+ [NTM Sample Notebooks](#NTM-sample-notebooks)
+ [NTM Hyperparameters](ntm_hyperparameters.md)
+ [Tune an NTM Model](ntm-tuning.md)
+ [NTM Response Formats](ntm-in-formats.md)

## Input/Output Interface for the NTM Algorithm<a name="NTM-inputoutput"></a>

Amazon SageMaker Neural Topic Model supports four data channels: train, validation, test, and auxiliary\. The validation, test, and auxiliary data channels are optional\. If you specify any of these optional channels, set the value of the `S3DataDistributionType` parameter for them to `FullyReplicated`\. If you provide validation data, the loss on this data is logged at every epoch, and the model stops training as soon as it detects that the validation loss is not improving\. If you don't provide validation data, the algorithm stops early based on the training data, but this can be less efficient\. If you provide test data, the algorithm reports the test loss from the final model\. 

The train, validation, and test data channels for NTM support both `recordIO-wrapped-protobuf` \(dense and sparse\) and `CSV` file formats\. For `CSV` format, each row must be represented densely with zero counts for words not present in the corresponding document, and have dimension equal to: \(number of records\) \* \(vocabulary size\)\. You can use either File mode or Pipe mode to train models on data that is formatted as `recordIO-wrapped-protobuf` or as `CSV`\. The auxiliary channel is used to supply a text file that contains vocabulary\. By supplying the vocabulary file, users are able to see the top words for each of the topics printed in the log instead of their integer IDs\. Having the vocabulary file also allows NTM to compute the Word Embedding Topic Coherence \(WETC\) scores, a new metric displayed in the log that captures similarity among the top words in each topic effectively\. The `ContentType` for the auxiliary channel is `text/plain`, with each line containing a single word, in the order corresponding to the integer IDs provided in the data\. The vocabulary file must be named `vocab.txt` and currently only UTF\-8 encoding is supported\. 

For inference, `text/csv`, `application/json`, `application/jsonlines`, and `application/x-recordio-protobuf` content types are supported\. Sparse data can also be passed for `application/json` and `application/x-recordio-protobuf`\. NTM inference returns `application/json` or `application/x-recordio-protobuf` *predictions*, which include the `topic_weights` vector for each observation\.

See the [blog post](https://aws.amazon.com/blogs/machine-learning/amazon-sagemaker-neural-topic-model-now-supports-auxiliary-vocabulary-channel-new-topic-evaluation-metrics-and-training-subsampling/) and the companion [notebook](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/scientific_details_of_algorithms/ntm_topic_modeling/ntm_wikitext.ipynb) for more details on using the auxiliary channel and the WETC scores\. For more information on how to compute the WETC score, see [Coherence\-Aware Neural Topic Modeling](https://arxiv.org/pdf/1809.02687.pdf)\. We used the pairwise WETC described in this paper for the Amazon SageMaker Neural Topic Model\.

For more information on input and output file formats, see [NTM Response Formats](ntm-in-formats.md) for inference and the [NTM Sample Notebooks](#NTM-sample-notebooks)\.

## EC2 Instance Recommendation for the NTM Algorithm<a name="NTM-instances"></a>

NTM training supports both GPU and CPU instance types\. We recommend GPU instances, but for certain workloads, CPU instances may result in lower training costs\. CPU instances should be sufficient for inference\.

## NTM Sample Notebooks<a name="NTM-sample-notebooks"></a>

For a sample notebook that uses the SageMaker NTM algorithm to uncover topics in documents from a synthetic data source where the topic distributions are known, see the [Introduction to Basic Functionality of NTM](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/ntm_synthetic/ntm_synthetic.ipynb)\. For instructions how to create and access Jupyter notebook instances that you can use to run the example in SageMaker, see [Use Amazon SageMaker Notebook Instances](nbi.md)\. Once you have created a notebook instance and opened it, select the **SageMaker Examples** tab to see a list of all the SageMaker samples\. The topic modeling example notebooks using the NTM algorithms are located in the **Introduction to Amazon algorithms** section\. To open a notebook, click on its **Use** tab and select **Create copy**\.