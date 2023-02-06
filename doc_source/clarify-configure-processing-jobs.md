# Amazon SageMaker Clarify Bias Detection and Model Explainability<a name="clarify-configure-processing-jobs"></a>

This topic describes how to configure an Amazon SageMaker Clarify processing job capable of computing bias metrics and feature attributions for explainability\. It is implemented using a specialized SageMaker Clarify container image\. Instructions are provided for how to locate and download one of these container images\. A brief overview of how SageMaker Clarify works is sketched\. The parameters needed to configure the processing job and type of analysis are described\. Prerequisites are outlined and some advice about compute resources consumed by SageMaker Clarify processing job is provided\. For additional information about bias metrics and explainability and how to interpret them, see [Learn How Amazon SageMaker Clarify Helps Detect Bias](http://aws.amazon.com/blogs/machine-learning/learn-how-amazon-sagemaker-clarify-helps-detect-bias), [Fairness Measures for Machine Learning in Finance](https://pages.awscloud.com/rs/112-TZM-766/images/Fairness.Measures.for.Machine.Learning.in.Finance.pdf), and the [Amazon AI Fairness and Explainability Whitepaper](https://pages.awscloud.com/rs/112-TZM-766/images/Amazon.AI.Fairness.and.Explainability.Whitepaper.pdf)\.

## Sample Notebooks<a name="clarify-post-training-bias-model-explainability-sample-notebooks"></a>

Amazon SageMaker Clarify provides the following sample notebook for post\-training bias detection and model explainability:
+ [Amazon SageMaker Clarify Processing](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-clarify/index.html#sagemaker-clarify-processing) – Use SageMaker Clarify to create a processing job for the detecting bias and explaining model predictions with feature attributions\. Examples include using CSV and JSON Lines data formats, bringing your own container, and running processing jobs with Spark\.

This notebook has been verified to run in Amazon SageMaker Studio only\. If you need instructions on how to open a notebook in Amazon SageMaker Studio, see [Create or Open an Amazon SageMaker Studio Notebook](notebooks-create-open.md)\. If you're prompted to choose a kernel, choose **Python 3 \(Data Science\)**\.

**Topics**
+ [Sample Notebooks](#clarify-post-training-bias-model-explainability-sample-notebooks)
+ [Prerequisites](#clarify-processing-job-configure-prerequisites)
+ [How SageMaker Clarify Processing Jobs Work](clarify-processing-job-configure-how-it-works.md)
+ [Get Started with a SageMaker Clarify Container](clarify-processing-job-configure-container.md)
+ [Configure a SageMaker Clarify Processing Job Container's Input and Output Parameters](clarify-processing-job-configure-parameters.md)
+ [Configure the Analysis](clarify-processing-job-configure-analysis.md)
+ [Run SageMaker Clarify Processing Jobs for Bias Analysis and Explainability](clarify-processing-job-run.md)
+ [Detect Post\-training Data and Model Bias with Amazon SageMaker Clarify](clarify-detect-post-training-bias.md)
+ [Amazon SageMaker Clarify Model Explainability](clarify-model-explainability.md)
+ [Troubleshoot SageMaker Clarify Processing Jobs](clarify-processing-job-run-troubleshooting.md)

## Prerequisites<a name="clarify-processing-job-configure-prerequisites"></a>

Before you begin, you need to meet the following prerequisites: 
+ You need to provide an input dataset as tabular files in CSV or [JSON Lines](https://jsonlines.org/) format\. The input dataset must include a label column for bias analysis\. The dataset should be prepared for machine learning with any pre\-processing needed, such as data cleaning or feature engineering, already completed\.
+ You need to provide a model artifact that supports either the CSV or JSON Lines file format as one of its content type inputs\. For post\-training bias metrics and explainability, we use the dataset to make inferences with the model artifact\. Each row minus the label column must be ready to be used as payload for inferences\.
+ When creating processing jobs with the SageMaker container image, you need the following:
  + Network isolation must be disabled for the processing job\.
  + If the model is in a VPC, the processing job must be in the same VPC as the model\.
  + The IAM user of the caller must have permissions for SageMaker APIs\. We recommend that you use the `"arn:aws:iam::aws:policy/AmazonSageMakerFullAccess"` managed policy\.