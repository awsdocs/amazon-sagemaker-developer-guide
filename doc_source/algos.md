# Use Amazon SageMaker built\-in algorithms<a name="algos"></a>

A machine learning algorithm uses example data to create a generalized solution \(a *model*\) that addresses the business question you are trying to answer\. After you create a model using example data, you can use it to answer the same business question for a new set of data\. This is also referred to as obtaining inferences\.

Amazon SageMaker provides several built\-in machine learning algorithms that you can use for a variety of problem types\. 

Because you create a model to address a business question, your first step is to understand the problem that you want to solve\. Specifically, the format of the answer that you are looking for influences the algorithm that you choose\. For example, suppose that you are a bank marketing manager, and that you want to conduct a direct mail campaign to attract new customers\. Consider the potential types of answers that you're looking for:
+ Answers that fit into discrete categories—For example, answers to these questions:

   
  + "Based on past customer responses, should I mail this particular customer?" Answers to this question fall into two categories, "yes" or "no\." In this case, you use the answer to narrow the recipients of the mail campaign\.

     
  + "Based on past customer segmentation, which segment does this customer fall into?" Answers might fall into categories such as "empty nester," "suburban family," or "urban professional\." You could use these segments to decide who should receive the mailing\.

     

  For this type of discrete classification problem, SageMaker provides two algorithms: [Linear learner algorithm](linear-learner.md) and the [XGBoost Algorithm](xgboost.md)\. You set the following hyperparameters to direct these algorithms to produce discrete results:

   
  + For the Linear Learner algorithm, set the `predictor_type` hyperparameter to `binary_classifier`\. 

     
  + For the XGBoost algorithm, set the `objective` hyperparameter to `reg:logistic`\.

   
+ Answers that are quantitative—Consider this question: "Based on the return on investment \(ROI\) from past mailings, what is the ROI for mailing this customer?” In this case, you use the ROI to target customers for the mail campaign\. For these quantitative analysis problems, you can also use the [Linear learner algorithm](linear-learner.md) or the [XGBoost Algorithm](xgboost.md) algorithms\. You set the following hyperparameters to direct these algorithms to produce quantitative results:

   
  + For the Linear Learner algorithm, set the `predictor_type` hyperparameter to `regressor`\. 

     
  + For the XGBoost algorithm, set the `objective` hyperparameter to `reg:linear`\.

   
+ Answers in the form of discrete recommendations—Consider this question: "Based on past responses to mailings, what is the recommended content for each customer?" In this case, you are looking for a recommendation on what to mail, not whether to mail, the customer\. For this problem, SageMaker provides the [Factorization Machines Algorithm](fact-machines.md) algorithm\.

   

All of the questions in the preceding examples rely on having example data that includes answers\. There are times that you don't need, or can't get, example data with answers\. This is true for problems whose answers identify groups\. For example:
+ "I want to group current and prospective customers into 10 groups based on their attributes\. How should I group them? " You might choose to send the mailing to customers in the group that has the highest percentage of current customers\. That is, prospective customers that most resemble current customers based on the same set of attributes\. For this type of question, SageMaker provides the [K\-Means Algorithm](k-means.md)\.

   
+ "What are the attributes that differentiate these customers, and what are the values for each customer along those dimensions\." You use these answers to simplify the view of current and prospective customers, and, maybe, to better understand these customer attributes\. For this type of question, SageMaker provides the [Principal Component Analysis \(PCA\) Algorithm](pca.md) algorithm\.

In addition to these general\-purpose algorithms, SageMaker provides algorithms that are tailored to specific use cases\. These include:
+ [Image Classification Algorithm](image-classification.md)—Use this algorithm to classify images\. It uses example data with answers \(referred to as *supervised algorithm*\)\.

   
+ [Sequence\-to\-Sequence Algorithm](seq-2-seq.md)—This supervised algorithm is commonly used for neural machine translation\. 

   
+ [Latent Dirichlet Allocation \(LDA\) Algorithm](lda.md)—This algorithm is suitable for determining topics in a set of documents\. It is an *unsupervised algorithm*, which means that it doesn't use example data with answers during training\.

   
+ [Neural Topic Model \(NTM\) Algorithm](ntm.md)—Another unsupervised technique for determining topics in a set of documents, using a neural network approach\.

**Topics**
+ [Common elements of built\-in algorithms](common-info-all-im-models.md)
+ [BlazingText algorithm](blazingtext.md)
+ [DeepAR Forecasting Algorithm](deepar.md)
+ [Factorization Machines Algorithm](fact-machines.md)
+ [Image Classification Algorithm](image-classification.md)
+ [IP Insights Algorithm](ip-insights.md)
+ [K\-Means Algorithm](k-means.md)
+ [K\-Nearest Neighbors \(k\-NN\) Algorithm](k-nearest-neighbors.md)
+ [Latent Dirichlet Allocation \(LDA\) Algorithm](lda.md)
+ [Linear learner algorithm](linear-learner.md)
+ [Neural Topic Model \(NTM\) Algorithm](ntm.md)
+ [Object2Vec Algorithm](object2vec.md)
+ [Object Detection Algorithm](object-detection.md)
+ [Principal Component Analysis \(PCA\) Algorithm](pca.md)
+ [Random Cut Forest \(RCF\) Algorithm](randomcutforest.md)
+ [Semantic Segmentation Algorithm](semantic-segmentation.md)
+ [Sequence\-to\-Sequence Algorithm](seq-2-seq.md)
+ [XGBoost Algorithm](xgboost.md)