# SageMaker JumpStart<a name="studio-jumpstart"></a>

**Important**  
 To use new features with an existing notebook instance or Studio app, you must restart the notebook instance or the Studio app to get the latest updates\. 

 You can use SageMaker JumpStart to learn about SageMaker features and capabilities through curated 1\-click solutions, example notebooks, and pretrained models that you can deploy\. You can also fine\-tune the models and deploy them\. 

 To access JumpStart, you must first launch SageMaker Studio\. JumpStart features are not available in SageMaker notebook instances, and you can't access them through SageMaker APIs or the AWS CLI\. 

 Open JumpStart by using the JumpStart launcher in the **Get Started** section or by choosing the JumpStart icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-icon.png)\) in the *left sidebar*\. 

In the *file and resource browser* \(the left pane\), you can find JumpStart options\. From here you can choose to browse JumpStart for solutions, models, notebooks, and other resources, or you can view your currently launched solutions, endpoints, and training jobs\. 

 To see what JumpStart has to offer, choose the JumpStart icon, and then choose **Browse JumpStart**\. JumpStart opens in a new tab in the *main work area*\. Here you can browse 1\-click solutions, models, example notebooks, blogs, and video tutorials\. 

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-assets.png) 

**Important**  
Amazon SageMaker JumpStart makes certain content available from third\-party sources\. This content may be subject to separate license terms\. You are responsible for reviewing and complying with any applicable license terms and making sure they are acceptable for your use case before downloading or using the content\. 

## Using JumpStart<a name="jumpstart-using"></a>

 At the top of the JumpStart page you can use search to look for topics of interest\.  

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-search.png) 

 You can find JumpStart resources by using search, or by browsing each category that follows the search panel: 
+  **Solutions** – Launch end\-to\-end machine learning solutions that tie SageMaker to other AWS services with one click\. 
+  **Text models** – Deploy and fine\-tune pretrained transformers for various natural language processing use cases\. 
+  **Vision models** – Deploy and fine\-tune pretrained models for image classification and object detection with one click\.
+  **SageMaker algorithms** – Train and deploy SageMaker built\-in Algorithms for various problem types with these example notebooks\. 
+  **Example notebooks** – Run example notebooks that use SageMaker features like spot instance training and experiments over a large variety of model types and use cases\. 
+  **Blogs** – Read deep dives and solutions from machine learning experts hosted by Amazon\. 
+  **Video tutorials** – Watch video tutorials for SageMaker features and machine learning use cases from machine learning experts hosted by Amazon\. 

## Solutions<a name="jumpstart-solutions"></a>

 When you choose a solution, JumpStart shows a description of the solution and a **Launch** button\. When you click **Launch**, JumpStart creates all of the resources necessary to run the solution, including training and model hosting instances\. After JumpStart launches the solution, JumpStart shows an **Open Notebook** button\. You can click the button to use the provided notebooks and explore the solution’s features\. As artifacts are generated during launch or after running the provided notebooks, they are listed in the **Generated Artifacts** table\. You can delete individual Artifacts with the Trash icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-trash.png)\)\. You can delete all of the solution’s resources by choosing **Delete solution resources**\.

 

## Models<a name="jumpstart-models"></a>

 Models are available for quick deployment directly from JumpStart\. You can also fine\-tune some of these models\. When you browse the models, you can scroll to the deploy and fine\-tune sections to the **Description** section\. In the **Description** section, you can learn more about the model, including what it can do with the model, what kind of inputs and outputs are expected, and the kind of data you need if you want to use transfer learning to fine\-tune the model\. 

The following tables list the models currently offered in JumpStart\. The available models are sorted by their model type and task\. To view other model sets, click the task tab for those models\.

### Text Models<a name="jumpstart-models-text"></a>

------
#### [ Text Classification Models ]




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
#### [ Text Generation Models ]




