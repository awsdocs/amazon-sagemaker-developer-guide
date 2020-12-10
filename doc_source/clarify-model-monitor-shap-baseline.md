# Create a SHAP baseline for models in production<a name="clarify-model-monitor-shap-baseline"></a>

Explanations are typically contrastive, that is, they account for deviations from a baseline\. For information on explainability baselines, see [SHAP baselines for explainability](clarify-feature-attribute-shap-baselines.md)\.

In addition to providing explanations for per\-instance inferences, SageMaker Clarify also supports global explanation for ML models that helps us understand the behavior of a model as a whole in terms of its features\. We generate a global explanation of an ML model by aggregating the Shapley values over multiple instances\. Our offering supports three different ways of aggregation\. These forms of aggregation can be used to define baselines\.
+ `mean_abs`: mean of absolute SHAP values for all instances
+ `median`: median of SHAP values for all instances
+ `mean_sq`: mean of squared SHAP values for all instances

After you have configured your application to capture real\-time inference data, the first task to monitor for drift in feature attribution is to create a baseline to compare against\. This involves configuring the data inputs, which groups are sensitive, how the predictions are captures, and the model and its post\-training bias metrics\. Then the baseline job needs starting\. Model explainability monitor can explain the predictions of a deployed model producing inferences and detect feature attribution drift on a regular basis\.

```
model_explainability_monitor = ModelExplainabilityMonitor(
    role=role,
    sagemaker_session=sagemaker_session,
    max_runtime_in_seconds=1800,
)
```

In this example, the explainability baselining job shares the test dataset with the bias baselining job, so here it uses the same `DataConfig`, the only difference is the job output URI\.

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

Currently the Clarify explainer offers a scalable and efficient implementation of SHAP, so the explainability config is SHAPConfig, including
+ `baseline`: A list of rows \(at least one\) or S3 object URI to be used as the baseline dataset in the Kernel SHAP algorithm\. The format should be the same as the dataset format\. Each row should contain only the feature columns/values and omit the label column/values\.
+ `num_samples`: Number of samples to be used in the Kernel SHAP algorithm\. This number determines the size of the generated synthetic dataset to compute the SHAP values\.
+ agg\_method: Aggregation method for global SHAP values\. Valid values are
  + `mean_abs`: mean of absolute SHAP values for all instances
  + `median`: median of SHAP values for all instances
  + `mean_sq`: mean of squared SHAP values for all instances
+ `use_logit`: Indicator of whether the logit function is to be applied to the model predictions\. Default is False\. If "use\_logit" is true then the SHAP values will have log\-odds units\.
+ `save_local_shap_values` \(bool\): Indicator of whether to save the local SHAP values in the output location\. Default is `True`\.

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

Kick off baselining job\. The same model\_config is required, because the explainability baselining job needs to create shadow endpoint to get predictions for generated synthetic dataset\.

```
model_explainability_monitor.suggest_baseline(
    data_config=model_explainability_data_config,
    model_config=model_config,
    explainability_config=shap_config,
)
print(f"ModelExplainabilityMonitor baselining job: {model_explainability_monitor.latest_baselining_job_name}")
```

Text

```
code
```