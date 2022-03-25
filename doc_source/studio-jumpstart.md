# SageMaker JumpStart<a name="studio-jumpstart"></a>

**Note**  
 To use new features with an existing notebook instance or Studio app, you must restart the notebook instance or the Studio app to get the latest updates\. For more information, see [Update SageMaker Studio and Studio Apps](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-tasks-update.html)\.

 You can use SageMaker JumpStart to learn about SageMaker features and capabilities through curated one\-step solutions, example notebooks, and pretrained models that you can deploy\. You can also fine\-tune the models and deploy them\. 

You can access JumpStart using Amazon SageMaker Studio or programmatically through the SageMaker APIs\. The following steps show how to access JumpStart models and solutions using Amazon SageMaker Studio\. For information about how to use JumpStart models programmatically, see [Use Prebuilt Models with SageMaker JumpStart](https://sagemaker.readthedocs.io/en/stable/overview.html#use-prebuilt-models-with-sagemaker-jumpstart)\.

 Open JumpStart by using the JumpStart launcher in the **Get Started** section or by choosing the JumpStart icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-icon.png)\) in the *left sidebar*\. 

In the *file and resource browser* \(the left pane\), you can find JumpStart options\. From here, choose to browse JumpStart for solutions, models, notebooks, and other resources\. You can also view your currently launched solutions, endpoints, and training jobs\.

 To see what JumpStart has to offer, choose the JumpStart icon, and then choose **Browse JumpStart**\. JumpStart opens in a new tab in the *main work area*\. Here you can browse one\-step solutions, models, example notebooks, blogs, and video tutorials\. 

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-assets.png) 

**Important**  
Amazon SageMaker JumpStart makes certain content available from third\-party sources\. This content may be subject to separate license terms\. You are responsible for reviewing and complying with any applicable license terms and making sure they are acceptable for your use case before downloading or using the content\. 

## Using JumpStart<a name="jumpstart-using"></a>

 At the top of the JumpStart page, you can use search to look for topics of interest\.  

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-search.png) 

 You can find JumpStart resources by using search, or by browsing each category that follows the search panel: 
+  **Solutions** – Launch end\-to\-end machine learning solutions that tie SageMaker to other AWS services with one click\. 
+  **Text models** – Deploy and fine\-tune pretrained transformers for various natural language processing use cases\. 
+  **Vision models** – Deploy and fine\-tune pretrained models for image classification and object detection with one click\.
+  **SageMaker algorithms** – Train and deploy SageMaker built\-in Algorithms for various problem types with these example notebooks\. 
+  **Example notebooks** – Run example notebooks that use SageMaker features like Spot Instance training and experiments over a large variety of model types and use cases\. 
+  **Blogs** – Read deep dives and solutions from machine learning experts hosted by Amazon\. 
+  **Video tutorials** – Watch video tutorials for SageMaker features and machine learning use cases from machine learning experts hosted by Amazon\. 

## Solutions<a name="jumpstart-solutions"></a>

 When you choose a solution, JumpStart shows a description of the solution and a **Launch** button\. When you click **Launch**, JumpStart creates all of the resources necessary to run the solution, including training and model hosting instances\. After JumpStart launches the solution, JumpStart shows an **Open Notebook** button\. You can click the button to use the provided notebooks and explore the solution’s features\. As artifacts are generated during launch or after running the provided notebooks, they are listed in the **Generated Artifacts** table\. You can delete individual artifacts with the trash icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-trash.png)\)\. You can delete all of the solution’s resources by choosing **Delete solution resources**\.

 

## Models<a name="jumpstart-models"></a>

 Models are available for quick deployment directly from JumpStart\. You can also fine\-tune some of these models\. When you browse the models, you can scroll past the deploy and train sections to the **Description** section\. In the **Description** section, you can learn more about the model, including what you can do with the model\. You can also learn what types of inputs and outputs are expected, and the type of data that you need to use transfer learning to fine\-tune the model\. 

The following tables list the models currently offered in JumpStart\. The available models are sorted by their model type and task\. To view other model sets, click the task tab for those models\.

### Tabular Models<a name="jumpstart-models-tabular"></a>

------
#### [ Classification ]




