# Neural Topic Model \(NTM\)<a name="ntm"></a>

Amazon SageMaker NTM is an unsupervised learning algorithm that learns latent representations of large collections of discrete data, such as a corpus of documents\. The latent representation of each document is provided in terms of a probability distribution over a fixed set of aspects, often referred to as topics\. Each topic, in turn, can be represented in terms of a probability distribution over words in the vocabulary\. The semantics of topics are usually inferred by examining the top ranking words in each topic\. Because the method is unsupervised, only the number of topics, not the topics themselves, are prespecified\. In addition, the topics are not guaranteed to align with how a human might naturally categorize documents\.

Topic modeling provides a way to visualize the contents of a large document corpus in terms of the learned topics\. Documents relevant to each topic might be indexed or searched for based on their soft topic labels\. The latent representations of documents might also be used to find similar documents in the topic space\. You can also use the latent representations of documents that the topic model learns for input to another supervised algorithm such as a document classifier\. Because the latent representations of documents are expected to capture the semantics of the underlying documents algorithms based in part on these representations are expected to perform better than those based on lexical features alone\.

Although you can use both the Amazon SageMaker NTM and LDA algorithms for topic modeling, they are distinct algorithms and can be expected to produce different results on the same input data\. 

For more information on the mathematics behind NTM, see [Neural Variational Inference for Text Processing](https://arxiv.org/pdf/1511.06038.pdf)\.

## Input/Output Interface<a name="NTM-inputoutput"></a>

Amazon SageMaker Neural Topic Model supports three data channels: train, validation, and test\. The validation and test data channels are optional\. If you specify the validation channel, the test channel, or both, set the value of the `S3DataDistributionType` parameter for each of these channels to `FullyReplicated`\. If you provide validation data, the loss on this data is logged at every epoch, and the model stops training as soon as it detects that the validation loss is not improving\. If you don't provide validation data, the algorithm stops early based on the training data, but this can be less efficient\. If you provide test data, the algorithm reports the test loss from the final model\. NTM supports both `recordIO-wrapped-protobuf` \(dense and sparse\) and `CSV` file formats\. For `CSV` format, each row must be represented densely with zero counts for words not present in the corresponding document , and have the dimension `number of records * vocabulary size`\.

For inference, `text/csv`, `application/json`, and `application/x-recordio-protobuf` content types are supported\. Sparse data can also be passed for `application/json` and `application/x-recordio-protobuf`\. NTM inference returns `application/json` or `application/x-recordio-protobuf` *predictions*, which include the `topic_weights` vector for each observation\.

Please see the example notebooks for more detail on training and inference formats\.

## EC2 Instance Recommendation<a name="NTM-instances"></a>

NTM training supports both GPU and CPU instance types\. We recommend GPU instances, but for certain workloads, CPU instances may result in lower training costs\. CPU instances should be sufficient for inference\.

**Topics**
+ [Input/Output Interface](#NTM-inputoutput)
+ [EC2 Instance Recommendation](#NTM-instances)
+ [NTM Hyperparameters](ntm_hyperparameters.md)
+ [NTM Response Formats](ntm-in-formats.md)