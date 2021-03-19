# Model Explainability<a name="clarify-model-explainability"></a>

Amazon SageMaker Clarify provides tools to help explain how machine learning \(ML\) models make predictions\. These tools can help ML modelers and developers and other internal stakeholders understand model characteristics as a whole prior to deployment and to debug predictions provided by the model after it's deployed\. Transparency about how ML models arrive at their predictions is also critical to consumers and regulators who need to trust the model predictions if they are going to accept the decisions based on them\. SageMaker Clarify uses a model\-agnostic feature attribution approach, which you can used to understand why a model made a prediction after training and to provide per\-instance explanation during inference\. The implementation includes a scalable and efficient implementation of [SHAP](https://papers.nips.cc/paper/2017/file/8a20a8621978632d76c43dfd28b67767-Paper.pdf), based on the concept of a Shapley value from the field of cooperative game theory that assigns each feature an importance value for a particular prediction\.

What is the function of an explanation in the machine learning context? An explanation can be thought of as the answer to a *Why question* that helps humans understand the cause of a prediction\. In the context of an ML model, you might be interested in answering questions such as: 
+ “Why did the model predict a negative outcome such as a loan rejection for a given applicant?” 
+ “How does the model make predictions?” 
+ “Why did the model make an incorrect prediction?"
+ "Which features have the largest influence on the behavior of the model?” 

You can use explanations for auditing and meeting regulatory requirements, building trust in the model and supporting human decision\-making, and debugging and improving model performance\.

The need to satisfy the demands for human understanding about the nature and outcomes of ML inference is key to the sort of explanation needed\. Research from philosophy and cognitive science disciplines has shown that people care especially about contrastive explanations, or explanations of why an event X happened instead of some other event Y that did not occur\. Here, X could be an unexpected or surprising event that happened and Y corresponds to an expectation based on their existing mental model referred to as a *baseline*\. Note that for the same event X, different people might seek different explanations depending on their point of view or mental model Y\. In the context of explainable AI, you can think of X as the example being explained and Y as a baseline that is typically chosen to represent an uninformative or average example in the dataset\. Sometimes, the baseline might be implicit as with an image with all pixels of the same color in the case of ML models for images\.

**Topics**
+ [Feature Attributions that Use Shapley Values](clarify-shapley-values.md)
+ [SHAP Baselines for Explainability](clarify-feature-attribute-shap-baselines.md)
+ [Create Feature Attribute Baselines and Explainability Reports](clarify-feature-attribute-baselines-reports.md)