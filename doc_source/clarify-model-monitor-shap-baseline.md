# Create a SHAP Baseline for Models in Production<a name="clarify-model-monitor-shap-baseline"></a>

Explanations are typically contrastive, that is, they account for deviations from a baseline\. For information on explainability baselines, see [SHAP Baselines for Explainability](clarify-feature-attribute-shap-baselines.md)\.

In addition to providing explanations for per\-instance inferences, SageMaker Clarify also supports global explanation for ML models that helps you understand the behavior of a model as a whole in terms of its features\. SageMaker Clarify generates a global explanation of an ML model by aggregating the Shapley values over multiple instances\. SageMaker Clarify supports the following different ways of aggregation, which you can use to define baselines:
+ `mean_abs` – Mean of absolute SHAP values for all instances\.
+ `median` – Median of SHAP values for all instances\.
+ `mean_sq` – Mean of squared SHAP values for all instances\.

After you have configured your application to capture real\-time or batch transform inference data, the first task to monitor for drift in feature attribution is to create a baseline to compare against\. This involves configuring the data inputs, which groups are sensitive, how the predictions are captured, and the model and its posttraining bias metrics\. Then you need to start the baselining job\. Model explainability monitor can explain the predictions of a deployed model that's producing inferences and detect feature attribution drift on a regular basis\.

```
model_explainability_monitor = ModelExplainabilityMonitor(
    role=role,
    sagemaker_session=sagemaker_session,
    max_runtime_in_seconds=1800,
)
```

In this example, the explainability baselining job shares the test dataset with the bias baselining job, so it uses the same `DataConfig`, and the only difference is the job output URI\.

```
model_explainability_baselining_job_result_uri = f"{baseline_results_uri}/model_explainability"
model_explainability_data_config = DataConfig(
    s3_data_input_path=validation_dataset,
    s3_output_path=model_explainability_baselining_job_result_uri,
    label=label_header,
    headers=all_headers,
    dataset_type=dataset_type,
)
```

Currently the SageMaker Clarify explainer offers a scalable and efficient implementation of SHAP, so the explainability config is SHAPConfig, including the following:
+ `baseline` – A list of rows \(at least one\) or S3 object URI to be used as the baseline dataset in the Kernel SHAP algorithm\. The format should be the same as the dataset format\. Each row should contain only the feature columns/values and omit the label column/values\.
+ `num_samples` – Number of samples to be used in the Kernel SHAP algorithm\. This number determines the size of the generated synthetic dataset to compute the SHAP values\.
+ agg\_method – Aggregation method for global SHAP values\. Following are valid values:
  + `mean_abs` – Mean of absolute SHAP values for all instances\.
  + `median` – Median of SHAP values for all instances\.
  + `mean_sq` – Mean of squared SHAP values for all instances\.
+ `use_logit` – Indicator of whether the logit function is to be applied to the model predictions\. Default is `False`\. If `use_logit` is `True`, the SHAP values will have log\-odds units\.
+ `save_local_shap_values` \(bool\) – Indicator of whether to save the local SHAP values in the output location\. Default is `False`\.

```
# Here use the mean value of test dataset as SHAP baseline
test_dataframe = pd.read_csv(test_dataset, header=None)
shap_baseline = [list(test_dataframe.mean())]

shap_config = SHAPConfig(
    baseline=shap_baseline,
    num_samples=100,
    agg_method="mean_abs",
    save_local_shap_values=False,
)
```

Start a baselining job\. The same `model_config` is required because the explainability baselining job needs to create a shadow endpoint to get predictions for the generated synthetic dataset\.

```
model_explainability_monitor.suggest_baseline(
    data_config=model_explainability_data_config,
    model_config=model_config,
    explainability_config=shap_config,
)
print(f"ModelExplainabilityMonitor baselining job: {model_explainability_monitor.latest_baselining_job_name}")
```