| Model | Fine\-tunable | Source | 
| --- | --- | --- | 
| DistilGPT 2 | No | [Hugging Face](https://huggingface.co/distilgpt2) | 
| GPT 2 | No | [Hugging Face](https://huggingface.co/gpt2) | 
| GPT 2 Large | No | [Hugging Face](https://huggingface.co/gpt2-large) | 
| GPT 2 Medium | No | [Hugging Face](https://huggingface.co/gpt2-medium) | 
| OpenAI GPT | No | [Hugging Face](https://huggingface.co/openai-gpt) | 

------
#### [ Extractive Question Answering Models ]




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
#### [ Sentence Pair Classification Models ]




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

### Vision Models<a name="jumpstart-models-vision"></a>

------
#### [ Image Classification Models ]




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
#### [ Image Embedding Models ]




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
#### [ Object Detection Models ]




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

## Deploy a model<a name="jumpstart-deploy"></a>

 When you deploy a model from JumpStart, SageMaker hosts the model and deploys an endpoint that you can use for inference\. JumpStart also provides an example notebook that you can use to access the model after it's deployed\. 

## Model Deployment Configuration<a name="jumpstart-config"></a>

 After you choose a model, the **Deploy Model** pane opens\. Choose **Deployment Configuration** to configure your model deployment\. 

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-deploy.png) 

 The default **Machine Type** for deploying a model depends on the model\. The machine type is the hardware that the training job runs on\. In the following example, the `ml.m5.large` instance is the default for this particular BERT model\. 

You can also change the **Endpoint Name**\. 

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-deploy-config.png) 

## Fine\-Tune a Model<a name="jumpstart-fine-tune"></a>

 Fine\-tuning trains a pretrained model on a new dataset without training from scratch\. This process, also known as transfer learning, can produce accurate models with smaller datasets and less training time\. 

## Fine\-Tuning Data Source<a name="jumpstart-fine-tune-data"></a>

 When you fine\-tune a model, you can use the default dataset or choose your own data, which is located in an S3 bucket\. 

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-fine-tune.png) 

 To browse the buckets available to you, choose **Find S3 bucket**\. These buckets are limited by the permissions used to set up your Studio account\. You can also specify an S3 URI by choosing **Enter S3 bucket location**\. 

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
 The S3 bucket must be in the same AWS Region where you're running SageMaker Studio because SageMaker doesn't allow cross\-region requests\. 

## Fine\-Tuning deployment configuration<a name="jumpstart-fine-tune-deploy"></a>

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
+ **Epochs** – One epoch is one cycle through the entire dataset\. Multiple intervals complete a batch, and multiple batches eventually complete an epoch\. Multiple epochs are run until the accuracy of the model reaches an acceptable level, or in other words, when the error rate drops below an acceptable level\. 
+ **Learning rate** – The amount that values should be changed between epochs\. As the model is refined, its internal weights are being nudged and error rates are checked to see if the model improves\. A typical learning rate is 0\.1 or 0\.01, where 0\.01 is a much smaller adjustment and could cause the training to take a long time to converge, whereas 0\.1 is much larger and can cause the training to overshoot\. It is one of the primary hyperparameters that you might adjust for training your model\. Note that for text models, a much smaller learning rate \(5e\-5 for BERT\) can result in a more accurate model\. 
+ **Batch size** – The number of records from the dataset that to be selected for each interval to send to the GPUs available in training\. In an image example, you might send out 32 images per GPU, so 32 would be your batch size\. If you choose an instance type with more than one GPU, the batch is divided by the number of GPUs\. Suggested batch size varies depending on the data and the model that you are using\. For example, how you optimize for image data differs from how you handle language data\. In the instance type chart in the deployment configuration section, you can see the number of GPUs per instance type\. Start with a standard recommended batch size \(for example, 32 for a vision model\)\. Then, multiply this by the number of GPUs in the instance type that you selected\. For example, if you're using a `p3.8xlarge`, this would be 32\(batch size\)\*4\(GPUs\), for a total of 128 as your batch size adjusted for the number of GPUs\. For a text model like BERT, try starting with a batch size of 64, and then reduce as needed\. 

 ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-hyperparameters.png) 

## Training Output<a name="jumpstart-training"></a>

When the fine\-tuning process is complete, JumpStart provides information about the model: parent model, training job name, training job Amazon Resource Name \(ARN\), training time, and output path\. The output path is where you can find your new model in an S3 bucket\. The folder structure uses the model name you provided and the model file is in an `/output` subfolder and it's always named `model.tar.gz`\.  

 Example: `s3://bucket/model-name/output/model.tar.gz` 