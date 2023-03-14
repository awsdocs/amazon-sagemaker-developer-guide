# Amazon SageMaker Clarify Bias Detection and Model Explainability<a name="clarify-configure-processing-jobs"></a>

This topic describes how to configure an Amazon SageMaker Clarify processing job which can compute bias metrics and feature attributions for explainability\. Clarify processing jobs are implemented using a specialized SageMaker Clarify container image\. The following **Topics** list contains instructions on how to configure, run and troubleshoot a Clarify processing job and how to configure an analysis\. 

For additional information about bias metrics, explainability and how to interpret them, see [Learn How Amazon SageMaker Clarify Helps Detect Bias](http://aws.amazon.com/blogs/machine-learning/learn-how-amazon-sagemaker-clarify-helps-detect-bias), [Fairness Measures for Machine Learning in Finance](https://pages.awscloud.com/rs/112-TZM-766/images/Fairness.Measures.for.Machine.Learning.in.Finance.pdf), and the [Amazon AI Fairness and Explainability Whitepaper](https://pages.awscloud.com/rs/112-TZM-766/images/Amazon.AI.Fairness.and.Explainability.Whitepaper.pdf)\. 

**Topics**
+ [How SageMaker Clarify Processing Jobs Work](clarify-processing-job-configure-how-it-works.md)
+ [Get Started with a SageMaker Clarify Container](clarify-processing-job-configure-container.md)
+ [Configure a SageMaker Clarify Processing Job](clarify-processing-job-configure-parameters.md)
+ [Configure the Analysis](clarify-processing-job-configure-analysis.md)
+ [Run SageMaker Clarify Processing Jobs for Bias Analysis and Explainability](clarify-processing-job-run.md)
+ [Detect Post\-training Data and Model Bias with Amazon SageMaker Clarify](clarify-detect-post-training-bias.md)
+ [Measure Post\-training Data and Model Bias](clarify-measure-post-training-bias.md)
+ [Amazon SageMaker Clarify Model Explainability](clarify-model-explainability.md)
+ [Sample Notebooks](#clarify-post-training-bias-model-explainability-sample-notebooks)
+ [Troubleshoot SageMaker Clarify Processing Jobs](clarify-processing-job-run-troubleshooting.md)

## Sample Notebooks<a name="clarify-post-training-bias-model-explainability-sample-notebooks"></a>

Amazon SageMaker Clarify provides the following sample notebooks for post\-training bias detection and model explainability:
+ [Fairness and Explainability with SageMaker Clarify](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-clarify/fairness_and_explainability/fairness_and_explainability.html) – Use SageMaker Clarify to create a processing job for detecting bias and explaining model predictions with feature attributions\. You can also see [an example notebook to read a dataset in JSON Lines format](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-clarify/fairness_and_explainability/fairness_and_explainability_jsonlines_format.html)\.
+ Additional example notebooks using Clarify include the following
  + [Bringing your own container](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-clarify/fairness_and_explainability/fairness_and_explainability_byoc.html)
  + [Running processing jobs with Spark](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-clarify/fairness_and_explainability/fairness_and_explainability_spark.html)
  + [Partial dependence plots \(PDPs\)](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-clarify/fairness_and_explainability/explainability_with_pdp.html)
  + Natural language processing \(NLP\) explainability with [text sentiment analysis](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-clarify/text_explainability/text_explainability.html)
  + Computer vision \(CV\) explainability with [image classification](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-clarify/computer_vision/image_classification/explainability_image_classification.html) and [object detection](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-clarify/computer_vision/object_detection/object_detection_clarify.html)

These notebooks have been verified to run in Amazon SageMaker Studio\. If you need instructions on how to open a notebook in Studio, see [Create or Open an Amazon SageMaker Studio Notebook](notebooks-create-open.md)\. If you're prompted to choose a kernel, choose **Python 3 \(Data Science\)**\.