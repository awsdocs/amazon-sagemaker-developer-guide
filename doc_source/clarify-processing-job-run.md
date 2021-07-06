# Run SageMaker Clarify Processing Jobs for Bias Analysis and Explainability<a name="clarify-processing-job-run"></a>

You use SageMaker Clarify processing jobs to analyze potential sources of bias in your training data and to check your trained model for bias\. For the procedure to analyze the data in SageMaker Studio, see [Generate Reports for Bias in Pretraining Data in SageMaker Studio](clarify-data-bias-reports-ui.md)\. The focus here is on posttraining bias metric and SHAP values for explainability\. Model predictions can be a source of bias \(for example, if they make predictions that more frequently produce a negative result for one group than another\)\. SageMaker Clarify is integrated with SageMaker Experiments so that after a model has been trained, you can identify attributes you would like to check for bias \(for example, income\)\. SageMaker runs a set of algorithms to check the trained model and provides you with a visual report on the different types of bias for each attribute, such as whether high\-income earners receive more positive predictions compared to low\-income earners\.

**Topics**
+ [Compute Resources Required for SageMaker Clarify Processing Jobs](#clarify-processing-job-run-resources)
+ [Run the Clarify Processing Job](#clarify-processing-job-run-code)
+ [Run the Clarify Processing Job with Spark](#clarify-processing-job-run-spark)
+ [Get the Analysis Results](#clarify-processing-job-run-analysis-results)

## Compute Resources Required for SageMaker Clarify Processing Jobs<a name="clarify-processing-job-run-resources"></a>

Take the following into consideration when determining the compute resources you need to run SageMaker Clarify processing jobs:
+ Processing jobs can take several minutes or more to complete\.
+ Computing explanations can be more time intensive than the actual inference\. This includes the time to launch compute resources\.
+ Computing explanations can be more compute intensive than the actual inference\. Review and monitor the charges you may incur from using SageMaker resources\. For more information, see [Amazon SageMaker Pricing](http://aws.amazon.com/sagemaker/pricing/)\. 

## Run the Clarify Processing Job<a name="clarify-processing-job-run-code"></a>

For an example notebook with instructions on how to run a SageMaker Clarify processing job in Studio to detect posttraining model bias, see [Explainability and bias detection with Amazon SageMaker Clarify](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker_processing/fairness_and_explainability/fairness_and_explainability.html)\.

If you need instructions on how to open a notebook in Amazon SageMaker Studio, see [Create or Open an Amazon SageMaker Studio Notebook](notebooks-create-open.md)\. The following code samples are taken from the example notebook listed previously\.

After you have trained your model, instantiate the SageMaker Clarify processor using the following command:

```
from sagemaker import clarify
clarify_processor = clarify.SageMakerClarifyProcessor(role=role,
                                                      instance_count=1,
                                                      instance_type='ml.c4.xlarge',
                                                      sagemaker_session=session)
```

Next, configure the input dataset, where to store the output, the label column targeted with a `DataConfig` object, specify information about your trained model with `ModelConfig`, and provide information about the formats of your predictions with `ModelPredictedLabelConfig`\.

```
bias_report_output_path = 's3://{}/{}/clarify-bias'.format(bucket, prefix)
bias_data_config = clarify.DataConfig(s3_data_input_path=train_uri,
                                      s3_output_path=bias_report_output_path,
                                      label='Target',
                                      headers=training_data.columns.to_list(),
                                      dataset_type='text/csv')

model_config = clarify.ModelConfig(model_name=model_name,
                                   instance_type='ml.c5.xlarge',
                                   instance_count=1,
                                   accept_type='text/csv')

predictions_config = clarify.ModelPredictedLabelConfig(probability_threshold=0.8)
```

Use `BiasConfig` to provide information on which columns contain the facets \(sensitive groups, `Sex`\), what the sensitive features \(`facet_values_or_threshold`\) might be, and what the desirable outcomes are \(`label_values_or_threshold`\)\. 

```
bias_config = clarify.BiasConfig(label_values_or_threshold=[1],
                                facet_name='Sex',
                                facet_values_or_threshold=[0])
```

You can run both the pretraining and posttraining analysis in the processing job at the same time with `run_bias()`\. 

```
clarify_processor.run_bias(data_config=bias_data_config,
                           bias_config=bias_config,
                           model_config=model_config,
                           model_predicted_label_config=predictions_config,
                           pre_training_methods='all',
                           post_training_methods='all')
```

View the results in Studio or download them from the `bias_report_output_path` S3 bucket\.

## Run the Clarify Processing Job with Spark<a name="clarify-processing-job-run-spark"></a>

[Apache Spark](https://spark.apache.org/) is a unified analytics engine for large\-scale data processing\. When working with large datasets, you can use the Spark processing capabilities of SageMaker Clarify to enable your Clarify processing jobs to run faster\. To use Spark processing for Clarify jobs, set the instance count to a number greater than one\. Clarify uses Spark distributed computing when there is more than once instance per Clarify processor\.

The following example shows how to use [SageMakerClarifyProcessor](https://sagemaker.readthedocs.io/en/stable/api/training/processing.html#sagemaker.processing.Processor) to create a Clarify processor with 5 instances\. 

Clarify runs any jobs associated with this processor using Spark distributed processing\. 

```
from sagemaker import clarify
clarify_processor = clarify.SageMakerClarifyProcessor(role=role,
                         instance_count=5,
                         instance_type='ml.c5.xlarge',
                         max_runtime_in_seconds=1200,
                         volume_size_in_gb=100)
```

If you configure a Clarify job to save local SHAP values in the [SHAPConfig](https://sagemaker.readthedocs.io/en/stable/api/training/processing.html#sagemaker.clarify.SHAPConfig) class, Spark saves the local SHAP value as multiple part files in parallel\.

If you add more instances, we recommend that you also increase the number of instances in the model configuration [ModelConfig](https://sagemaker.readthedocs.io/en/stable/api/training/processing.html#sagemaker.clarify.ModelConfig) for the shadow endpoint\. This is to prevent the processing instances from being bottlenecked by the shadow endpoint\. Specifically, we recommend that you use a one\-to\-one ratio of endpoint to processing instances\. 

## Get the Analysis Results<a name="clarify-processing-job-run-analysis-results"></a>

After the processing job is finished, you can download the output files to inspect or visualize the results in Studio\. The output directory contains the following files:
+ `analysis.json` – Bias metrics and SHAP values in JSON format\.
+ `report.ipynb` – Static notebook to visualize the bias metrics and SHAP values\.
+ `explanations_shap/out.csv` – Local \(per\-instance\) SHAP values for each row in the dataset in the same format as input dataset\. On each row, the output file contains SHAP values for each feature and the predicted label\.

In the analysis JSON file, the bias metrics and SHAP values are organized into three separate sections\.

```
{
    "explanations": { . . . }
    "pre_training_bias_metrics": { . . . }
    "post_training_bias_metrics": { . . . }
}
```

SHAP values are in the `“explanations”` section\. Values correspond to the global SHAP value for each feature column\.

```
    "explanations": {
        "kernel_shap": {
            "label0": {
                "global_shap_values": {
                    "feature_0": 0.022486410860333206,
                    "feature_1": 0.007381025261958729,
                    "feature_2": 0.006843906804137847
                },
                "expected_value": 0.508233428001
            }
        }
    }
```

The bias metrics are in the pretraining and posttraining bias metrics sections\.

```
{
    "post_training_bias_metrics": {
        "label": "target",
        "label_value_or_threshold": "1",
        "facets": {
            "feature_2": [
                {
                    "value_or_threshold": "1",
                    "metrics": [
                        {
                            "name": "DI",
                            "description": "Disparate Impact (DI)",
                            "value": 0.711340206185567
                        },
                        {
                            "name": "DCR",
                            "description": "Difference in Conditional Rejections (DCR)",
                            "value": -0.34782608695652184
                        }
                        { . . . }                        
                    ]
                }
            ]
        }
    }
}
```

For more information on bias metrics and SHAP values and how to interpret them, see the [Amazon AI Fairness and Explainability Whitepaper](https://pages.awscloud.com/rs/112-TZM-766/images/Amazon.AI.Fairness.and.Explainability.Whitepaper.pdf)\.

A bar chart of top SHAP values and tables with the bias metrics are provided in the report notebook\.