# Latent Dirichlet Allocation \(LDA\) Algorithm<a name="lda"></a>

The Amazon SageMaker Latent Dirichlet Allocation \(LDA\) algorithm is an unsupervised learning algorithm that attempts to describe a set of observations as a mixture of distinct categories\. LDA is most commonly used to discover a user\-specified number of topics shared by documents within a text corpus\. Here each observation is a document, the features are the presence \(or occurrence count\) of each word, and the categories are the topics\. Since the method is unsupervised, the topics are not specified up front, and are not guaranteed to align with how a human may naturally categorize documents\. The topics are learned as a probability distribution over the words that occur in each document\. Each document, in turn, is described as a mixture of topics\.

The exact content of two documents with similar topic mixtures will not be the same\. But overall, you would expect these documents to more frequently use a shared subset of words, than when compared with a document from a different topic mixture\. This allows LDA to discover these word groups and use them to form topics\. As an extremely simple example, given a set of documents where the only words that occur within them are: *eat*, *sleep*, *play*, *meow*, and *bark*, LDA might produce topics like the following:


| **Topic** | *eat* | *sleep*  | *play* | *meow* | *bark* | 
| --- | --- | --- | --- | --- | --- | 
| Topic 1  | 0\.1  | 0\.3  | 0\.2  | 0\.4  | 0\.0  | 
| Topic 2  | 0\.2  | 0\.1 | 0\.4  | 0\.0  | 0\.3  | 

You can infer that documents that are more likely to fall into Topic 1 are about cats \(who are more likely to *meow* and *sleep*\), and documents that fall into Topic 2 are about dogs \(who prefer to *play* and *bark*\)\. These topics can be found even though the words dog and cat never appear in any of the texts\. 

**Topics**
+ [Input/Output Interface for the LDA Algorithm](#lda-inputoutput)
+ [EC2 Instance Recommendation for the LDA Algorithm](#lda-instances)
+ [LDA Sample LDA Notebooks](#LDA-sample-notebooks)
+ [How LDA Works](lda-how-it-works.md)
+ [LDA Hyperparameters](lda_hyperparameters.md)
+ [Tune an LDA Model](lda-tuning.md)

## Input/Output Interface for the LDA Algorithm<a name="lda-inputoutput"></a>

LDA expects data to be provided on the train channel, and optionally supports a test channel, which is scored by the final model\. LDA supports both `recordIO-wrapped-protobuf` \(dense and sparse\) and `CSV` file formats\. For `CSV`, the data must be dense and have dimension equal to *number of records \* vocabulary size*\. LDA can be trained in File or Pipe mode when using recordIO\-wrapped protobuf, but only in File mode for the `CSV` format\.

For inference, `text/csv`, `application/json`, and `application/x-recordio-protobuf` content types are supported\. Sparse data can also be passed for `application/json` and `application/x-recordio-protobuf`\. LDA inference returns `application/json` or `application/x-recordio-protobuf` *predictions*, which include the `topic_mixture` vector for each observation\.

Please see the [LDA Sample LDA Notebooks](#LDA-sample-notebooks) for more detail on training and inference formats\.

## EC2 Instance Recommendation for the LDA Algorithm<a name="lda-instances"></a>

LDA currently only supports single\-instance CPU training\. CPU instances are recommended for hosting/inference\.

## LDA Sample LDA Notebooks<a name="LDA-sample-notebooks"></a>

For a sample notebook that shows how to train the SageMaker Latent Dirichlet Allocation algorithm on a dataset and then how to deploy the trained model to perform inferences about the topic mixtures in input documents, see the [An Introduction to SageMaker LDA](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/lda_topic_modeling/LDA-Introduction.ipynb)\. For instructions how to create and access Jupyter notebook instances that you can use to run the example in SageMaker, see [Use Amazon SageMaker Notebook Instances](nbi.md)\. Once you have created a notebook instance and opened it, select the **SageMaker Examples** tab to see a list of all the SageMaker samples\. The topic modeling example notebooks using the NTM algorithms are located in the **Introduction to Amazon algorithms** section\. To open a notebook, click on its **Use** tab and select **Create copy**\.