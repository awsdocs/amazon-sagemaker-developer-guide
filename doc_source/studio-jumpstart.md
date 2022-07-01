# SageMaker JumpStart<a name="studio-jumpstart"></a>

SageMaker JumpStart provides pre\-trained, open\-source models for a wide range of problem types to help you get started with machine learning\. You can incrementally train and tune these models before deployment\. JumpStart also provides solution\-templates that set up infrastructure for common use cases, and executable example notebooks for machine learning with SageMaker\.

You can access the pre\-trained models, solution templates, and examples through the JumpStart landing page in Amazon SageMaker Studio\. The following steps show how to access JumpStart models and solutions using Amazon SageMaker Studio\.

You can also access the models using the SageMaker Python SDK\. For information about how to use JumpStart models programmatically via API, see [Use SageMaker JumpStart Algorithms with Pretrained Models](https://sagemaker.readthedocs.io/en/stable/overview.html#use-sagemaker-jumpstart-algorithms-with-pretrained-models)\.

**Topics**
+ [Open JumpStart](#jumpstart-open)
+ [Using JumpStart](#jumpstart-using)
+ [Solution Templates](#jumpstart-solutions)
+ [Models](#jumpstart-models)
+ [Deploy a Model](#jumpstart-deploy)
+ [Model Deployment Configuration](#jumpstart-config)
+ [Fine\-Tune a Model](#jumpstart-fine-tune)
+ [Fine\-Tuning Data Source](#jumpstart-fine-tune-data)
+ [Fine\-Tuning Deployment Configuration](#jumpstart-fine-tune-deploy)
+ [Hyperparameters](#jumpstart-hyperparameters)
+ [Training Output](#jumpstart-training)
+ [Amazon SageMaker JumpStart Industry](studio-jumpstart-industry.md)

## Open JumpStart<a name="jumpstart-open"></a>

In Amazon SageMaker Studio, open JumpStart by using one of the following:
+ The JumpStart launcher in the **Get Started** section\.
+ The JumpStart icon \(![\[The JumpStart icon.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-icon.png)\) in the *left sidebar*\.
+ The **Browse JumpStart** button in the launched assets pane\.

![\[SageMaker Studio interface with JumpStart Launcher, JumpStart icon, and Browse JumpStart button.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-assets.png)

**Important**  
Before downloading or using third\-party content: You are responsible for reviewing and complying with any applicable license terms and making sure that they are acceptable for your use case\. 

## Using JumpStart<a name="jumpstart-using"></a>

From the SageMaker JumpStart landing page, you can browse for solutions, models, notebooks, and other resources\. You can also view your currently launched solutions, endpoints, and training jobs\. Using the JumpStart search bar, you can search for topics of interest\. 

 ![\[The SageMaker JumpStart landing page with search bar and autosuggest options.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-search.png) 

You can find JumpStart resources by using search, or by browsing each category that follows the search panel: 
+  **Featured** – The latest or most used solutions, models, and examples\. 
+  **Solutions** – In one step, launch comprehensive machine learning solutions that tie SageMaker to other AWS services\. Select **Explore All Solutions** to view all available solutions\.
+  **Models** – Find a model that fits your needs from the collection of text, vision, and tabular models\. You can filter the collection by problem types, data types, and frameworks\. Then, deploy and refine pre\-trained models for image classification and object detection in one step\. Select **Explore All Models** to view all available models\.
+  **Resources** – Use example notebooks, blogs, and video tutorials to learn and head start your problem types\.
  +  **Example notebooks** – Run example notebooks that use SageMaker features like Spot Instance training and experiments over a large variety of model types and use cases\. 
  +  **Blogs** – Read details and solutions from machine learning experts, hosted by Amazon\. 
  +  **Video tutorials** – Watch video tutorials for SageMaker features and machine learning use cases from machine learning experts, hosted by Amazon\. 

## Solution Templates<a name="jumpstart-solutions"></a>

From the JumpStart landing page, get solution templates that set up the complete infrastructure for common use cases\. 

When you choose a solution template, JumpStart shows a description of the solution and a **Launch** button\. When you choose **Launch**, JumpStart creates all of the resources that you need to run the solution\. This includes training and model hosting instances\. 

After JumpStart launches the solution, JumpStart shows an **Open Notebook** button\. Choose the button to use the provided notebooks and explore the solution’s features\. When artifacts are generated, during launch or after running the provided notebooks, they're listed in the **Generated Artifacts** table\. You can delete individual artifacts with the trash icon \(![\[The trash icon for JumpStart.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-trash.png)\)\. You can delete all of the solution’s resources by choosing **Delete solution resources**\. 

 The following is the full list of available solution templates: 


| Use case  | Solution name  | Description  | Get started  | 
| --- | --- | --- | --- | 
| Time series forecasting  | Demand forecasting  | Demand forecasting for multivariate time series data using three state\-of\-the\-art time series forecasting algorithms: [LSTNet](https://ts.gluon.ai/api/gluonts/gluonts.model.lstnet.html), [Prophet](https://facebook.github.io/prophet/), and [SageMaker DeepAR](https://docs.aws.amazon.com/sagemaker/latest/dg/deepar.html)\. |  [GitHub »](https://github.com/awslabs/sagemaker-deep-demand-forecast)  | 
| Credit rating prediction  | Corporate credit rating prediction  | Multimodal \(long text and tabular\) machine learning for quality credit predictions using AWS [AutoGluon Tabular](https://auto.gluon.ai/stable/tutorials/tabular_prediction/index.html)\. | [GitHub »](https://github.com/awslabs/sagemaker-corporate-credit-rating) | 
|  ​  | Graph\-based credit scoring  | Predict corporate credit ratings using tabular data and a corporate network by training a [Graph Neural Network GraphSAGE](https://cs.stanford.edu/people/jure/pubs/graphsage-nips17.pdf) and AWS [AutoGluon Tabular](https://auto.gluon.ai/stable/tutorials/tabular_prediction/index.html) model\. | Find in Amazon SageMaker Studio\.  | 
|  ​  | Explain credit decisions  | Predict credit default in credit applications and provide explanations using [LightGBM](https://lightgbm.readthedocs.io/en/latest/) and [SHAP \(SHapley Additive exPlanations\)](https://shap.readthedocs.io/en/latest/index.html)\. |  [GitHub »](https://github.com/awslabs/sagemaker-explaining-credit-decisions)  | 
| Extract and analyze data from documents  | Privacy for sentiment classification  | [Anonymize text ](https://www.amazon.science/blog/preserving-privacy-in-analyses-of-textual-data) to better preserve user privacy in sentiment classification\. |  [GitHub »](https://github.com/awslabs/sagemaker-privacy-for-nlp)  | 
|  ​  | Document summarization, entity, and relationship extraction  | Document summarization, entity, and relationship extraction using the [transformers](https://huggingface.co/docs/transformers/index) library in PyTorch\. |  [GitHub »](https://github.com/awslabs/sagemaker-document-understanding)  | 
|  ​  | Handwriting recognition  | Recognize handwritten text in images by training an [object detection model](https://mxnet.apache.org/versions/1.0.0/api/python/gluon/model_zoo.html#mxnet.gluon.model_zoo.vision.resnet34_v1) and [handwriting recognition model](https://arxiv.org/abs/1910.00663)\. Label your own data using [SageMaker Ground Truth](https://aws.amazon.com/sagemaker/data-labeling/)\. | [GitHub »](https://github.com/awslabs/sagemaker-handwritten-text-recognition) | 
|  ​  | Filling in missing values in tabular records  | Fill missing values in tabular records by training a [SageMaker AutoPilot](https://aws.amazon.com/sagemaker/autopilot/) model\. |  [GitHub »](https://github.com/awslabs/filling-in-missing-values-in-tabular-records)  | 
| Fraud detection  | Detect malicious users and transactions  | Automatically detect potentially fraudulent activity in transactions using [SageMaker XGBoost](https://docs.aws.amazon.com/sagemaker/latest/dg/xgboost.html) with the over\-sampling technique [Synthetic Minority Over\-sampling](https://arxiv.org/abs/1106.1813) \(SMOTE\)\. |  [GitHub »](https://github.com/awslabs/fraud-detection-using-machine-learning)  | 
|  ​  | Fraud detection in financial transactions using deep graph library  | Detect fraud in financial transactions by training a [graph convolutional network](https://arxiv.org/pdf/1703.06103.pdf) with the [deep graph library](https://www.dgl.ai/) and a [SageMaker XGBoost](https://docs.aws.amazon.com/sagemaker/latest/dg/xgboost.html) model\. |  [GitHub »](https://github.com/awslabs/sagemaker-graph-fraud-detection)  | 
| Predictive maintenance  | Predictive maintenance for vehicle fleets  | Predict vehicle fleet failures using vehicle sensor and maintenance information with a convolutional neural network model\. |  [GitHub »](https://github.com/awslabs/aws-fleet-predictive-maintenance/)  | 
|  ​  | Predictive maintenance for manufacturing  | Predict the remaining useful life for each sensor by training a [stacked Bidirectional LSTM neural network](https://arxiv.org/pdf/1801.02143.pdf) model using historical sensor readings\. |  [GitHub »](https://github.com/awslabs/predictive-maintenance-using-machine-learning)  | 
| Computer vision  | Product defect detection in images  | Identify defective regions in product images by training an [object detection model](https://arxiv.org/abs/1506.01497)\. |  [GitHub »](https://github.com/awslabs/sagemaker-defect-detection)  | 
| Churn prediction  | Churn prediction with text  | Predict churn using numerical, categorical, and textual features with [BERT encoder](https://huggingface.co/) and [RandomForestClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html)\. |  [GitHub »](https://github.com/awslabs/sagemaker-churn-prediction-text)  | 
| Personalized recommendations  | Entity resolution in identity graphs with deep graph library  | Perform cross\-device entity linking for online advertising by training a [graph convolutional network](https://arxiv.org/pdf/1703.06103.pdf) with [deep graph library](https://www.dgl.ai/)\. |  [GitHub »](https://github.com/awslabs/sagemaker-graph-entity-resolution)  | 
|  ​  | Purchase modeling  | Predict whether a customer will make a purchase by training a [SageMaker XGBoost](https://docs.aws.amazon.com/sagemaker/latest/dg/xgboost.html) model\. |  [GitHub »](https://github.com/awslabs/sagemaker-purchase-modelling)  | 
| Reinforcement learning  | Reinforcement learning for Battlesnake AI competitions  | Provide a reinforcement learning workflow for training and inference with the [BattleSnake](https://play.battlesnake.com/) AI competitions\. |  [GitHub »](https://github.com/awslabs/sagemaker-battlesnake-ai)  | 
|  ​  | Distributed reinforcement learning for Procgen challenge  | Distributed reinforcement learning starter kit for [NeurIPS 2020 Procgen](https://www.aicrowd.com/challenges/neurips-2020-procgen-competition) Reinforcement learning challenge\. | [GitHub »](https://github.com/aws-samples/sagemaker-rl-procgen-ray) | 

## Models<a name="jumpstart-models"></a>

JumpStart supports models across fifteen of the most popular problem types\. Of the supported problem types, Vision and NLP\-related types total thirteen\. There are eight problem types that support incremental training and fine\-tuning\. For more information about incremental training and hyper\-parameter tuning, see [SageMaker Automatic Model Tuning](https://docs.aws.amazon.com/sagemaker/latest/dg/automatic-model-tuning.html)\.​ JumpStart also supports four popular algorithms for tabular data modeling\.

You can search and browse models from the JumpStart landing page in Studio\. When you select a model, the model detail page provides information about the model, and you can train and deploy your model in a few steps\. The description section describes what you can do with the model, the expected types of inputs and outputs, and the data type needed for fine\-tuning your model\. 

You can also programmatically utilize models with the [SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/overview.html#use-prebuilt-models-with-sagemaker-jumpstart)\. 

The list of problem types and links to their example Jupyter notebooks are summarized in the following table\. For a complete list of JumpStart models, see [JumpStart Available Model Table](https://sagemaker.readthedocs.io/en/stable/doc_utils/jumpstart.html)\.


| Problem types  | Supports inference with pre\-trained models  | Trainable on a custom dataset  | Supported frameworks  | Example Notebooks  | 
| --- | --- | --- | --- | --- | 
| Image classification  | Yes  | Yes  |  PyTorch, TensorFlow  |  [Introduction to JumpStart \- Image Classification](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/jumpstart_image_classification/Amazon_JumpStart_Image_Classification.ipynb)  | 
| Object detection  | Yes  | Yes  | PyTorch, TensorFlow, MXNet |  [Introduction to JumpStart \- Object Detection](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/jumpstart_object_detection/Amazon_JumpStart_Object_Detection.ipynb)  | 
| Semantic segmentation  | Yes  | Yes  | MXNet  |  [Introduction to JumpStart \- Semantic Segmentation](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/jumpstart_semantic_segmentation/Amazon_JumpStart_Semantic_Segmentation.ipynb)  | 
| Instance segmentation  | Yes  | Yes  | MXNet  |  [Introduction to JumpStart \- Instance Segmentation](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/jumpstart_instance_segmentation/Amazon_JumpStart_Instance_Segmentation.ipynb)  | 
| Image embedding  | Yes  | No  | TensorFlow, MXNet |  [Introduction to JumpStart \- Image Embedding](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/jumpstart_image_embedding/Amazon_JumpStart_Image_Embedding.ipynb)  | 
| Text classification  | Yes  | Yes  | TensorFlow |  [Introduction to JumpStart \- Text Classification](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/jumpstart_text_classification/Amazon_JumpStart_Text_Classification.ipynb)  | 
| Sentence pair classification  | Yes  | Yes  | TensorFlow, Hugging Face |  [Introduction to JumpStart \- Sentence Pair Classification](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/jumpstart_sentence_pair_classification/Amazon_JumpStart_Sentence_Pair_Classification.ipynb)  | 
| Question answering  | Yes  | Yes  | PyTorch, Hugging Face |  [Introduction to JumpStart – Question Answering](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/jumpstart_question_answering/Amazon_JumpStart_Question_Answering.ipynb)  | 
| Named entity recognition  | Yes  | No  | Hugging Face  |  [Introduction to JumpStart \- Named Entity Recognition](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/jumpstart_named_entity_recognition/Amazon_JumpStart_Named_Entity_Recognition.ipynb)  | 
| Text summarization  | Yes  | No  | Hugging Face  |  [Introduction to JumpStart \- Text Summarization](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/jumpstart_text_summarization/Amazon_JumpStart_Text_Summarization.ipynb)  | 
| Text generation  | Yes  | No  | Hugging Face  |  [Introduction to JumpStart \- Text Generation](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/jumpstart_text_generation/Amazon_JumpStart_Text_Generation.ipynb)  | 
| Machine translation  | Yes  | No  | Hugging Face  |  [Introduction to JumpStart \- Machine Translation](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/jumpstart_machine_translation/Amazon_JumpStart_Machine_Translation.ipynb)  | 
| Text embedding  | Yes  | No  | TensorFlow, MXNet |  [Introduction to JumpStart \- Text Embedding](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/jumpstart_text_embedding/Amazon_JumpStart_Text_Embedding.ipynb)  | 
| Tabular classification  | Yes  | Yes  | LightGBM, CatBoost, XGBoost, AutoGluon\-Tabular, TabTransformer, Linear Learner |  [Introduction to JumpStart \- Tabular Classification \- LightGBM, CatBoost](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/lightgbm_catboost_tabular/Amazon_Tabular_Classification_LightGBM_CatBoost.ipynb) [Introduction to JumpStart \- Tabular Classification \- XGBoost, Linear Learner](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/xgboost_linear_learner_tabular/Amazon_Tabular_Classification_XGBoost_LinearLearner.ipynb) [Introduction to JumpStart \- Tabular Classification \- AutoGluon Learner](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/autogluon_tabular/Amazon_Tabular_Classification_AutoGluon.ipynb) [Introduction to JumpStart \- Tabular Classification \- TabTransformer Learner](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/tabtransformer_tabular/Amazon_Tabular_Classification_TabTransformer.ipynb)  | 
| Tabular regression  | Yes  | Yes  | LightGBM, CatBoost, XGBoost, AutoGluon\-Tabular, TabTransformer, Linear Learner |  [Introduction to JumpStart \- Tabular Regression \- LightGBM, CatBoost](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/lightgbm_catboost_tabular/Amazon_Tabular_Regression_LightGBM_CatBoost.ipynb) [Introduction to JumpStart – Tabular Regression \- XGBoost, Linear Learner](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/xgboost_linear_learner_tabular/Amazon_Tabular_Regression_XGBoost_LinearLearner.ipynb) [Introduction to JumpStart – Tabular Regression \- AutoGluon Learner](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/autogluon_tabular/Amazon_Tabular_Regression_AutoGluon.ipynb) [Introduction to JumpStart – Tabular Regression \- TabTransformer Learner](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/tabtransformer_tabular/Amazon_Tabular_Regression_TabTransformer.ipynb)  | 

## Deploy a Model<a name="jumpstart-deploy"></a>

When you deploy a model from JumpStart, SageMaker hosts the model and deploys an endpoint that you can use for inference\. JumpStart also provides an example notebook that you can use to access the model after it's deployed\. 

## Model Deployment Configuration<a name="jumpstart-config"></a>

 After you choose a model, the **Deploy Model** pane opens\. Choose **Deployment Configuration** to configure your model deployment\. 

 ![\[Deploy Model pane option to open settings for Deployment Configuration and Security Settings.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-deploy.png) 

The default instance type for deploying a model depends on the model\. The instance type is the hardware that the training job runs on\. In the following example, the `ml.g4dn.xlarge` instance is the default for this particular BERT model\. 

You can also change the **Endpoint name**\. 

 ![\[JumpStart Deploy Model pane with Deployment Configuration open to select its settings.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-deploy-config.png) 

Choose **Security Settings** to specify the AWS Identity and Access Management \(IAM \) role, Amazon Virtual Private Cloud \(Amazon VPC\), and encryption keys for the model\.

 ![\[JumpStart Deploy Model pane with Security Settings open to select its settings.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-deploy-security.png) 

### Model Deployment Security<a name="jumpstart-config-security"></a>

When you deploy a model with JumpStart, you can specify an IAM role, Amazon VPC, and encryption keys for the model\. If you don't specify any values for these entries: The default IAM role is Your Studio runtime role; default encryption is used; no Amazon VPC is used\.

#### IAM role<a name="jumpstart-config-security-iam"></a>

You can select an IAM role that is passed as part of training jobs and hosting jobs\. SageMaker uses this role to access training data and model artifacts\. If you don't select an IAM role, SageMaker deploys the model using your Studio runtime role\. For more information about IAM roles, see [Identity and Access Management for Amazon SageMaker](security-iam.md)\.

The role that you pass must have access to the resources that the model needs, and must include all of the following\.
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

If you select this option, you must manually enter the ARN for an existing IAM role\. If your Studio runtime role or Amazon VPC block the `iam:list* `call, you must use this option to use an existing IAM role\.

 ![\[JumpStart Security Settings IAM section with Input IAM role selected.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-deploy-security-inputiam.png) 

#### Amazon VPC<a name="jumpstart-config-security-vpc"></a>

All JumpStart models run in network isolation mode\. After the model container is created, no more calls can be made\. You can select an Amazon VPC that is passed as part of training jobs and hosting jobs\. SageMaker uses this Amazon VPC to push and pull resources from your Amazon S3 bucket\. This Amazon VPC is different from the Amazon VPC that limits access to the public internet from your Studio instance\. For more information about the Studio Amazon VPC, see [Connect SageMaker Studio Notebooks in a VPC to External Resources](studio-notebooks-and-internet-access.md)\.

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

If you select this option, you must manually select the subnet and security group that compose your Amazon VPC\. If your Studio runtime role or Amazon VPC blocks the `ec2:list*` call, you must use this option to select the subnet and security group\.

 ![\[JumpStart Security Settings VPC section with Input VPC selected.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-deploy-security-inputvpc.png) 

#### Encryption keys<a name="jumpstart-config-security-encryption"></a>

You can select an AWS KMS key that is passed as part of training jobs and hosting jobs\. SageMaker uses this key to encrypt the Amazon EBS volume for the container, and the repackaged model in Amazon S3 for hosting jobs and the output for training jobs\. For more information about AWS KMS keys, see [AWS KMS keys](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#kms_keys)\.

The key that you pass must trust the IAM role that you pass\. If you do not specify an IAM role, the AWS KMS key must trust your Studio runtime role\.

If you do not select an AWS KMS key, SageMaker provides default encryption for the data in the Amazon EBS volume and the Amazon S3 artifacts\.

**Find encryption keys**

If you select this option, you must select existing AWS KMS keys from the dropdown list\.

 ![\[JumpStart Security Settings encryption section with Find encryption keys selected.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-deploy-security-findencryption.png) 

**Input encryption keys**

If you select this option, you must manually enter the AWS KMS keys\. If your Studio execution role or Amazon VPC block the `kms:list* `call, you must use this option to select existing AWS KMS keys\.

 ![\[JumpStart Security Settings encryption section with Input encryption keys selected.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-deploy-security-inputencryption.png) 

## Fine\-Tune a Model<a name="jumpstart-fine-tune"></a>

 Fine\-tuning trains a pretrained model on a new dataset without training from scratch\. This process, also known as transfer learning, can produce accurate models with smaller datasets and less training time\. 

## Fine\-Tuning Data Source<a name="jumpstart-fine-tune-data"></a>

 When you fine\-tune a model, you can use the default dataset or choose your own data, which is located in an Amazon S3 bucket\. 

 To browse the buckets available to you, choose **Find S3 bucket**\. These buckets are limited by the permissions used to set up your Studio account\. You can also specify an Amazon S3 URI by choosing **Enter Amazon S3 bucket location**\. 

 ![\[JumpStart data source settings with default dataset selected.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-dataset.png) 

**Tip**  
 To find out how to format the data in your bucket, choose **Learn more**\. The description section for the model has detailed information about inputs and outputs\.  

 For text models: 
+  The bucket must have a data\.csv file\. 
+  The first column must be a unique integer for the class label\. For example: `1`, `2`, `3`, `4`, `n`
+  The second column must be a string\. 
+  The second column should have the corresponding text that matches the type and language for the model\.  

 For vision models: 
+  The bucket must have as many subdirectories as the number of classes\. 
+  Each subdirectory should contain images that belong to that class in \.jpg format\. 

**Note**  
 The Amazon S3 bucket must be in the same AWS Region where you're running SageMaker Studio because SageMaker doesn't allow cross\-Region requests\. 

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
+ **Batch size** – The number of records from the dataset that is to be selected for each interval to send to the GPUs for training\. 

  In an image example, you might send out 32 images per GPU, so 32 would be your batch size\. If you choose an instance type with more than one GPU, the batch is divided by the number of GPUs\. Suggested batch size varies depending on the data and the model that you are using\. For example, how you optimize for image data differs from how you handle language data\. 

  In the instance type chart in the deployment configuration section, you can see the number of GPUs per instance type\. Start with a standard recommended batch size \(for example, 32 for a vision model\)\. Then, multiply this by the number of GPUs in the instance type that you selected\. For example, if you're using a `p3.8xlarge`, this would be 32\(batch size\) multiplied by 4 \(GPUs\), for a total of 128, as your batch size adjusts for the number of GPUs\. For a text model like BERT, try starting with a batch size of 64, and then reduce as needed\. 

 ![\[JumpStart Hyperparameter settings with values entered.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-hyperparameters.png) 

## Training Output<a name="jumpstart-training"></a>

When the fine\-tuning process is complete, JumpStart provides information about the model: parent model, training job name, training job Amazon Resource Name \(ARN\), training time, and output path\. The output path is where you can find your new model in an Amazon S3 bucket\. The folder structure uses the model name that you provided and the model file is in an `/output` subfolder and it's always named `model.tar.gz`\.  

 Example: `s3://bucket/model-name/output/model.tar.gz` 