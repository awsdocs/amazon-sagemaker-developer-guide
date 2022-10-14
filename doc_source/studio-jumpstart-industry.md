# Amazon SageMaker JumpStart Industry: Financial<a name="studio-jumpstart-industry"></a>

Use SageMaker JumpStart Industry: Financial solutions, models, and example notebooks to learn about SageMaker features and capabilities through curated one\-step solutions and example notebooks of industry\-focused machine learning \(ML\) problems\. The notebooks also walk through how to use the SageMaker JumpStart Industry Python SDK to enhance industry text data and fine\-tune pretrained models\.

**Topics**
+ [Amazon SageMaker JumpStart Industry Python SDK](#studio-jumpstart-industry-pysdk)
+ [Amazon SageMaker JumpStart Industry: Financial Solution](#studio-jumpstart-industry-solutions)
+ [Amazon SageMaker JumpStart Industry: Financial Models](#studio-jumpstart-industry-models)
+ [Amazon SageMaker JumpStart Industry: Financial Example Notebooks](#studio-jumpstart-industry-examples)
+ [Amazon SageMaker JumpStart Industry: Financial Blog Posts](#studio-jumpstart-industry-blogs)

## Amazon SageMaker JumpStart Industry Python SDK<a name="studio-jumpstart-industry-pysdk"></a>

SageMaker JumpStart provides processing tools for curating industry datasets and fine\-tuning pretrained models through its client library called SageMaker JumpStart Industry Python SDK\. For detailed API documentation of the SDK, and to learn more about processing and enhancing industry text datasets for improving the performance of state\-of\-the\-art models on SageMaker JumpStart, see the [SageMaker JumpStart Industry Python SDK open source documentation](https://sagemaker-jumpstart-industry-pack.readthedocs.io)\.

## Amazon SageMaker JumpStart Industry: Financial Solution<a name="studio-jumpstart-industry-solutions"></a>

SageMaker JumpStart Industry: Financial provides the following solution notebook:
+ **Corporate Credit Rating Prediction – Financial Services**

This SageMaker JumpStart Industry: Financial solution provides a template for a text\-enhanced corporate credit rating model\. It shows how to take a model based on numeric features \(in this case, Altman's famous 5 financial ratios\) combined with texts from SEC filings to achieve an improvement in the prediction of credit ratings\. In addition to the 5 Altman ratios, you can add more variables as needed or set custom variables\. This solution notebook shows how SageMaker JumpStart Industry Python SDK helps process NLP scoring of texts from SEC filings\. Furthermore, the solution demonstrates how to train a model using the enhanced dataset to achieve a best\-in\-class model, deploy the model to a SageMaker endpoint for production, and receive improved predictions in real time\.

**Note**  
The solution notebooks are for demonstration purposes only\. They should not be relied on as financial or investment advice\.

The following screenshot shows the solution card provided through the SageMaker JumpStart page on Studio\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-finance-solutions.png)

**Note**  
The SageMaker JumpStart Industry: Financial solutions, model cards, and example notebooks are hosted and runnable only through SageMaker Studio\. Log in to the [SageMaker console](https://console.aws.amazon.com/sagemaker), and launch SageMaker Studio\. For more information about how to find the solution card, see the previous topic at [SageMaker JumpStart](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-jumpstart.html)\.

## Amazon SageMaker JumpStart Industry: Financial Models<a name="studio-jumpstart-industry-models"></a>

SageMaker JumpStart Industry: Financial provides the following pretrained [Robustly Optimized BERT approach \(RoBERTa\)](https://arxiv.org/pdf/1907.11692.pdf) models:
+ **RoBERTa\-SEC\-Base **
+ **RoBERTa\-SEC\-WIKI\-Base **
+ **RoBERTa\-SEC\-Large **
+ **RoBERTa\-SEC\-WIKI\-Large **

The RoBERTa\-SEC\-Base and RoBERTa\-SEC\-Large models are the text embedding models based on [GluonNLP's RoBERTa model](https://nlp.gluon.ai/api/model.html#gluonnlp.model.RoBERTaModel) and pre\-trained on S&P 500 SEC 10\-K/10\-Q reports of the decade of the 2010's \(from 2010 to 2019\)\. In addition to these, SageMaker JumpStart Industry: Financial provides two more RoBERTa variations, RoBERTa\-SEC\-WIKI\-Base and RoBERTa\-SEC\-WIKI\-Large, which are pretrained on the SEC filings and common texts of Wikipedia\. 

By deploying the model cards through SageMaker JumpStart, you'll be able to access their corresponding notebooks\. The paired notebooks will walk you through how the pretrained models can be fine\-tuned for specific classification tasks on multimodal datasets, which are enhanced by the SageMaker JumpStart Industry Python SDK\.

**Note**  
The model notebooks are for demonstration purposes only\. They should not be relied on as financial or investment advice\.

The following screenshot shows the pretrained model cards provided through the SageMaker JumpStart page on Studio\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-finance-models.png)

**Note**  
The SageMaker JumpStart Industry: Financial solutions, model cards, and example notebooks are hosted and runnable only through SageMaker Studio\. Log in to the [SageMaker console](https://console.aws.amazon.com/sagemaker), and launch SageMaker Studio\. For more information about how to find the model cards, see the previous topic at [SageMaker JumpStart](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-jumpstart.html)\.

## Amazon SageMaker JumpStart Industry: Financial Example Notebooks<a name="studio-jumpstart-industry-examples"></a>

SageMaker JumpStart Industry: Financial provides the following hands\-on examples of solving industry\-focused ML problems:
+ **SEC Filings Retrieval w/ Summarizer and Scoring** – This example introduces how to use the SageMaker JumpStart Industry Python SDK for processing the SEC filings, such as text summarization and scoring texts based on NLP score types and their corresponding word lists\. To preview the content of this notebook, see [Simple Construction of a Multimodal Dataset from SEC Filings and NLP Scores](https://sagemaker-jumpstart-industry-pack.readthedocs.io/en/latest/notebooks/finance/notebook1/SEC_Retrieval_Summarizer_Scoring.html)
+ **ML on a TabText \(Multimodal\) Dataset** – This example shows how to merge different types of datasets into a single dataframe called TabText and perform multimodal ML\. To preview the content of this notebook, see [Machine Learning on a TabText Dataframe – An Example Based on the Paycheck Protection Program](https://sagemaker-jumpstart-industry-pack.readthedocs.io/en/latest/notebooks/finance/notebook2/PPP_TabText_ML.html)
+ **Multi\-category ML on SEC filings data** – This example shows how to train an AutoGluon NLP model over the multimodal \(TabText\) datasets curated from SEC filings for a multiclass classification task\. [Classify SEC 10K/Q Filings to Industry Codes Based on the MDNA Text Column](https://sagemaker-jumpstart-industry-pack.readthedocs.io/en/latest/notebooks/finance/notebook3/SEC_MNIST_ML.html)

**Note**  
The example notebooks are for demonstrative purposes only\. They should not be relied on as financial or investment advice\.

The following screenshot shows the example notebook cards provided through the SageMaker JumpStart page on Studio\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-finance-examples.png)

**Note**  
The SageMaker JumpStart Industry: Financial solutions, model cards, and example notebooks are hosted and runnable only through SageMaker Studio\. Log in to the [SageMaker console](https://console.aws.amazon.com/sagemaker), and launch SageMaker Studio\. For more information about how to find the example notebooks, see the previous topic at [SageMaker JumpStart](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-jumpstart.html)\.

To preview the content of the example notebooks, see [Tutorials – Finance](https://sagemaker-jumpstart-industry-pack.readthedocs.io/en/latest/notebooks/index.html) in the *SageMaker JumpStart Industry Python SDK documentation*\.

## Amazon SageMaker JumpStart Industry: Financial Blog Posts<a name="studio-jumpstart-industry-blogs"></a>

For thorough applications of using SageMaker JumpStart Industry: Financial solutions, models, examples, and the SDK, see the following blog posts:
+ [Use pre\-trained financial language models for transfer learning in Amazon SageMaker JumpStart](https://aws.amazon.com/blogs/machine-learning/use-pre-trained-financial-language-models-for-transfer-learning-in-amazon-sagemaker-jumpstart/)
+ [Use SEC text for ratings classification using multimodal ML in Amazon SageMaker JumpStart](https://aws.amazon.com/blogs/machine-learning/use-sec-text-for-ratings-classification-using-multimodal-ml-in-amazon-sagemaker-jumpstart/)