| Model | Fine\-tunable | Source | 
| --- | --- | --- | 
| LightGBM Classification | Yes | [https://lightgbm\.readthedocs\.io/en/latest/](https://lightgbm.readthedocs.io/en/latest/) | 
| CatBoost Classification | Yes | [https://catboost\.ai/](https://catboost.ai/) | 
| XGBoost Classification | Yes | [https://xgboost\.readthedocs\.io/en/latest/](https://xgboost.readthedocs.io/en/latest/) | 
| Linear Classification | Yes | [https://scikit\-learn\.org/stable/](https://scikit-learn.org/stable/) | 

------
#### [ Regression ]




| Model | Fine\-tunable | Source | 
| --- | --- | --- | 
| LightGBM Regression | Yes | [https://lightgbm\.readthedocs\.io/en/latest/](https://lightgbm.readthedocs.io/en/latest/) | 
| CatBoost Regression | Yes | [https://catboost\.ai/](https://catboost.ai/) | 
| XGBoost Regression | Yes | [https://xgboost\.readthedocs\.io/en/latest/](https://xgboost.readthedocs.io/en/latest/) | 
| Linear Regression | Yes | [https://scikit\-learn\.org/stable/](https://scikit-learn.org/stable/) | 

------

### Text Models<a name="jumpstart-models-text"></a>

------
#### [ Text Classification ]




| Model | Fine\-tunable | Source | 
| --- | --- | --- | 
| BERT Base Cased | Yes | [Tensorflow Hub](https://tfhub.dev/tensorflow/bert_en_cased_L-12_H-768_A-12/2) | 
| BERT Base MEDLINE/PubMed | Yes | [Tensorflow Hub](https://tfhub.dev/google/experts/bert/pubmed/1) | 
| BERT Base Multilingual Cased | Yes | [Tensorflow Hub](https://tfhub.dev/tensorflow/bert_multi_cased_L-12_H-768_A-12/2) | 
| BERT Base Uncased | Yes | [Tensorflow Hub](https://tfhub.dev/tensorflow/bert_en_uncased_L-12_H-768_A-12/2) | 
| BERT Base WikiPedia and BookCorpus | Yes | [Tensorflow Hub](https://tfhub.dev/google/experts/bert/wiki_books/1) | 
| BERT Large Cased | Yes | [Tensorflow Hub](https://tfhub.dev/tensorflow/bert_en_cased_L-24_H-1024_A-16/2) | 
| BERT Large Cased Whole Word Masking | Yes | [Tensorflow Hub](https://tfhub.dev/tensorflow/bert_en_wwm_cased_L-24_H-1024_A-16/2) | 
| BERT Large Uncased Whole Word Masking | Yes | [Tensorflow Hub](https://tfhub.dev/tensorflow/bert_en_wwm_uncased_L-24_H-1024_A-16/2) | 
| ELECTRA\-Base\+\+ | Yes | [Tensorflow Hub](https://tfhub.dev/google/electra_base/1) | 
| ELECTRA\-Small\+\+ | Yes | [Tensorflow Hub](https://tfhub.dev/google/electra_small/1) | 

------
#### [ Text Embedding ]




| Model | Fine\-tunable | Source | 
| --- | --- | --- | 
| RoBERTa\-SEC\-Base | No | [GluonCV](https://nlp.gluon.ai/master/_modules/gluonnlp/models/roberta.html) | 
| BERT Base Uncased L\-2 H\-256 A\-4 | No | [TensorFlow Hub](https://tfhub.dev/tensorflow/small_bert/bert_en_uncased_L-2_H-256_A-4/2) | 
| RoBERTa\-SEC\-WIKI\-Base | No | [GluonCV](https://nlp.gluon.ai/master/_modules/gluonnlp/models/roberta.html) | 
| RoBERTa\-SEC\-Large | No | [GluonCV](https://nlp.gluon.ai/master/_modules/gluonnlp/models/roberta.html) | 
| RoBERTa\-SEC\-WIKI\-Large | No | [GluonCV](https://nlp.gluon.ai/master/_modules/gluonnlp/models/roberta.html) | 
| BERT Large Universal Sentence Encoder | No | [TensorFlow Hub](https://tfhub.dev/google/universal-sentence-encoder-cmlm/en-large/1) | 
| BERT Base Universal Sentence Encoder | No | [TensorFlow Hub](https://tfhub.dev/google/universal-sentence-encoder-cmlm/en-base/1) | 
| BERT Base Wikipedia and BooksCorpus Finetune SST\-2 | No | [TensorFlow Hub](https://tfhub.dev/google/experts/bert/wiki_books/sst2/2) | 
| BERT Base Uncased L\-6 H\-256 A\-4 | No | [TensorFlow Hub](https://tfhub.dev/tensorflow/small_bert/bert_en_uncased_L-6_H-256_A-4/2) | 
| BERT Base Wikipedia and BooksCorpus Finetune MNLI | No | [TensorFlow Hub](https://tfhub.dev/google/experts/bert/wiki_books/mnli/2) | 
| BERT Base Uncased L\-4 H\-768 A\-12 | No | [TensorFlow Hub](https://tfhub.dev/tensorflow/small_bert/bert_en_uncased_L-4_H-768_A-12/2) | 
| BERT Base Uncased L\-12 H\-512 A\-8 | No | [TensorFlow Hub](https://tfhub.dev/tensorflow/small_bert/bert_en_uncased_L-12_H-512_A-8/2) | 
| BERT Large Talking heads Gated Gelu | No | [TensorFlow Hub](https://tfhub.dev/tensorflow/talkheads_ggelu_bert_en_large/2) | 
| BERT Base Uncased L\-12 H\-256 A\-4 | No | [TensorFlow Hub](https://tfhub.dev/tensorflow/small_bert/bert_en_uncased_L-12_H-256_A-4/2) | 
| BERT Base Uncased L\-4 H\-512 A\-8 | No | [TensorFlow Hub](https://tfhub.dev/tensorflow/small_bert/bert_en_uncased_L-4_H-512_A-8/2) | 
| BERT Base Uncased L\-6 H\-768 A\-12 | No | [TensorFlow Hub](https://tfhub.dev/tensorflow/small_bert/bert_en_uncased_L-6_H-768_A-12/2) | 
| BERT Base Uncased L\-8 H\-512 A\-8 | No | [TensorFlow Hub](https://tfhub.dev/tensorflow/small_bert/bert_en_uncased_L-8_H-512_A-8/2) | 
| BERT Base Uncased L\-4 H\-128 A\-2 | No | [TensorFlow Hub](https://tfhub.dev/tensorflow/small_bert/bert_en_uncased_L-4_H-128_A-2/2) | 
| BERT Base Uncased L\-10 H\-768 A\-12 | No | [TensorFlow Hub](https://tfhub.dev/tensorflow/small_bert/bert_en_uncased_L-10_H-768_A-12/2) | 
| BERT Base Uncased L\-12 H\-128 A\-2 | No | [TensorFlow Hub](https://tfhub.dev/tensorflow/small_bert/bert_en_uncased_L-12_H-128_A-2/2) | 
| BERT Base Uncased L\-4 H\-256 A\-4 | No | [TensorFlow Hub](https://tfhub.dev/tensorflow/small_bert/bert_en_uncased_L-4_H-256_A-4/2) | 
| BERT Base Uncased L\-8 H\-768 A\-12 | No | [TensorFlow Hub](https://tfhub.dev/tensorflow/small_bert/bert_en_uncased_L-8_H-768_A-12/2) | 
| BERT Base Uncased L\-8 H\-256 A\-4 | No | [TensorFlow Hub](https://tfhub.dev/tensorflow/small_bert/bert_en_uncased_L-8_H-256_A-4/2) | 
| BERT Base Uncased L\-2 H\-512 A\-8 | No | [TensorFlow Hub](https://tfhub.dev/tensorflow/small_bert/bert_en_uncased_L-2_H-512_A-8/2) | 
| BERT Base Uncased L\-12 H\-768 A\-12 | No | [TensorFlow Hub](https://tfhub.dev/tensorflow/small_bert/bert_en_uncased_L-12_H-768_A-12/2) | 
| BERT Base Uncased L\-2 H\-768 A\-12 | No | [TensorFlow Hub](https://tfhub.dev/tensorflow/small_bert/bert_en_uncased_L-2_H-768_A-12/2) | 
| BERT Base Uncased L\-6 H\-512 A\-8 | No | [TensorFlow Hub](https://tfhub.dev/tensorflow/small_bert/bert_en_uncased_L-6_H-512_A-8/2) | 
| BERT Base Uncased L\-10 H\-512 A\-8 | No | [TensorFlow Hub](https://tfhub.dev/tensorflow/small_bert/bert_en_uncased_L-10_H-512_A-8/2) | 
| BERT Base Uncased L\-2 H\-128 A\-2 | No | [TensorFlow Hub](https://tfhub.dev/tensorflow/small_bert/bert_en_uncased_L-2_H-128_A-2/2) | 
| BERT Base Uncased L\-10 H\-128 A\-2 | No | [TensorFlow Hub](https://tfhub.dev/tensorflow/small_bert/bert_en_uncased_L-10_H-128_A-2/2) | 
| BERT Base Uncased L\-12 H\-768 A\-12 | No | [TensorFlow Hub](https://tfhub.dev/tensorflow/bert_en_uncased_L-12_H-768_A-12/4) | 
| BERT Base Uncased L\-6 H\-128 A\-2 | No | [TensorFlow Hub](https://tfhub.dev/tensorflow/small_bert/bert_en_uncased_L-6_H-128_A-2/2) | 
| BERT Base Uncased L\-10 H\-256 A\-4 | No | [TensorFlow Hub](https://tfhub.dev/tensorflow/small_bert/bert_en_uncased_L-10_H-256_A-4/2) | 
| BERT Base Talking heads Gated Gelu | No | [TensorFlow Hub](https://tfhub.dev/tensorflow/talkheads_ggelu_bert_en_base/2) | 

------
#### [ Text Generation ]




| Model | Fine\-tunable | Source | 
| --- | --- | --- | 
| DistilGPT 2 | No | [Hugging Face](https://huggingface.co/distilgpt2) | 
| GPT 2 | No | [Hugging Face](https://huggingface.co/gpt2) | 

------
#### [ Extractive Question Answering ]




| Model | Fine\-tunable | Source | 
| --- | --- | --- | 
| BERT Base Cased | Yes | [PyTorch Hub](https://pytorch.org/hub/huggingface_pytorch-transformers/) | 
| BERT Base Multilingual Cased | Yes | [PyTorch Hub](https://pytorch.org/hub/huggingface_pytorch-transformers/) | 
| BERT Base Multilingual Uncased | Yes | [PyTorch Hub](https://pytorch.org/hub/huggingface_pytorch-transformers/) | 
| BERT Base Uncased | Yes | [PyTorch Hub](https://pytorch.org/hub/huggingface_pytorch-transformers/) | 
| BERT Large Cased | Yes | [PyTorch Hub](https://pytorch.org/hub/huggingface_pytorch-transformers/) | 
| BERT Large Cased Whole Word Masking | Yes | [PyTorch Hub](https://pytorch.org/hub/huggingface_pytorch-transformers/) | 
| BERT Large Cased Whole Word Masking SQuAD | Yes | [PyTorch Hub](https://pytorch.org/hub/huggingface_pytorch-transformers/) | 
| BERT Large Uncased | Yes | [PyTorch Hub](https://pytorch.org/hub/huggingface_pytorch-transformers/) | 
| BERT Large Uncased Whole Word Masking | Yes | [PyTorch Hub](https://pytorch.org/hub/huggingface_pytorch-transformers/) | 
| BERT Large Uncased Whole Word Masking SQuAD | Yes | [PyTorch Hub](https://pytorch.org/hub/huggingface_pytorch-transformers/) | 
| DistilBERT Base Cased | Yes | [PyTorch Hub](https://pytorch.org/hub/huggingface_pytorch-transformers/) | 
| DistilBERT Base Multilingual Cased | Yes | [PyTorch Hub](https://pytorch.org/hub/huggingface_pytorch-transformers/) | 
| DistilBERT Base Uncased | Yes | [PyTorch Hub](https://pytorch.org/hub/huggingface_pytorch-transformers/) | 
| DistilRoBERTa Base | Yes | [PyTorch Hub](https://pytorch.org/hub/huggingface_pytorch-transformers/) | 
| RoBERTa Base | Yes | [PyTorch Hub](https://pytorch.org/hub/huggingface_pytorch-transformers/) | 
| RoBERTa Base OpenAI | Yes | [PyTorch Hub](https://pytorch.org/hub/huggingface_pytorch-transformers/) | 
| RoBERTa Large | Yes | [PyTorch Hub](https://pytorch.org/hub/huggingface_pytorch-transformers/) | 
| RoBERTa Large OpenAI | Yes | [PyTorch Hub](https://pytorch.org/hub/huggingface_pytorch-transformers/) | 

------
#### [ Machine Translation ]




| Model | Fine\-tunable | Source | 
| --- | --- | --- | 
| t5 Small en de | No | [Hugging Face](https://huggingface.co/t5-small) | 
| t5 Base en de | No | [Hugging Face](https://huggingface.co/t5-base) | 
| t5 Large en de | No | [Hugging Face](https://huggingface.co/t5-large) | 
| Helsinki opus en es | No | [Hugging Face](https://huggingface.co/Helsinki-NLP/opus-mt-en-es) | 
| Helsinki opus en vi | No | [Hugging Face](https://huggingface.co/Helsinki-NLP/opus-mt-en-vi) | 

------
#### [ Named Entity Recognition ]




| Model | Fine\-tunable | Source | 
| --- | --- | --- | 
| Distilbert Base cased | No | [Hugging Face](https://huggingface.co/elastic/distilbert-base-cased-finetuned-conll03-english) | 
| Distilbert Base Uncased | No | [Hugging Face](https://huggingface.co/elastic/distilbert-base-uncased-finetuned-conll03-english) | 

------
#### [ Sentence Pair Classification ]




| Model | Fine\-tunable | Source | 
| --- | --- | --- | 
| BERT Base Cased | Yes | [Hugging Face](https://huggingface.co/bert-base-cased) | 
| BERT Base Cased | Yes | [Tensorflow Hub](https://tfhub.dev/tensorflow/bert_en_cased_L-12_H-768_A-12/2) | 
| BERT Base MEDLINE/PubMed | Yes | [Tensorflow Hub](https://tfhub.dev/google/experts/bert/pubmed/1) | 
| BERT Base Multilingual Cased | Yes | [Hugging Face](https://huggingface.co/bert-base-multilingual-cased) | 
| BERT Base Multilingual Cased | Yes | [Tensorflow Hub](https://tfhub.dev/tensorflow/bert_multi_cased_L-12_H-768_A-12/2) | 
| BERT Base Multilingual Uncased | Yes | [Hugging Face](https://huggingface.co/bert-base-multilingual-uncased) | 
| BERT Base Uncased | Yes | [Hugging Face](https://huggingface.co/bert-base-uncased) | 
| BERT Base Uncased | Yes | [Tensorflow Hub](https://tfhub.dev/tensorflow/bert_en_uncased_L-12_H-768_A-12/2) | 
| BERT Base Wikipedia and BooksCorpus | Yes | [Tensorflow Hub](https://tfhub.dev/google/experts/bert/wiki_books/1) | 
| BERT Large Cased | Yes | [Hugging Face](https://huggingface.co/bert-large-cased) | 
| BERT Large Cased Whole Word Masking | Yes | [Hugging Face](https://huggingface.co/bert-large-cased-whole-word-masking) | 
| BERT Large Cased Whole Word Masking | Yes | [Tensorflow Hub](https://tfhub.dev/tensorflow/bert_en_wwm_cased_L-24_H-1024_A-16/2) | 
| BERT Large Uncased | Yes | [Hugging Face](https://huggingface.co/bert-large-uncased) | 
| BERT Large Uncased | Yes | [Tensorflow Hub](https://tfhub.dev/tensorflow/bert_en_uncased_L-24_H-1024_A-16/2) | 
| BERT Large Uncased Whole Word Masking | Yes | [Hugging Face](https://huggingface.co/bert-large-uncased-whole-word-masking) | 
| BERT Large Uncased Whole Word Masking | Yes | [Tensorflow Hub](https://tfhub.dev/tensorflow/bert_en_wwm_uncased_L-24_H-1024_A-16/2) | 
| DistilBERT Base Cased | Yes | [Hugging Face](https://huggingface.co/distilbert-base-cased) | 
| DistilBERT Base Multilingual Cased | Yes | [Hugging Face](https://huggingface.co/distilbert-base-multilingual-cased) | 
| DistilBERT Base Uncased | Yes | [Hugging Face](https://huggingface.co/distilbert-base-uncased) | 
| DistilRoBERTa Base | Yes | [Hugging Face](https://huggingface.co/distilroberta-base) | 
| ELECTRA\-Base\+\+ | Yes | [Tensorflow Hub](https://tfhub.dev/google/electra_base/1) | 
| ELECTRA\-Small\+\+ | Yes | [Tensorflow Hub](https://tfhub.dev/google/electra_small/1) | 
| RoBERTa Base | Yes | [Hugging Face](https://huggingface.co/roberta-base) | 
| RoBERTa Base OpenAI | Yes | [Hugging Face](https://huggingface.co/roberta-base-openai-detector) | 
| RoBERTa Large | Yes | [Hugging Face](https://huggingface.co/roberta-large) | 
| RoBERTa Large OpenAI | Yes | [Hugging Face](https://huggingface.co/roberta-large-openai-detector) | 
| XLM CLM English\-German | Yes | [Hugging Face](https://huggingface.co/xlm-clm-ende-1024) | 
| XLM MLM 15 XNLI Languages | Yes | [Hugging Face](https://huggingface.co/xlm-mlm-xnli15-1024) | 
| XLM MLM English\-German | Yes | [Hugging Face](https://huggingface.co/xlm-mlm-ende-1024) | 
| XLM MLM English\-Romanian | Yes | [Hugging Face](https://huggingface.co/xlm-mlm-enro-1024) | 
| XLM MLM TLM 15 XNLI Languages | Yes | [Hugging Face](https://huggingface.co/xlm-mlm-tlm-xnli15-1024) | 

------
#### [ Text Summarization ]




| Model | Fine\-tunable | Source | 
| --- | --- | --- | 
| Distilbart xsum 1\-1 | No | [Hugging Face](https://huggingface.co/sshleifer/distilbart-xsum-1-1) | 
| Bert Small2bert | No | [Hugging Face](https://huggingface.co/mrm8488/bert-small2bert-small-finetuned-cnn_daily_mail-summarization) | 
| Distilbart CNN 6\-6 | No | [Hugging Face](https://huggingface.co/sshleifer/distilbart-cnn-6-6) | 
| Distilbart xsum 12\-3 | No | [Hugging Face](https://huggingface.co/sshleifer/distilbart-xsum-12-3) | 
| Distilbart CNN 12\-6 | No | [Hugging Face](https://huggingface.co/sshleifer/distilbart-cnn-12-6) | 
| Bart Large CNN samsum | No | [Hugging Face](https://huggingface.co/philschmid/bart-large-cnn-samsum) | 
| Bigbird Pegasus Large Arxiv | No | [Hugging Face](https://huggingface.co/google/bigbird-pegasus-large-arxiv) | 
| Bigbird Pegasus Large Pubmed | No | [Hugging Face](https://huggingface.co/google/bigbird-pegasus-large-pubmed) | 

------

### Vision Models<a name="jumpstart-models-vision"></a>

------
#### [ Image Classification ]




| Model | Fine\-tunable | Source | 
| --- | --- | --- | 
| AlexNet | Yes | [PyTorch Hub](https://pytorch.org/hub/pytorch_vision_alexnet/) | 
| BiT\-M R101x1 | Yes | [Tensorflow Hub](https://tfhub.dev/google/bit/m-r101x1/ilsvrc2012_classification/1) | 
| BiT\-M R101x1 ImageNet\-21k | Yes | [Tensorflow Hub](https://tfhub.dev/google/bit/m-r101x1/imagenet21k_classification/1) | 
| BiT\-M R101x3 | Yes | [Tensorflow Hub](https://tfhub.dev/google/bit/m-r101x3/ilsvrc2012_classification/1) | 
| BiT\-M R101x3 ImageNet\-21k | Yes | [Tensorflow Hub](https://tfhub.dev/google/bit/m-r101x3/imagenet21k_classification/1) | 
| BiT\-M R50x1 | Yes | [Tensorflow Hub](https://tfhub.dev/google/bit/m-r50x1/ilsvrc2012_classification/1) | 
| BiT\-M R50x1 ImageNet\-21k | Yes | [Tensorflow Hub](https://tfhub.dev/google/bit/m-r50x1/imagenet21k_classification/1) | 
| BiT\-M R50x3 | Yes | [Tensorflow Hub](https://tfhub.dev/google/bit/m-r50x3/ilsvrc2012_classification/1) | 
| BiT\-M R50x3 ImageNet\-21k | Yes | [Tensorflow Hub](https://tfhub.dev/google/bit/m-r50x3/imagenet21k_classification/1) | 
| BiT\-S R101x1 | Yes | [Tensorflow Hub](https://tfhub.dev/google/bit/s-r101x1/ilsvrc2012_classification/1) | 
| BiT\-S R101x3 | Yes | [Tensorflow Hub](https://tfhub.dev/google/bit/s-r101x3/ilsvrc2012_classification/1) | 
| BiT\-S R50x1 | Yes | [Tensorflow Hub](https://tfhub.dev/google/bit/s-r50x1/ilsvrc2012_classification/1) | 
| BiT\-S R50x3 | Yes | [Tensorflow Hub](https://tfhub.dev/google/bit/s-r50x3/ilsvrc2012_classification/1) | 
| DenseNet 121 | Yes | [PyTorch Hub](https://pytorch.org/hub/pytorch_vision_densenet/) | 
| DenseNet 161 | Yes | [PyTorch Hub](https://pytorch.org/hub/pytorch_vision_densenet/) | 
| DenseNet 169 | Yes | [PyTorch Hub](https://pytorch.org/hub/pytorch_vision_densenet/) | 
| DenseNet 201 | Yes | [PyTorch Hub](https://pytorch.org/hub/pytorch_vision_densenet/) | 
| EfficientNet B0 | Yes | [Tensorflow Hub](https://tfhub.dev/google/efficientnet/b0/classification/1) | 
| EfficientNet B0 Lite | Yes | [Tensorflow Hub](https://tfhub.dev/tensorflow/efficientnet/lite0/classification/2) | 
| EfficientNet B1 | Yes | [Tensorflow Hub](https://tfhub.dev/google/efficientnet/b1/classification/1) | 
| EfficientNet B1 Lite | Yes | [Tensorflow Hub](https://tfhub.dev/tensorflow/efficientnet/lite1/classification/2) | 
| EfficientNet B2 | Yes | [Tensorflow Hub](https://tfhub.dev/google/efficientnet/b2/classification/1) | 
| EfficientNet B2 Lite | Yes | [Tensorflow Hub](https://tfhub.dev/tensorflow/efficientnet/lite2/classification/2) | 
| EfficientNet B3 | Yes | [Tensorflow Hub](https://tfhub.dev/google/efficientnet/b3/classification/1) | 
| EfficientNet B3 Lite | Yes | [Tensorflow Hub](https://tfhub.dev/tensorflow/efficientnet/lite3/classification/2) | 
| EfficientNet B4 | Yes | [Tensorflow Hub](https://tfhub.dev/google/efficientnet/b4/classification/1) | 
| EfficientNet B4 Lite | Yes | [Tensorflow Hub](https://tfhub.dev/tensorflow/efficientnet/lite4/classification/2) | 
| EfficientNet B5 | Yes | [Tensorflow Hub](https://tfhub.dev/google/efficientnet/b5/classification/1) | 
| EfficientNet B6 | Yes | [Tensorflow Hub](https://tfhub.dev/google/efficientnet/b6/classification/1) | 
| EfficientNet B7 | Yes | [Tensorflow Hub](https://tfhub.dev/google/efficientnet/b7/classification/1) | 
| GoogLeNet | Yes | [PyTorch Hub](https://pytorch.org/hub/pytorch_vision_googlenet/) | 
| Inception ResNet V2 | Yes | [Tensorflow Hub](https://tfhub.dev/google/imagenet/inception_resnet_v2/classification/4) | 
| Inception V1 | Yes | [Tensorflow Hub](https://tfhub.dev/google/imagenet/inception_v1/classification/4) | 
| Inception V2 | Yes | [Tensorflow Hub](https://tfhub.dev/google/imagenet/inception_v2/classification/4) | 
| Inception V3 | Yes | [Tensorflow Hub](https://tfhub.dev/google/imagenet/inception_v3/classification/4) | 
| Inception V3 Preview | Yes | [Tensorflow Hub](https://tfhub.dev/google/tf2-preview/inception_v3/classification/4) | 
| MobileNet V1 0\.25 128 | Yes | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v1_025_128/classification/4) | 
| MobileNet V1 0\.25 160 | Yes | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v1_025_160/classification/4) | 
| MobileNet V1 0\.25 192 | Yes | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v1_025_192/classification/4) | 
| MobileNet V1 0\.25 224 | Yes | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v1_025_224/classification/4) | 
| MobileNet V1 0\.50 128 | Yes | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v1_050_128/classification/4) | 
| MobileNet V1 0\.50 160 | Yes | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v1_050_160/classification/4) | 
| MobileNet V1 0\.50 192 | Yes | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v1_050_192/classification/4) | 
| MobileNet V1 0\.50 224 | Yes | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v1_050_224/classification/4) | 
| MobileNet V1 0\.75 128 | Yes | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v1_075_128/classification/4) | 
| MobileNet V1 0\.75 160 | Yes | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v1_075_160/classification/4) | 
| MobileNet V1 0\.75 192 | Yes | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v1_075_192/classification/4) | 
| MobileNet V1 0\.75 224 | Yes | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v1_075_224/classification/4) | 
| MobileNet V1 1\.00 128 | Yes | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v1_100_128/classification/4) | 
| MobileNet V1 1\.00 160 | Yes | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v1_100_160/classification/4) | 
| MobileNet V1 1\.00 192 | Yes | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v1_100_192/classification/4) | 
| MobileNet V1 1\.00 224 | Yes | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v1_100_224/classification/4) | 
| MobileNet V2 | Yes | [Tensorflow Hub](https://tfhub.dev/google/tf2-preview/mobilenet_v2/classification/4) | 
| MobileNet V2 | Yes | [PyTorch Hub](https://pytorch.org/hub/pytorch_vision_mobilenet_v2/) | 
| MobileNet V2 0\.35 224 | Yes | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v2_035_224/classification/4) | 
| MobileNet V2 0\.50 224 | Yes | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v2_050_224/classification/4) | 
| MobileNet V2 0\.75 224 | Yes | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v2_075_224/classification/4) | 
| MobileNet V2 1\.00 224 | Yes | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v2_100_224/classification/4) | 
| MobileNet V2 1\.30 224 | Yes | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v2_130_224/classification/4) | 
| MobileNet V2 1\.40 224 | Yes | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v2_140_224/classification/4) | 
| ResNet 101 | Yes | [PyTorch Hub](https://pytorch.org/hub/pytorch_vision_resnet/) | 
| ResNet 152 | Yes | [PyTorch Hub](https://pytorch.org/hub/pytorch_vision_resnet/) | 
| ResNet 18 | Yes | [PyTorch Hub](https://pytorch.org/hub/pytorch_vision_resnet/) | 
| ResNet 34 | Yes | [PyTorch Hub](https://pytorch.org/hub/pytorch_vision_resnet/) | 
| ResNet 50 | Yes | [Tensorflow Hub](https://tfhub.dev/tensorflow/resnet_50/classification/1) | 
| ResNet 50 | Yes | [PyTorch Hub](https://pytorch.org/hub/pytorch_vision_resnet/) | 
| ResNet V1 101 | Yes | [Tensorflow Hub](https://tfhub.dev/google/imagenet/resnet_v1_101/classification/4) | 
| ResNet V1 152 | Yes | [Tensorflow Hub](https://tfhub.dev/google/imagenet/resnet_v1_152/classification/4) | 
| ResNet V1 50 | Yes | [Tensorflow Hub](https://tfhub.dev/google/imagenet/resnet_v1_50/classification/4) | 
| ResNet V2 101 | Yes | [Tensorflow Hub](https://tfhub.dev/google/imagenet/resnet_v2_101/classification/4) | 
| ResNet V2 152 | Yes | [Tensorflow Hub](https://tfhub.dev/google/imagenet/resnet_v2_152/classification/4) | 
| ResNet V2 50 | Yes | [Tensorflow Hub](https://tfhub.dev/google/imagenet/resnet_v2_50/classification/4) | 
| Resnext 101 | Yes | [PyTorch Hub](https://pytorch.org/hub/pytorch_vision_resnext/) | 
| Resnext 50 | Yes | [PyTorch Hub](https://pytorch.org/hub/pytorch_vision_resnext/) | 
| ShuffleNet V2 | Yes | [PyTorch Hub](https://pytorch.org/hub/pytorch_vision_shufflenet_v2/) | 
| SqueezeNet 0 | Yes | [PyTorch Hub](https://pytorch.org/hub/pytorch_vision_squeezenet/) | 
| SqueezeNet 1 | Yes | [PyTorch Hub](https://pytorch.org/hub/pytorch_vision_squeezenet/) | 
| VGG 11 | Yes | [PyTorch Hub](https://pytorch.org/hub/pytorch_vision_vgg/) | 
| VGG 11\-BN | Yes | [PyTorch Hub](https://pytorch.org/hub/pytorch_vision_vgg/) | 
| VGG\-13 | Yes | [PyTorch Hub](https://pytorch.org/hub/pytorch_vision_vgg/) | 
| VGG 13\-BN | Yes | [PyTorch Hub](https://pytorch.org/hub/pytorch_vision_vgg/) | 
| VGG 16 | Yes | [PyTorch Hub](https://pytorch.org/hub/pytorch_vision_vgg/) | 
| VGG 16\-BN | Yes | [PyTorch Hub](https://pytorch.org/hub/pytorch_vision_vgg/) | 
| VGG 19 | Yes | [PyTorch Hub](https://pytorch.org/hub/pytorch_vision_vgg/) | 
| VGG 19\-BN | Yes | [PyTorch Hub](https://pytorch.org/hub/pytorch_vision_vgg/) | 
| Wide ResNet 101 | Yes | [PyTorch Hub](https://pytorch.org/hub/pytorch_vision_wide_resnet/) | 
| Wide ResNet 50 | Yes | [PyTorch Hub](https://pytorch.org/hub/pytorch_vision_wide_resnet/) | 

------
#### [ Image Embedding ]




| Model | Fine\-tunable | Source | 
| --- | --- | --- | 
| BiT\-M R101x1 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/bit/m-r101x1/1) | 
| BiT\-M R101x3 ImageNet\-21k Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/bit/m-r101x3/1) | 
| BiT\-M R50x1 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/bit/m-r50x1/1) | 
| BiT\-M R50x3 ImageNet\-21k | No | [Tensorflow Hub](https://tfhub.dev/google/bit/m-r50x3/1) | 
| BiT\-S R101x1 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/bit/s-r101x1/1) | 
| BiT\-S R101x3 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/bit/s-r101x3/1) | 
| BiT\-S R50x1 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/bit/s-r50x1/1) | 
| BiT\-S R50x3 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/bit/s-r50x3/1) | 
| EfficientNet B0 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/efficientnet/b0/feature-vector/1) | 
| EfficientNet B0 Lite Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/efficientnet/lite0/feature-vector/2) | 
| EfficientNet B1 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/efficientnet/b1/feature-vector/1) | 
| EfficientNet B1 Lite Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/efficientnet/lite1/feature-vector/2) | 
| EfficientNet B2 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/efficientnet/b2/feature-vector/1) | 
| EfficientNet B2 Lite Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/efficientnet/lite2/feature-vector/2) | 
| EfficientNet B3 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/efficientnet/b3/feature-vector/1) | 
| EfficientNet B3 Lite Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/efficientnet/lite3/feature-vector/2) | 
| EfficientNet B4 Lite Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/efficientnet/lite4/feature-vector/2) | 
| EfficientNet B6 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/efficientnet/b6/feature-vector/1) | 
| Inception V1 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/imagenet/inception_v1/feature_vector/4) | 
| Inception V2 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/imagenet/inception_v2/feature_vector/4) | 
| Inception V3 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/imagenet/inception_v3/feature_vector/4) | 
| Inception V3 Preview Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/tf2-preview/inception_v3/feature_vector/4) | 
| MobileNet V1 0\.25 128 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v1_025_128/feature_vector/4) | 
| MobileNet V1 0\.25 160 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v1_025_160/feature_vector/4) | 
| MobileNet V1 0\.25 192 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v1_025_192/feature_vector/4) | 
| MobileNet V1 0\.25 224 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v1_025_224/feature_vector/4) | 
| MobileNet V1 0\.50 128 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v1_050_128/feature_vector/4) | 
| MobileNet V1 0\.50 160 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v1_050_160/feature_vector/4) | 
| MobileNet V1 0\.50 192 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v1_050_192/feature_vector/4) | 
| MobileNet V1 0\.50 224 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v1_050_224/feature_vector/4) | 
| MobileNet V1 0\.75 128 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v1_075_128/feature_vector/4) | 
| MobileNet V1 0\.75 160 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v1_075_160/feature_vector/4) | 
| MobileNet V1 0\.75 192 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v1_075_192/feature_vector/4) | 
| MobileNet V1 0\.75 224 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v1_075_224/feature_vector/4) | 
| MobileNet V1 1\.00 128 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v1_100_128/feature_vector/4) | 
| MobileNet V1 1\.00 160 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v1_100_160/feature_vector/4) | 
| MobileNet V1 1\.00 192 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v1_100_192/feature_vector/4) | 
| MobileNet V1 1\.00 224 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v1_100_224/feature_vector/4) | 
| MobileNet V2 0\.35 224 Feature Vector  | No | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v2_035_224/feature_vector/4) | 
| MobileNet V2 0\.50 224 | No | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v2_050_224/feature_vector/4) | 
| MobileNet V2 0\.75 224 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v2_075_224/feature_vector/4) | 
| MobileNet V2 1\.00 224 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v2_100_224/feature_vector/4) | 
| MobileNet V2 1\.30 224 Feature Vector  | No | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v2_130_224/feature_vector/4) | 
| MobileNet V2 1\.40 224 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/imagenet/mobilenet_v2_140_224/feature_vector/4) | 
| MobileNet V2 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/tf2-preview/mobilenet_v2/feature_vector/4) | 
| ResNet 50 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/resnet_50/feature_vector/1) | 
| ResNet V1 101 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/imagenet/resnet_v1_101/feature_vector/4) | 
| ResNet V1 152 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/imagenet/resnet_v1_152/feature_vector/4) | 
| ResNet V1 50 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/imagenet/resnet_v1_50/feature_vector/4) | 
| ResNet V2 101 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/imagenet/resnet_v2_101/feature_vector/4) | 
| ResNet V2 152 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/imagenet/resnet_v2_152/feature_vector/4) | 
| ResNet V2 50 Feature Vector | No | [Tensorflow Hub](https://tfhub.dev/google/imagenet/resnet_v2_50/feature_vector/4) | 

------
#### [ Instance Segmentation ]




| Model | Fine\-tunable | Source | 
| --- | --- | --- | 
| MASK RCNN FPN RESNET101 COCO | No | [GluonCV](https://cv.gluon.ai/model_zoo/segmentation.html) | 
| MASK RCNN FPN RESNET50 COCO | No | [GluonCV](https://cv.gluon.ai/model_zoo/segmentation.html) | 
| MASK RCNN FPN RESNET18 COCO | No | [GluonCV](https://cv.gluon.ai/model_zoo/segmentation.html) | 
| MASK RCNN RESNET18 COCO | No | [GluonCV](https://cv.gluon.ai/model_zoo/segmentation.html) | 

------
#### [ Object Detection ]




| Model | Fine\-tunable | Source | 
| --- | --- | --- | 
| CenterNet 1024x1024 | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/centernet/hourglass_1024x1024/1) | 
| CenterNet 1024x1024 Keypoints | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/centernet/hourglass_1024x1024_kpts/1) | 
| CenterNet 512x512 | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/centernet/hourglass_512x512/1) | 
| CenterNet 512x512 Keypoints | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/centernet/hourglass_512x512_kpts/1) | 
| CenterNet ResNet\-v1\-101 | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/centernet/resnet101v1_fpn_512x512/1) | 
| CenterNet ResNet\-v1\-50 | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/centernet/resnet50v1_fpn_512x512/1) | 
| CenterNet ResNet\-v1\-50 Keypoints | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/centernet/resnet50v1_fpn_512x512_kpts/1) | 
| CenterNet ResNet\-v2\-50 | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/centernet/resnet50v2_512x512/1) | 
| CenterNet ResNet\-v2\-50 Keypoints | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/centernet/resnet50v2_512x512_kpts/1) | 
| Faster R\-CNN Resnet V2 1024x1024 | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/faster_rcnn/inception_resnet_v2_1024x1024/1) | 
| Faster R\-CNN Resnet V2 640x640 | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/faster_rcnn/inception_resnet_v2_640x640/1) | 
| Faster R\-CNN Resnet\-101 V1 1024x1024 | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/faster_rcnn/resnet101_v1_1024x1024/1) | 
| Faster R\-CNN Resnet\-101 V1 640x640 | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/faster_rcnn/resnet101_v1_640x640/1) | 
| Faster R\-CNN Resnet\-101 V1 800x1333 | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/faster_rcnn/resnet101_v1_800x1333/1) | 
| Faster R\-CNN Resnet\-152 V1 1024x1024 | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/faster_rcnn/resnet152_v1_1024x1024/1) | 
| Faster R\-CNN Resnet\-152 V1 800x1333 | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/faster_rcnn/resnet152_v1_800x1333/1) | 
| Faster R\-CNN Resnet\-152 V1 640x640 | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/faster_rcnn/resnet152_v1_640x640/1) | 
| Faster R\-CNN Resnet\-50 V1 1024x1024 | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/faster_rcnn/resnet50_v1_1024x1024/1) | 
| Faster R\-CNN Resnet\-50 V1 640x640 | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/faster_rcnn/resnet50_v1_640x640/1) | 
| Faster R\-CNN Resnet\-50 V1 800x1333 | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/faster_rcnn/resnet50_v1_800x1333/1) | 
| Faster RCNN ResNet 101 V1d | No | [GluonCV](https://cv.gluon.ai/model_zoo/detection.html) | 
| Faster RCNN ResNet 50 V1b | No | [GluonCV](https://cv.gluon.ai/model_zoo/detection.html) | 
| FRCNN MobileNet V3 large 320 FPN | No | [PyTorch Hub](https://pytorch.org/vision/stable/_modules/torchvision/models/detection/faster_rcnn.html) | 
| FRCNN MobileNet V3 large FPN | No | [PyTorch Hub](https://pytorch.org/vision/stable/_modules/torchvision/models/detection/faster_rcnn.html) | 
| FRCNN ResNet 50 FPN | No | [PyTorch Hub](https://pytorch.org/vision/stable/_modules/torchvision/models/detection/faster_rcnn.html) | 
| Retinanet SSD Resnet\-101 1024x1024 | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/retinanet/resnet101_v1_fpn_1024x1024/1) | 
| Retinanet SSD Resnet\-101 640x640 | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/retinanet/resnet101_v1_fpn_640x640/1) | 
| Retinanet SSD Resnet\-152 1024x1024 | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/retinanet/resnet152_v1_fpn_1024x1024/1) | 
| Retinanet SSD Resnet\-152 640x640 | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/retinanet/resnet152_v1_fpn_640x640/1) | 
| Retinanet SSD Resnet\-50 1024x1024 | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/retinanet/resnet50_v1_fpn_1024x1024/1) | 
| Retinanet SSD Resnet\-50 640x640 | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/retinanet/resnet50_v1_fpn_640x640/1) | 
| SSD | No | [PyTorch Hub](https://pytorch.org/hub/nvidia_deeplearningexamples_ssd/) | 
| SSD 512 ResNet 50 V1 | Yes | [GluonCV](https://cv.gluon.ai/model_zoo/detection.html) | 
| SSD EfficientDet D0 | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/efficientdet/d0/1) | 
| SSD EfficientDet D1 | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/efficientdet/d1/1) | 
| SSD EfficientDet D2 | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/efficientdet/d2/1) | 
| SSD EfficientDet D3 | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/efficientdet/d3/1) | 
| SSD EfficientDet D4 | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/efficientdet/d4/1) | 
| SSD EfficientDet D5 | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/efficientdet/d5/1) | 
| SSD MobileNet 1\.0 | Yes | [GluonCV](https://cv.gluon.ai/model_zoo/detection.html) | 
| SSD Mobilenet V1 640x640 | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/ssd_mobilenet_v1/fpn_640x640/1) | 
| SSD Mobilenet V2 | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/ssd_mobilenet_v2/2) | 
| SSD Mobilenet V2 320x320 | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/ssd_mobilenet_v2/fpnlite_320x320/1) | 
| SSD Mobilenet V2 640x640 | No | [Tensorflow Hub](https://tfhub.dev/tensorflow/ssd_mobilenet_v2/fpnlite_640x640/1) | 
| SSD ResNet 50 V1 | Yes | [GluonCV](https://cv.gluon.ai/model_zoo/detection.html) | 
| SSD VGG 16 Atrous 300 | Yes | [GluonCV](https://cv.gluon.ai/model_zoo/detection.html) | 
| SSD VGG 16 Atrous 512 | Yes | [GluonCV](https://cv.gluon.ai/model_zoo/detection.html) | 
| YOLO V3 DarkNet 53 | No | [GluonCV](https://cv.gluon.ai/model_zoo/detection.html) | 
| YOLO V3 MobileNet 1\.0 | No | [GluonCV](https://cv.gluon.ai/model_zoo/detection.html) | 

------
#### [ Semantic Segmentation ]




| Model | Fine\-tunable | Source | 
| --- | --- | --- | 
| FCN ResNet 101 ADE20K | Yes | [GluonCV](https://cv.gluon.ai/model_zoo/segmentation.html) | 
| FCN ResNet 101 COCO | Yes | [GluonCV](https://cv.gluon.ai/model_zoo/segmentation.html) | 
| FCN ResNet 101 Pascal VOC | Yes | [GluonCV](https://cv.gluon.ai/model_zoo/segmentation.html) | 
| FCN ResNet 50 ADE20K | Yes | [GluonCV](https://cv.gluon.ai/model_zoo/segmentation.html) | 

------

## Deploy a Model<a name="jumpstart-deploy"></a>

 When you deploy a model from JumpStart, SageMaker hosts the model and deploys an endpoint that you can use for inference\. JumpStart also provides an example notebook that you can use to access the model after it's deployed\. 

## Model Deployment Configuration<a name="jumpstart-config"></a>

 After you choose a model, the **Deploy Model** pane opens\. Choose **Deployment Configuration** to configure your model deployment\. 

 ![\[Deploy Model pane option to open settings for Deployment Configuration and Security Settings.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-deploy.png) 

 The default instance type for deploying a model depends on the model\. The instance type is the hardware that the training job runs on\. In the following example, the `ml.g4dn.xlarge` instance is the default for this particular BERT model\. 

You can also change the **Endpoint name**\. 

 ![\[JumpStart Deploy Model pane with Deployment Configuration opened to select its settings.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-deploy-config.png) 

Choose **Security Settings** to specify the IAM role, Amazon VPC, and encryption keys for the model\.

 ![\[JumpStart Deploy Model pane with Security Settings opened to select its settings.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-deploy-security.png) 

### Model Deployment Security<a name="jumpstart-config-security"></a>

When you deploy a model with JumpStart, you can specify an AWS Identity and Access Management \(IAM \) role, Amazon Virtual Private Cloud \(Amazon VPC\), and encryption keys for the model\. If you do not specify any values for these entries, your Studio runtime role is the default IAM role that is used, default encryption is used, and no Amazon VPC is used\.

#### IAM role<a name="jumpstart-config-security-iam"></a>

You can select an IAM role that is passed as part of training jobs and hosting jobs\. SageMaker uses this role to access training data and model artifacts\. If you do not select an IAM role, SageMaker deploys the model using your Studio runtime role\. For more information about IAM roles, see [Identity and Access Management for Amazon SageMaker](security-iam.md)\.

The role that you pass must have access to the resources that the model needs, including at the least the following\.
+ For training jobs: [CreateTrainingJob API: Execution Role Permissions](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html#sagemaker-roles-createtrainingjob-perms)\.
+ For hosting jobs: [CreateModel API: Execution Role Permissions](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html#sagemaker-roles-createmodel-perms)\.

**Note**  
You can scope down the Amazon S3 permissions granted in each of the following roles\. Do this by using the Amazon Resource Name \(ARN\) of your Amazon Simple Storage Service \(Amazon S3\) bucket and the JumpStart Amazon S3 bucket\.  

```
{
  "Effect": "Allow",
  "Action": [
    "s3:GetObject",
    "s3:PutObject",
    "s3:ListMultipartUploadParts"
  ],
  "Resources": [
    "arn:aws:s3:<region>::bucket/jumpstart-cache-prod-<region>/*",
    "arn:aws:s3:<region>:<account>:bucket/*",
  ]
},{
  "Effect": "Allow",
  "Action": [
    "s3:ListBucket",
  ],
  "Resources": [
    "arn:aws:s3:<region>::bucket/jumpstart-cache-prod-<region>",
    "arn:aws:s3:<region>:<account>:bucket",
  ]
```

**Find IAM role**

If you select this option, you must select an existing IAM role from the dropdown list\.

 ![\[JumpStart Security Settings IAM section with Find IAM role selected.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-deploy-security-findiam.png) 

**Input IAM role**

If you select this option, you must manually enter the ARN for an existing IAM role\. If your Studio runtime role or Studio Amazon VPC block the `iam:list* `call, you must use this option to use an existing IAM role\.

 ![\[JumpStart Security Settings IAM section with Input IAM role selected.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-deploy-security-inputiam.png) 

#### Amazon VPC<a name="jumpstart-config-security-vpc"></a>

All JumpStart models run in network isolation mode\. After the model container is created, no more calls can be made\. You can select an Amazon VPC that is passed as part of training jobs and hosting jobs\. SageMaker uses this Amazon VPC to push and pull resources from your Amazon S3 bucket\. This Amazon VPC is different from the Studio Amazon VPC, that limits access to the public internet from your Studio instance\. For more information about the Studio Amazon VPC, see [Connect SageMaker Studio Notebooks in a VPC to External Resources](studio-notebooks-and-internet-access.md)\.

The Amazon VPC that you pass does not need access to the public internet, but it does need access to Amazon S3\. The Amazon VPC endpoint for Amazon S3 must allow access to at least the following resources that the model needs\.

```
{
  "Effect": "Allow",
  "Action": [
    "s3:GetObject",
    "s3:PutObject",
    "s3:ListMultipartUploadParts"
  ],
  "Resources": [
    "arn:aws:s3:<region>::bucket/jumpstart-cache-prod-<region>/*",
    "arn:aws:s3:<region>:<account>:bucket/*",
  ]
},{
  "Effect": "Allow",
  "Action": [
    "s3:ListBucket",
  ],
  "Resources": [
    "arn:aws:s3:<region>::bucket/jumpstart-cache-prod-<region>",
    "arn:aws:s3:<region>:<account>:bucket",
  ]
```

If you do not select an Amazon VPC, no Amazon VPC is used\.

**Find VPC**

If you select this option, you must select an existing Amazon VPC from the dropdown list\. After you select an Amazon VPC, you must select a subnet and security group for your Amazon VPC\. For more information about subnets and security groups, see [Overview of VPCs and subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html)\.

 ![\[JumpStart Security Settings VPC section with Find VPC selected.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-deploy-security-findvpc.png) 

**Input VPC**

If you select this option, you must manually select the subnet and security group that compose your Amazon VPC\. If your Studio runtime role or Studio Amazon VPC block the `ec2:list*` call, you must use this option to select the subnet and security group\.

 ![\[JumpStart Security Settings VPC section with Input VPC selected.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-deploy-security-inputvpc.png) 

#### Encryption keys<a name="jumpstart-config-security-encryption"></a>

You can select an AWS KMS key that is passed as part of training jobs and hosting jobs\. SageMaker uses this key to encrypt the Amazon EBS volume for the container, as well as the repackaged model in Amazon S3 for hosting jobs and the output for training jobs\. For more information about AWS KMS keys, see [AWS KMS keys](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#kms_keys)\.

The key that you pass must trust the IAM role that you pass\. If you do not specify an IAM role, the AWS KMS key must trust your Studio runtime role\.

If you do not select an AWS KMS key, SageMaker provides default encryption for the data in the Amazon EBS volume and the Amazon S3 artifacts\.

**Find encryption keys**

If you select this option, you must select existing AWS KMS keys from the dropdown list\.

 ![\[JumpStart Security Settings encryption section with Find encryption keys selected.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-deploy-security-findencryption.png) 

**Input encryption keys**

If you select this option, you must manually enter the AWS KMS keys\. If your Studio execution role or Studio Amazon VPC block the `kms:list* `call, you must use this option to select existing AWS KMS keys\.

 ![\[JumpStart Security Settings encryption section with Input encryption keys selected.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-deploy-security-inputencryption.png) 

## Fine\-Tune a Model<a name="jumpstart-fine-tune"></a>

 Fine\-tuning trains a pretrained model on a new dataset without training from scratch\. This process, also known as transfer learning, can produce accurate models with smaller datasets and less training time\. 

## Fine\-Tuning Data Source<a name="jumpstart-fine-tune-data"></a>

 When you fine\-tune a model, you can use the default dataset or choose your own data, which is located in an Amazon S3 bucket\. 

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-fine-tune.png) 

 To browse the buckets available to you, choose **Find S3 bucket**\. These buckets are limited by the permissions used to set up your Studio account\. You can also specify an Amazon S3 URI by choosing **Enter Amazon S3 bucket location**\. 

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-dataset.png) 

**Tip**  
 To find out how to format the data in your bucket, choose **Learn more**\. Also, the description section for the model has detailed information about inputs and outputs\.  

 For text models: 
+  The bucket must have a data\.csv file\. 
+  The first column must be a unique integer for the class label\. For example: 1, 2, 3, 4, n\. 
+  The second column must be a string\. 
+  The second column should have the corresponding text that matches the type and language for the model\.  

 For vision models: 
+  The bucket must have as many subdirectories as the number of classes\. 
+  Each subdirectory should contain images that belong to that class in \.jpg format\. 

**Note**  
 The S3 bucket must be in the same AWS Region where you're running SageMaker Studio because SageMaker doesn't allow cross\-Region requests\. 

## Fine\-Tuning Deployment Configuration<a name="jumpstart-fine-tune-deploy"></a>

 The p3 family is recommended as the fastest for deep learning training, and this is recommended for fine\-tuning a model\. The following chart shows the number of GPUs in each instance type\. There are other available options that you can choose from, including p2 and g4 instance types\. 


|  |  | 
| --- |--- |
|  Instance type  |  GPUs  | 
|  p3\.2xlarge  |  1  | 
|  p3\.8xlarge  |  4  | 
|  p3\.16xlarge  |  8  | 
|  p3dn\.24xlarge  |  8  | 

## Hyperparameters<a name="jumpstart-hyperparameters"></a>

 You can customize the hyperparameters of the training job that are used to fine\-tune the model\.  

If you use the default dataset for text models without changing the hyperparameters, you get a nearly identical model as a result\. For vision models, the default dataset is different from the dataset used to train the pretrained models, so your model is different as a result\. 

 You have the following hyperparameter options: 
+ **Epochs** – One epoch is one cycle through the entire dataset\. Multiple intervals complete a batch, and multiple batches eventually complete an epoch\. Multiple epochs are run until the accuracy of the model reaches an acceptable level, or when the error rate drops below an acceptable level\. 
+ **Learning rate** – The amount that values should be changed between epochs\. As the model is refined, its internal weights are being nudged and error rates are checked to see if the model improves\. A typical learning rate is 0\.1 or 0\.01, where 0\.01 is a much smaller adjustment and could cause the training to take a long time to converge, whereas 0\.1 is much larger and can cause the training to overshoot\. It is one of the primary hyperparameters that you might adjust for training your model\. Note that for text models, a much smaller learning rate \(5e\-5 for BERT\) can result in a more accurate model\. 
+ **Batch size** – The number of records from the dataset that to be selected for each interval to send to the GPUs available in training\. In an image example, you might send out 32 images per GPU, so 32 would be your batch size\. If you choose an instance type with more than one GPU, the batch is divided by the number of GPUs\. Suggested batch size varies depending on the data and the model that you are using\. For example, how you optimize for image data differs from how you handle language data\. In the instance type chart in the deployment configuration section, you can see the number of GPUs per instance type\. Start with a standard recommended batch size \(for example, 32 for a vision model\)\. Then, multiply this by the number of GPUs in the instance type that you selected\. For example, if you're using a `p3.8xlarge`, this would be 32\(batch size\) multiplied by 4\(GPUs\), for a total of 128, as your batch size adjusts for the number of GPUs\. For a text model like BERT, try starting with a batch size of 64, and then reduce as needed\. 

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-hyperparameters.png) 

## Training Output<a name="jumpstart-training"></a>

When the fine\-tuning process is complete, JumpStart provides information about the model: parent model, training job name, training job Amazon Resource Name \(ARN\), training time, and output path\. The output path is where you can find your new model in an Amazon S3 bucket\. The folder structure uses the model name that you provided and the model file is in an `/output` subfolder and it's always named `model.tar.gz`\.  

 Example: `s3://bucket/model-name/output/model.tar.gz` 