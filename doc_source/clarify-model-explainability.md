# Amazon SageMaker Clarify Model Explainability<a name="clarify-model-explainability"></a>

Amazon SageMaker Clarify provides tools to help explain how machine learning \(ML\) models make predictions\. These tools can help ML modelers and developers and other internal stakeholders understand model characteristics as a whole prior to deployment and to debug predictions provided by the model after it's deployed\. Transparency about how ML models arrive at their predictions is also critical to consumers and regulators\. They need to trust the model predictions if they are going to accept the decisions based on them\. SageMaker Clarify uses a model\-agnostic feature attribution approach\. You can use this to understand why a model made a prediction after training, and to provide per\-instance explanation during inference\. The implementation includes a scalable and efficient implementation of [SHAP](https://papers.nips.cc/paper/2017/file/8a20a8621978632d76c43dfd28b67767-Paper.pdf)\. This is based on the concept of a Shapley value, from the field of cooperative game theory, that assigns each feature an importance value for a particular prediction\.

Clarify produces partial dependence plots \(PDPs\) that show the marginal effect features have on the predicted outcome of a machine learning model\. Partial dependence helps explain target response given a set of input features\. It also supports both computer vision \(CV\) and natural language processing \(NLP\) explainability using the same Shapley Values \(SHAP\) algorithm as used for tabular data explanations\.

What is the function of an explanation in the machine learning context? An explanation can be thought of as the answer to a *Why question* that helps humans understand the cause of a prediction\. In the context of an ML model, you might be interested in answering questions such as: 
+ Why did the model predict a negative outcome such as a loan rejection for a given applicant? 
+ How does the model make predictions?
+ Why did the model make an incorrect prediction?
+ Which features have the largest influence on the behavior of the model?

You can use explanations for auditing and meeting regulatory requirements, building trust in the model and supporting human decision\-making, and debugging and improving model performance\.

The need to satisfy the demands for human understanding about the nature and outcomes of ML inference is key to the sort of explanation needed\. Research from philosophy and cognitive science disciplines has shown that people care especially about contrastive explanations, or explanations of why an event X happened instead of some other event Y that did not occur\. Here, X could be an unexpected or surprising event that happened and Y corresponds to an expectation based on their existing mental model referred to as a *baseline*\. Note that for the same event X, different people might seek different explanations depending on their point of view or mental model Y\. In the context of explainable AI, you can think of X as the example being explained and Y as a baseline that is typically chosen to represent an uninformative or average example in the dataset\. Sometimes, for example in the case of ML modeling of images, the baseline might be implicit, where an image whose pixels are all the same color can serves as a baseline\.

## Sample Notebooks<a name="clarify-model-explainability-sample-notebooks"></a>

Amazon SageMaker Clarify provides the following sample notebook for model explainability:
+ [Amazon SageMaker Clarify Processing](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-clarify/index.html#sagemaker-clarify-processing) – Use SageMaker Clarify to create a processing job for the detecting bias and explaining model predictions with feature attributions\. Examples include using CSV and JSONLines data formats, bringing your own container, and running processing jobs with Spark\.
+ [Explaining Image Classification with SageMaker Clarify](https://github.com/aws/amazon-sagemaker-examples/blob/master/sagemaker-clarify/computer_vision/image_classification/explainability_image_classification.ipynb) – SageMaker Clarify provides you with insights into how your computer vision models classify images\.
+ [Explaining object detection models with SageMaker Clarify ](https://github.com/aws/amazon-sagemaker-examples/blob/main/sagemaker-clarify/computer_vision/object_detection/object_detection_clarify.ipynb) – SageMaker Clarify provides you with insights into how your computer vision models detect objects\.

This notebook has been verified to run in Amazon SageMaker Studio only\. If you need instructions on how to open a notebook in Amazon SageMaker Studio, see [Create or Open an Amazon SageMaker Studio Notebook](notebooks-create-open.md)\. If you're prompted to choose a kernel, choose **Python 3 \(Data Science\)**\.

**Topics**
+ [Sample Notebooks](#clarify-model-explainability-sample-notebooks)
+ [Feature Attributions that Use Shapley Values](clarify-shapley-values.md)
+ [SHAP Baselines for Explainability](clarify-feature-attribute-shap-baselines.md)
+ [Partial Dependence Plots: Analysis Configuration and Output](clarify-partial-dependence-plots.md)
+ [SageMaker Clarify Computer Vision](clarify-model-explainability-computer-vision.md)
+ [Create Feature Attribute Baselines and Explainability Reports](clarify-feature-attribute-baselines-reports.md)
+ [Explainability Reports for NLP Models](clarify-model-explainability-nlp.md)