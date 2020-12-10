# Monitor feature attribution drift for models in production<a name="clarify-model-monitor-feature-attribution-drift"></a>

Amazon SageMaker Clarify provides tools to help us explain the predictions of a deployed machine learning \(ML\) model producing inferences\. These tools can help ML modelers and developers and other internal stakeholders understand model characteristics as a whole prior to deployment and to debug predictions provided by the model once deployed\. Transparency about how ML models arrive at their predictions is also critical to consumers and regulators who need to trust the model predictions if they are going to make decisions based on them\. SageMaker Clarify use a on model\-agnostic feature attribution approaches, which can be used both for global understanding of a model after training and for providing per\-instance explanation during inference\. The implementation includes a scalable and efficient implementation of SHAP, based on the concept of a Shapley value from the field of cooperative game theory that assigns each feature an importance value for a particular prediction\. For more information on model explainability and the approach to it using Shalpey values, see [Model explainability](clarify-model-explainability.md)

## Model Monitor sample notebook<a name="clarify-model-monitor-sample-notebooks-feature-drift"></a>

Amazon SageMaker Clarify provides the following sample notebook that shows how to capture real\-time inference data, create a baseline to monitor evolving bias against, ad inspect the results\. The following topics contain the highlights from the last two steps\.
+ [Monitoring bias drift and feature attribution drift Amazon SageMaker Clarify](https://github.com/aws/amazon-sagemaker-examples/blob/master/sagemaker_model_monitor/fairness_and_explainability/SageMaker-Model-Monitor-Fairness-and-Explainability.ipynb): Use Amazon SageMaker Model Monitor to monitor bias drift and feature attribution drift over time\.

This notebook has been verified to run in Amazon SageMaker Studio only\. If you need instructions on how to open a notebook in Amazon SageMaker Studio, see [Create or Open an Amazon SageMaker Studio Notebook](notebooks-create-open.md)\. Select the **Python 3 \(Data Science\)** kernal if prompted to choose one\. The following code samples are taken from the example notebook linked\. 

**Topics**
+ [Model Monitor sample notebook](#clarify-model-monitor-sample-notebooks-feature-drift)
+ [Create a SHAP baseline for models in production](clarify-model-monitor-shap-baseline.md)
+ [Schedule feature attribute drift monitoring jobs](clarify-model-monitor-feature-attribute-drift-schedule.md)
+ [Inspect reports for feature attribute drift in production models](clarify-feature-attribute-drift-report.md)