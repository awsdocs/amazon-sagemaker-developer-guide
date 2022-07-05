# Create Feature Attribute Baselines and Explainability Reports<a name="clarify-feature-attribute-baselines-reports"></a>

For an example notebook with instructions on how to run a SageMaker Clarify processing job in Studio that creates explanations for its predictions relative to a baseline, see [Explainability and bias detection with Amazon SageMaker Clarify](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker_processing/fairness_and_explainability/fairness_and_explainability.html)\.

If you need instructions on how to open a notebook in Amazon SageMaker Studio, see [Create or Open an Amazon SageMaker Studio Notebook](notebooks-create-open.md)\. The following code examples are taken from the example notebook listed previously\. This section discusses the code related to the use of Shapley values to provide reports that compare the relative contributions each feature made the predictions\.

Use `SHAPConfig` to create the baseline\. In this example, the `mean_abs` is the mean of absolute SHAP values for all instances, specified as the baseline\. You use `DataConfig` to configure the target variable, data input and output paths, and their formats\.

```
shap_config = clarify.SHAPConfig(baseline=[test_features.iloc[0].values.tolist()],
                                 num_samples=15,
                                 agg_method='mean_abs')

explainability_output_path = 's3://{}/{}/clarify-explainability'.format(bucket, prefix)
explainability_data_config = clarify.DataConfig(s3_data_input_path=train_uri,
                                s3_output_path=explainability_output_path,
                                label='Target',
                                headers=training_data.columns.to_list(),
                                dataset_type='text/csv')
```

**Note**  
Kernel SHAP in SageMaker Clarify supports omitting the “baseline” parameter\. In this case, a baseline based on clustering the input dataset is generated automatically\.

Then run the explainability job\.

```
clarify_processor.run_explainability(data_config=explainability_data_config,
                                     model_config=model_config,
                                     explainability_config=shap_config)
```

To view Partial Dependence plots \(PDP\), use `explainability_config=PDP_config`\.

You can select both types of reports with `explainability_config=[PDP_config,shap_config]`\.

View the results in Studio or download them from the `explainability_output_path` S3 bucket\.