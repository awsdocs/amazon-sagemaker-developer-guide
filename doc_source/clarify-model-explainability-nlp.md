# Explainability Reports for NLP Models<a name="clarify-model-explainability-nlp"></a>

Amazon SageMaker Clarify supports explanations for natural language processing \(NLP\) models\. This enables you to understand which sections of text are most important for the predictions made by your model\. This functionality can be used to explain either an individual local prediction or the model’s expected predictions for overall globsl explanations\. You can define the length of the text segment \(tokens, phrases, sentences, paragraphs\) to understand and visualize model’s behavior at multiple levels of granularity\. 

SageMaker Clarify NLP explainability is compatible with classification and regression models\. You can also use Clarify to explain your model's behavior on multi\-modal data \(both text and tabular features\)\. This feature for multi\-modal datasets can help you understand how important each feature is to model's output for either data type, and how individual features within each class contribute to the outcome\. SageMaker Clarify support 62 languages and text which includes multiple languages\.

To obtain feature importance for parts of an input text, create a `TextConfig` specifying the `granularity` of the parts of the text and the `language`\. Clarify then breaks the text down into tokens, sentences, or paragraphs depending on your choice of `granularity`\. It replaces subsets of these parts with the values from the `baseline`\. The `baseline` defaults to empty strings, meaning SageMaker Clarify simply drops parts of the input text\. SageMaker Clarify then replaces tokens \(sentences, paragraphs, respectively\) to determine how this affects the model’s prediction, in order to assess their importance\. 

```
from sagemaker import clarify

text_config = clarify.TextConfig(
    # Specify the language of your text or use "xx" for multi-language.
    language="en", 
    # Choose the granularity of your explanations as tokens, sentences or paragraphs.
    granularity="token" 
)
shap_config = clarify.SHAPConfig(
    # For now, we don't support global aggregation, 
    # so make sure you retrieve local explanations.
    save_local_shap_values=True,
    text_config=text_config,
    # Determine with what to replace tokens/sentences/paragraphs (based on your choice of granularity) with.
    baseline=[["<UNK>"]],
    # You can further specify num_samples and agg_method.
)
```

The remainder of the explainability job is coded as usual\.

```
explainability_output_path = 's3://{}/{}/clarify-explainability'.format(bucket, prefix)
explainability_data_config = clarify.DataConfig(
    s3_data_input_path=train_uri,
    s3_output_path=explainability_output_path,
    # With text data sometimes detection of headers fails,
    # so we recommend explicitely setting it in the config.
    headers=["text"],
    dataset_type="text/csv")
    
model_config = clarify.ModelConfig(
    model_name=model_name,
    # Specialized hardware is well-suited for NLP models.
    instance_type="ml.p2.xlarge",  
    instance_count=1
)
clarify_processor = clarify.SageMakerClarifyProcessor(
    role=role, instance_count=1, instance_type="ml.m5.xlarge", sagemaker_session=session
)

clarify_processor.run_explainability(
    data_config=explainability_data_config,
    model_config=model_config,
    explainability_config=shap_config)
```

View the local explanations in the `explainability_output_path` S3 bucket\. Currently\. SageMaker Clarify does not aggregate the importance of individual tokens/sentences/paragraphs\.

Extensions:
+ **Multi\-modal data **\- If your data does not just contain text, but also categorical or numeric features, you can simply add additional columns in your header and baseline to accommodate them\.
+ **JSON** \- If the model input or output data is in JSON Lines format, simply specify the `dataset_type` in the `DataConfig` or the `accept_type` and `content_type` in the `ModelConfig`\.
+ **Multi\-class model output** \- If your model does multi\-class or multi\-label predictions, SageMaker Clarify computes the importance for each class\.
+ **Model output** \- Some model output contain more thatn a single score\. A prediction of three examples, returns a matrix of shape `[3, 1]` such as `[[0.1], [-1.1], [10.2], [0.0], [3.2]]`\. You can specify `model_scores` in `run_explainability`\. For example, if your model predicts probabilities of five classes and additionally outputs the winning class, `[["c1", [0.1, 0.4, 0.3, 0.2]]` for example, then you can set `model_scores=1`\.

Tips for fast results:
+ During the development phase, work with a small subset of examples for quick iterations\.
+ To scale up for deployment, increase the `instance_count` for the processing job and the predictor\. This instructs SageMaker Clarify to partition the data and work with multiple machines in parallel\. For parallel processing, we recommend using the parquet data format since Spark can struggle with special characters in CSV files\.
+ Choose an appropriate `instance_type` for your model\. GPU instances are often suitable for NLP models\.
+ To save unnecessary computation, consider truncating your text\. Many models work with a maximum text length\. By applying the truncation in a preprocessing step, SageMaker Clarify does not waste resources determining the feature importance for the parts of text that are disregarded by the model\.