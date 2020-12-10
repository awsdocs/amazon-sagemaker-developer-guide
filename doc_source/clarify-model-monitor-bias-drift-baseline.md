# Create a bias drift baseline<a name="clarify-model-monitor-bias-drift-baseline"></a>

After you have configured your application to capture real\-time inference data, the first task to monitor for bias drift is to create a baseline\. This involves configuring the data inputs, which groups are sensitive, how the predictions are captures, and the model and its post\-training bias metrics\. Then the baseline job needs starting\.

Model bias monitor can detect bias drift of Machine Learning models in a regular basis\. Similar to the other monitoring types, the standard procedure of creating a model bias monitor is first baselining and then establishing a monitoring schedule\.

```
model_bias_monitor = ModelBiasMonitor(
    role=role,
    sagemaker_session=sagemaker_session,
    max_runtime_in_seconds=1800,
)
```

`DataConfig` stores information about the dataset to be analyzed, for example the dataset file, its format \(CSV or JSONLines\), headers \(if any\) and label\.

```
model_bias_baselining_job_result_uri = f"{baseline_results_uri}/model_bias"
model_bias_data_config = DataConfig(
    s3_data_input_path=validation_dataset,
    s3_output_path=model_bias_baselining_job_result_uri,
    label=label_header,
    headers=all_headers,
    dataset_type=dataset_type,
)
```

`BiasConfig` is the configuration of the sensitive groups in the dataset\. Typically, bias is measured by computing a metric and comparing it across groups\. The group of interest is called the "facet\." For post\-training bias, the positive label should also be taken into account\.

```
model_bias_config = BiasConfig(
    label_values_or_threshold=[1],
    facet_name="Account Length",
    facet_values_or_threshold=[100],
)
```

`ModelPredictedLabelConfig` specifies how to extract a predicted label from the model output\. Here the 0\.8 cutoff has been chosen in anticipation that customers will churn\. For more complicated outputs, there are a few more options, like "label" is the index, name or JSONPath to locate predicted label in endpoint response payload\.

```
model_predicted_label_config = ModelPredictedLabelConfig(
    probability_threshold=0.8,
)
```

`ModelConfig` is configuration related to model to be used for inferencing\. In order to compute post\-training bias metrics, the computation needs to get inferences for the model name provided\. To accomplish this, the processing job uses the model to create an ephemeral endpoint \(also known as "shadow endpoint"\)\. The processing job deletes the shadow endpoint after the computations are completed\. This configuration is also used by the explainability monitor\.

```
model_config = ModelConfig(
    model_name=model_name,
    instance_count=endpoint_instance_count,
    instance_type=endpoint_instance_type,
    content_type=dataset_type,
    accept_type=dataset_type,
)
```

Now you can start the baselining job\.

```
model_bias_monitor.suggest_baseline(
    model_config=model_config,
    data_config=model_bias_data_config,
    bias_config=model_bias_config,
    model_predicted_label_config=model_predicted_label_config,
)
print(f"ModelBiasMonitor baselining job: {model_bias_monitor.latest_baselining_job_name}")
```

The monitor scheduled automatically picks up baselining job name and waits for it before monitoring begins\.