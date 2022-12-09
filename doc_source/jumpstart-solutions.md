# Solution Templates<a name="jumpstart-solutions"></a>

SageMaker JumpStart provides one\-click, end\-to\-end solutions for many common machine learning use cases\. Explore the following use cases for more information on available solution templates\.
+ [Demand forecasting](#jumpstart-solutions-demand-forecasting)
+ [Credit rating prediction](#jumpstart-solutions-credit-prediction)
+ [Fraud detection](#jumpstart-solutions-fraud-detection)
+ [Computer vision](#jumpstart-solutions-computer-vision)
+ [Extract and analyze data from documents](#jumpstart-solutions-documents)
+ [Predictive maintenance](#jumpstart-solutions-predictive-maintenance)
+ [Churn prediction](#jumpstart-solutions-churn-prediction)
+ [Personalized recommendations](#jumpstart-solutions-recommendations)
+ [Reinforcement learning](#jumpstart-solutions-reinforcement-learning)
+ [Healthcare and life sciences](#jumpstart-solutions-healthcare-life-sciences)
+ [Financial pricing](#jumpstart-solutions-financial-pricing)
+ [Causal inference](#jumpstart-solutions-causal-inference)

Choose the solution template that best fits your use case from the JumpStart landing page\. When you choose a solution template, JumpStart opens a new tab showing a description of the solution and a **Launch** button\. When you select **Launch**, JumpStart creates all of the resources that you need to run the solution, including training and model hosting instances\. For more information on launching a JumpStart solution, see [Launch a Solution](jumpstart-solutions-launch.md)\.

After launching the solution, you can explore solution features and any generated artifacts in JumpStart\. Use the **Launched Quick start assets** menu to find your solution\. In your solution's tab, select **Open Notebook** to use provided notebooks and explore the solution’s features\. When artifacts are generated during launch or after running the provided notebooks, they're listed in the **Generated Artifacts** table\. You can delete individual artifacts with the trash icon \(![\[The trash icon for JumpStart.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/jumpstart/jumpstart-trash.png)\)\. You can delete all of the solution’s resources by choosing **Delete solution resources**\.

## Demand forecasting<a name="jumpstart-solutions-demand-forecasting"></a>

Demand forecasting uses historical time series data in order to make future estimations in relation to customer demand over a specific period and streamline the supply\-demand decision\-making process across businesses\. 

Demand forecasting use cases include predicting ticket sales in the transportation industry, stock prices, number of hospital visits, number of customer representatives to hire for multiple locations in the next month, product sales across multiple regions in the next quarter, cloud server usage for the next day for a video streaming service, electricity consumption for multiple regions over the next week, number of IoT devices and sensors such as energy consumption, and more\.

Time series data is categorized as *univariate* and *multi\-variate*\. For example, the total electricity consumption for a single household is a univariate time series over a period of time\. When multiple univariate time series are stacked on each other, it’s called a multi\-variate time series\. For example, the total electricity consumption of 10 different \(but correlated\) households in a single neighborhood make up a multi\-variate time series dataset\.


| Solution name  | Description  | Get started  | 
| --- | --- | --- | 
| Demand forecasting  | Demand forecasting for multivariate time series data using three state\-of\-the\-art time series forecasting algorithms: [LSTNet](https://ts.gluon.ai/api/gluonts/gluonts.model.lstnet.html), [Prophet](https://facebook.github.io/prophet/), and [SageMaker DeepAR](https://docs.aws.amazon.com/sagemaker/latest/dg/deepar.html)\. |  [GitHub »](https://github.com/awslabs/sagemaker-deep-demand-forecast)  | 

## Credit rating prediction<a name="jumpstart-solutions-credit-prediction"></a>

Use JumpStart's credit rating prediction solutions to predict corporate credit ratings or to explain credit prediction decisions made by machine learning models\. Compared to traditional credit rating modeling methods, machine learning models can automate and improve the accuracy of credit prediction\. 


| Solution name  | Description  | Get started  | 
| --- | --- | --- | 
| Corporate credit rating prediction  | Multimodal \(long text and tabular\) machine learning for quality credit predictions using AWS [AutoGluon Tabular](https://auto.gluon.ai/stable/tutorials/tabular_prediction/index.html)\. | [GitHub »](https://github.com/awslabs/sagemaker-corporate-credit-rating) | 
| Graph\-based credit scoring  | Predict corporate credit ratings using tabular data and a corporate network by training a [Graph Neural Network GraphSAGE](https://cs.stanford.edu/people/jure/pubs/graphsage-nips17.pdf) and AWS [AutoGluon Tabular](https://auto.gluon.ai/stable/tutorials/tabular_prediction/index.html) model\. | Find in Amazon SageMaker Studio\.  | 
| Explain credit decisions  | Predict credit default in credit applications and provide explanations using [LightGBM](https://lightgbm.readthedocs.io/en/latest/) and [SHAP \(SHapley Additive exPlanations\)](https://shap.readthedocs.io/en/latest/index.html)\. |  [GitHub »](https://github.com/awslabs/sagemaker-explaining-credit-decisions)  | 

## Fraud detection<a name="jumpstart-solutions-fraud-detection"></a>

Many businesses lose billions annually to fraud\. Machine learning based fraud detection models can help systematically identify likely fraudulent activities from a tremendous amount of data\. The following solutions use transaction and user identity datasets to identify fraudulent transactions\.


| Solution name  | Description  | Get started  | 
| --- | --- | --- | 
| Detect malicious users and transactions | Automatically detect potentially fraudulent activity in transactions using [SageMaker XGBoost](https://docs.aws.amazon.com/sagemaker/latest/dg/xgboost.html) with the over\-sampling technique [Synthetic Minority Over\-sampling](https://arxiv.org/abs/1106.1813) \(SMOTE\)\. |  [GitHub »](https://github.com/awslabs/fraud-detection-using-machine-learning)  | 
| Fraud detection in financial transactions using deep graph library | Detect fraud in financial transactions by training a [graph convolutional network](https://arxiv.org/pdf/1703.06103.pdf) with the [deep graph library](https://www.dgl.ai/) and a [SageMaker XGBoost](https://docs.aws.amazon.com/sagemaker/latest/dg/xgboost.html) model\. |  [GitHub »](https://github.com/awslabs/sagemaker-graph-fraud-detection)  | 
| Financial payment classification | Classify financial payments based on transaction information using [SageMaker XGBoost](https://docs.aws.amazon.com/sagemaker/latest/dg/xgboost.html)\. Use this solution template as an intermediate step in fraud detection, personalization, or anomaly detection\. |  Find in Amazon SageMaker Studio\.  | 

## Computer vision<a name="jumpstart-solutions-computer-vision"></a>

With the rise of business use cases such as autonomous vehicles, smart video surveillance, healthcare monitoring and various object counting tasks, fast and accurate object detection systems are rising in demand\. These systems involve not only recognizing and classifying every object in an image, but localizing each one by drawing the appropriate bounding box around it\. In the last decade, the rapid advances of deep learning techniques greatly accelerated the momentum of object detection\.


| Solution name  | Description  | Get started  | 
| --- | --- | --- | 
| Visual product defect detection | Identify defective regions in product images either by training an [object detection model from scratch](https://ieeexplore.ieee.org/document/8709818) or fine\-tuning pretrained SageMaker models\. |  [GitHub »](https://github.com/awslabs/sagemaker-defect-detection)  | 
| Handwriting recognition  | Recognize handwritten text in images by training an [object detection model](https://mxnet.apache.org/versions/1.0.0/api/python/gluon/model_zoo.html#mxnet.gluon.model_zoo.vision.resnet34_v1) and [handwriting recognition model](https://arxiv.org/abs/1910.00663)\. Label your own data using [SageMaker Ground Truth](http://aws.amazon.com/sagemaker/data-labeling/)\. | [GitHub »](https://github.com/awslabs/sagemaker-handwritten-text-recognition) | 
| Object detection for bird species | Identify birds species in a scene using a [SageMaker object detection model](https://docs.aws.amazon.com/sagemaker/latest/dg/object-detection.html)\. |  Find in Amazon SageMaker Studio\.  | 

## Extract and analyze data from documents<a name="jumpstart-solutions-documents"></a>

JumpStart provides solutions for you to uncover valuable insights and connections in business\-critical documents\. Use cases include text classification, document summarization, handwriting recognition, relationship extraction, question and answering, and filling in missing values in tabular records\.


| Solution name  | Description  | Get started  | 
| --- | --- | --- | 
| Privacy for sentiment classification  | [Anonymize text ](https://www.amazon.science/blog/preserving-privacy-in-analyses-of-textual-data) to better preserve user privacy in sentiment classification\. |  [GitHub »](https://github.com/awslabs/sagemaker-privacy-for-nlp)  | 
| Document understanding | Document summarization, entity, and relationship extraction using the [transformers](https://huggingface.co/docs/transformers/index) library in PyTorch\. |  [GitHub »](https://github.com/awslabs/sagemaker-document-understanding)  | 
| Handwriting recognition  | Recognize handwritten text in images by training an [object detection model](https://mxnet.apache.org/versions/1.0.0/api/python/gluon/model_zoo.html#mxnet.gluon.model_zoo.vision.resnet34_v1) and [handwriting recognition model](https://arxiv.org/abs/1910.00663)\. Label your own data using [SageMaker Ground Truth](http://aws.amazon.com/sagemaker/data-labeling/)\. | [GitHub »](https://github.com/awslabs/sagemaker-handwritten-text-recognition) | 
| Filling in missing values in tabular records  | Fill missing values in tabular records by training a [SageMaker AutoPilot](http://aws.amazon.com/sagemaker/autopilot/) model\. |  [GitHub »](https://github.com/awslabs/filling-in-missing-values-in-tabular-records)  | 

## Predictive maintenance<a name="jumpstart-solutions-predictive-maintenance"></a>

Predictive maintenance aims to optimize the balance between corrective and preventative maintenance by facilitating the timely replacement of components\. The following solutions use sensor data from industrial assets to predict machine failures, unplanned downtime, and repair costs\.


| Solution name  | Description  | Get started  | 
| --- | --- | --- | 
| Predictive maintenance for vehicle fleets  | Predict vehicle fleet failures using vehicle sensor and maintenance information with a convolutional neural network model\. |  [GitHub »](https://github.com/awslabs/aws-fleet-predictive-maintenance/)  | 
| Predictive maintenance for manufacturing  | Predict the remaining useful life for each sensor by training a [stacked Bidirectional LSTM neural network](https://arxiv.org/pdf/1801.02143.pdf) model using historical sensor readings\. |  [GitHub »](https://github.com/awslabs/predictive-maintenance-using-machine-learning)  | 

## Churn prediction<a name="jumpstart-solutions-churn-prediction"></a>

Customer churn, or rate of attrition, is a costly problem faced by a wide range of companies\. In an effort to reduce churn, companies can identify customers that are likely to leave their service in order to focus their efforts on customer retention\. Use a JumpStart churn prediction solution to analyze data sources such as user behavior and customer support chat logs to identify customers that are at a high risk of cancelling a subscription or service\.


| Solution name  | Description  | Get started  | 
| --- | --- | --- | 
| Churn prediction with text  | Predict churn using numerical, categorical, and textual features with [BERT encoder](https://huggingface.co/) and [RandomForestClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html)\. |  [GitHub »](https://github.com/awslabs/sagemaker-churn-prediction-text)  | 
| Churn prediction for mobile phone customers | Identify unhappy mobile phone customers using [SageMaker XGBoost](https://docs.aws.amazon.com/sagemaker/latest/dg/xgboost.html)\. |  Find in Amazon SageMaker Studio\.  | 

## Personalized recommendations<a name="jumpstart-solutions-recommendations"></a>

You can use JumpStart solutions to analyze customer identity graphs or user sessions to better understand and predict customer behavior\. Use the following solutions for personalized recommendations to model customer identity across multiple devices or to determine the likelihood of a customer making a purchase\. 


| Solution name  | Description  | Get started  | 
| --- | --- | --- | 
| Entity resolution in identity graphs with deep graph library  | Perform cross\-device entity linking for online advertising by training a [graph convolutional network](https://arxiv.org/pdf/1703.06103.pdf) with [deep graph library](https://www.dgl.ai/)\. |  [GitHub »](https://github.com/awslabs/sagemaker-graph-entity-resolution)  | 
| Purchase modeling | Predict whether a customer will make a purchase by training a [SageMaker XGBoost](https://docs.aws.amazon.com/sagemaker/latest/dg/xgboost.html) model\. |  [GitHub »](https://github.com/awslabs/sagemaker-purchase-modelling)  | 

## Reinforcement learning<a name="jumpstart-solutions-reinforcement-learning"></a>

Reinforcement learning \(RL\) is a type of learning that is based on interaction with the environment\. This type of learning is used by an agent that must learn behavior through trial\-and\-error interactions with a dynamic environment in which the goal is to maximize the long\-term rewards that the agent receives as a result of its actions\. Rewards are maximized by trading off exploring actions that have uncertain rewards with exploiting actions that have known rewards\.

RL is well\-suited for solving large, complex problems, such as supply chain management, HVAC systems, industrial robotics, game artificial intelligence, dialog systems, and autonomous vehicles\. 


| Solution name  | Description  | Get started  | 
| --- | --- | --- | 
| Reinforcement learning for Battlesnake AI competitions  | Provide a reinforcement learning workflow for training and inference with the [BattleSnake](https://play.battlesnake.com/) AI competitions\. |  [GitHub »](https://github.com/awslabs/sagemaker-battlesnake-ai)  | 
| Distributed reinforcement learning for Procgen challenge  | Distributed reinforcement learning starter kit for [NeurIPS 2020 Procgen](https://www.aicrowd.com/challenges/neurips-2020-procgen-competition) Reinforcement learning challenge\. | [GitHub »](https://github.com/aws-samples/sagemaker-rl-procgen-ray) | 

## Healthcare and life sciences<a name="jumpstart-solutions-healthcare-life-sciences"></a>

Clinicians and researchers can use JumpStart solutions to analyze medical imagery, genomic information, and clinical health records\. 


| Solution name  | Description  | Get started  | 
| --- | --- | --- | 
| Lung cancer survival prediction | Predict non\-small cell lung cancer patient survival status with 3\-dimensional lung computerized tomography \(CT\) scans, genomic data, and clinical health records using [SageMaker XGBoost](https://docs.aws.amazon.com/sagemaker/latest/dg/xgboost.html)\. |  [GitHub »](https://github.com/aws-samples/machine-learning-pipelines-for-multimodal-health-data/tree/sagemaker-soln-lcsp)  | 

## Financial pricing<a name="jumpstart-solutions-financial-pricing"></a>

Many businesses dynamically adjust pricing on a regular basis in order to maximize their returns\. Use the following JumpStart solutions for price optimization, dynamic pricing, option pricing, or portfolio optimization use cases\. 


| Solution name  | Description  | Get started  | 
| --- | --- | --- | 
| Price optimization |  Estimate price elasticity using Double Machine Learning \(ML\) for causal inference and the [Prophet](https://facebook.github.io/prophet/) forecasting procedure\. Use these estimates to optimize daily prices\.  |  Find in Amazon SageMaker Studio\.  | 

## Causal inference<a name="jumpstart-solutions-causal-inference"></a>

Researchers can use machine learning models such as Bayesian networks to represent causal dependencies and draw causal conclusions based on data\. Use the following JumpStart solution to understand the causal relationship between Nitrogen\-based fertilizer application and corn crop yields\.


| Solution name  | Description  | Get started  | 
| --- | --- | --- | 
| Crop yield counterfactuals |  Generate a counterfactual analysis of corn response to nitrogen\. This solution learns the crop phenology cycle in its entirety using multi\-spectral satellite imagery and [ground\-level observations](https://www.sciencedirect.com/science/article/pii/S2352340921010283#tbl0001)\.  |  Find in Amazon SageMaker Studio\.  | 