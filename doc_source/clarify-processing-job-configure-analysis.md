# Configure the Analysis<a name="clarify-processing-job-configure-analysis"></a>

To analyze your data and models for explainability and bias using SageMaker Clarify, you must configure a [Clarify processing job](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-processing-job-configure-parameters.html)\. Part of the configuration for this processing job includes the configuration of an analysis file\. The analysis file specifies the parameters for bias analysis and explainability\.

This guide describes the schema and parameters for this analysis configuration file\. This guide also includes examples of analysis configuration files for computing bias metrics for a tabular dataset, and generating explanations for natural language processing \(NLP\) and computer vision \(CV\) problems\.

You can create the analysis configuration file or use the [SageMaker Python SDK](https://sagemaker.readthedocs.io/) to generate one for you with the [SageMaker ClarifyProcessor](https://sagemaker.readthedocs.io/en/stable/api/training/processing.html#sagemaker.clarify.SageMakerClarifyProcessor) API\. Viewing the file contents can be helpful for understanding the underlying configuration used by the SageMaker Clarify job\.

**Topics**
+ [Schema for the analysis configuration file](#clarify-processing-job-configure-schema)
+ [Example analysis configuration files](#clarify-processing-job-configure-analysis-examples)

## Schema for the analysis configuration file<a name="clarify-processing-job-configure-schema"></a>

The following section describes the schema for the analysis configuration file including requirements and descriptions of parameters\.

### Requirements for the analysis configuration file<a name="clarify-processing-job-configure-schema-requirements"></a>

 The Clarify processing job expects the analysis configuration file to be structured with the following requirements:
+ The processing input name must be `analysis_config.`
+ The analysis configuration file is in JSON format, and encoded in UTF\-8\.
+ The analysis configuration file is an Amazon S3 object\.

You can specify additional parameters in the analysis configuration file\. The following section provides various options to tailor the Clarify processing job for your use case and desired types of analysis\.

### Parameters for analysis configuration files<a name="clarify-processing-job-configure-analysis-parameters"></a>

In the analysis configuration file, you can specify the following parameters\.
+ **version** – \(Optional\) The version string of the analysis configuration file schema\. If a version is not provided, SageMaker Clarify uses the latest supported version\. Currently, the only supported version is `1.0`\.
+ **dataset\_type** – The format of the dataset\. The input dataset format can be any of the following values:
  + `text/csv` for CSV
  + `application/jsonlines` for [SageMaker JSON Lines dense format](https://docs.aws.amazon.com/sagemaker/latest/dg/cdf-inference.html#cm-jsonlines)
  + `application/x-parquet` for Apache Parquet
  + `application/x-image` to activate explainability for computer vision problems
+ **dataset\_uri** – \(Optional\) The uniform resource identifier \(URI\) of the main dataset\. If you provide a S3 URI prefix, the Clarify processing job recursively collects all S3 files located under the prefix\. You can provide either a S3 URI prefix or a S3 URI to an image manifest file for computer vision problems\. If `dataset_uri` is provided, it takes precedence over the dataset processing job input\. For any format type except image, the Clarify processing job loads the input dataset into a tabular data frame, as a **tabular dataset**\. This format allows SageMaker to easily manipulate and analyze the input dataset\.
+ **headers** – \(Optional\) An array of strings containing the column names of a tabular dataset\. If a value for `headers` is not provided, then the Clarify processing job reads the headers from the dataset\. If the dataset doesn’t have headers, then the Clarify processing job will automatically generate placeholder names based on zero\-based column index\. As an example, placeholder names for the first and second columns will be **column\_0**, **column\_1**\.
**Note**  
By convention, if dataset\_type is "application/jsonlines" then `headers` should contain the following names in order: feature names, label name \(if `label` is specified\), and predicted label name \(if `predicted_label` is specified\)\. An example for `headers` for an "application/jsonlines" dataset type if `label` is specified is: `["feature1","feature2","feature3","target_label"]`\.
+ **label** – \(Optional\) A string or a zero\-based integer index\. If provided, `label` is used to locate the ground truth label, also known as an observed label or target attribute in a tabular dataset\. The ground truth label is used to compute bias metrics\. The value for `label` is specified depending on the value of the `dataset_type` parameter as follows\.
  + If `dataset_type` is **text/csv**, `label` can be specified as either of the following:
    + A valid column name
    + An index that lies within the range of dataset columns
  + If `dataset_type` is **application/x\-parquet**, `label` must be a valid column name\.
  + If `dataset_type` is **application/jsonlines**, `label` must be a [JMESPath](https://jmespath.org/) expression written to extract the ground truth label from the dataset\. By convention, if `headers` is specified, then it should contain the label name\.
+ **predicted\_label** – \(Optional\) A string or a zero\-based integer index\. If provided, `predicted_label` is used to locate the column containing the predicted label in a tabular dataset\. The predicted label is used to compute post\-training **bias metrics**\. The parameter `predicted_label` is optional if the dataset doesn’t include predicted label\. If predicted labels are required for computation, then the Clarify processing job will get predictions from the model\.

  The value for `predicted_label` is specified depending on the value of the `dataset_type` as follows:
  + If `dataset_type` is **text/csv**, `predicted_label` can be specified as either of the following:
    + A valid column name\. If `predicted_label_dataset_uri` is specified, but `predicted_label` is not provided, the default predicted label name is "predicted\_label"\. 
    + An index that lies within the range of dataset columns\. If `predicted_label_dataset_uri` is specified, then the index is used to locate the predicted label column in the predicted label dataset\.
  + If dataset\_type is **application/x\-parquet**, `predicted_label` must be a valid column name\.
  + If dataset\_type is **application/jsonlines**, `predicted_label` must be a valid [JMESPath](https://jmespath.org/) expression written to extract the ground truth label from the dataset\. By convention, if `headers` is specified, then it should contain the predicted label name\. 
+ **features** – Required if dataset\_type is `application/jsonlines`\. A JMESPath \(string\) expression written to locate the features in the input dataset\. For a `dataset_type` of `text/csv` or `application/x-parquet` all columns except for the ground truth label and predicted label columns are automatically assigned as features\.
+ **predicted\_label\_dataset\_uri** – Only applicable when dataset\_type is `text/csv`\. The S3 URI for a dataset containing predicted labels used to compute post\-training **bias metrics**\. The Clarify processing job will load the predictions from the provided URI instead of getting predictions from the model\. In this case, `predicted_label` is required to locate the predicted label column in the predicted label dataset\. If the predicted label dataset or the main dataset is split across multiple files, an identifier column must be specified by `joinsource_name_or_index` to join the two datasets\. 
+ **predicted\_label\_headers** – Only applicable when `predicted_label_dataset_uri` is specified\. An array of strings containing the column names of the predicted label dataset\. Besides the predicted label header, `predicted_label_headers` can also contain the header of the identifier column to join the predicted label dataset and the main dataset\. For more information, see the description for the parameter `joinsource_name_or_index` below\.
+ **joinsource\_name\_or\_index** – The name or zero\-based index of the column in tabular datasets to be used as a identifier column while performing an inner join\. This column is only used as an identifier\. It isn't used for any other computations like bias analysis or feature attribution analysis\. A value for `joinsource_name_or_index` is needed in the following cases:
  + There are multiple input datasets, and any one is split across multiple files
  + Distributed processing is activated by setting the Clarify processing job [InstanceCount](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ProcessingClusterConfig.html#sagemaker-Type-ProcessingClusterConfig-InstanceCount) to a value greater than `1`
+ **excluded\_columns** – \(Optional\) An array of names or zero\-based indices of columns to be excluded from being sent to the model as input for predictions\. Ground truth label and predicted label are automatically excluded already\.
+ **probability\_threshold** – \(Optional\) A floating point number above which, a label or object is selected\. The default value is `0.5`\. The Clarify processing job uses `probability_threshold` in the following cases:
  + In post\-training bias analysis, `probability_threshold` converts a numeric model prediction \(probability value or score\) to a binary label, if the model is a binary classifier\. A score greater than the threshold is converted to `1`\. Whereas, a score less than or equal to the threshold is converted to `0`\.
  + In computer vision explainability problems, if model\_type is **OBJECT\_DETECTION**`, probability_threshold` filters out objects detected with confidence scores lower than the threshold value\.
+ **label\_values\_or\_threshold** – Required for bias analysis\. An array of label values or a threshold number, which indicate positive outcome for ground truth and predicted labels for bias metrics\. For more information, see positive label values in [Amazon SageMaker Clarify Terms for Bias and Fairness](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-detect-data-bias.html#clarify-bias-and-fairness-terms)\. If the label is numeric, the threshold is applied as the lower bound to select the positive outcome\. To set `label_values_or_threshold` for different problem types, refer to the following examples:
  + For a binary classification problem, the label has two possible values, `0` and `1`\. If label value `1` is favorable to a demographic group observed in a sample, then `label_values_or_threshold` should be set to `[1]`\.
  + For a multi\-class classification problem, the label has three possible values, **bird**, **cat**, and **dog**\. If the latter two define a demographic group that bias favors, then `label_values_or_threshold` should be set to `["cat","dog"]`\.
  + For a regression problem, the label value is continuous, ranging from `0` to `1`\. If a value greater than `0.5` should designate a sample as having a positive result, then `label_values_or_threshold` should be set to `0.5`\.
+ **facet** – Required for bias analysis\. An array of facet objects, which are composed of sensitive attributes against which bias is measured\. You can use facets to understand the bias characteristics of your dataset and model even if your model is trained without using sensitive attributes\. For more information, see **Facet** in [Amazon SageMaker Clarify Terms for Bias and Fairness](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-detect-data-bias.html#clarify-bias-and-fairness-terms)\. Each facet object includes the following fields:
  + **name\_or\_index** – The name or zero\-based index of the sensitive attribute column in a tabular dataset\. If `facet_dataset_uri` is specified, then the index refers to the facet dataset instead of the main dataset\.
  + **value\_or\_threshold** – Required if `facet` is numeric and `label_values_or_threshold` is applied as the lower bound to select the sensitive group\)\. An array of facet values or a threshold number, that indicates the sensitive demographic group that bias favors\. If facet data type is categorical and `value_or_threshold` is not provided, bias metrics are computed as one group for every unique value \(rather than all values\)\. To set `value_or_threshold` for different `facet` data types, refer to the following examples:
    + For a binary facet data type, the feature has two possible values, `0` and `1`\. If you want to compute the bias metrics for each value, then `value_or_threshold` can be either omitted or set to an empty array\.
    + For a categorical facet data type, the feature has three possible values **bird**, **cat**, and **dog**\. If the first two define a demographic group that bias favors, then `value_or_threshold` should be set to `["bird", "cat"]`\. In this example, the dataset samples are split into two demographic groups\. The facet in the advantaged group has value **bird** or **cat**, while the facet in the disadvantaged group has value **dog**\.
    + For a numeric facet data type, the feature value is continuous, ranging from `0` to `1`\. As an example, if a value greater than `0.5` should designate a sample as favored, then `value_or_threshold` should be set to `0.5`\. In this example, the dataset samples are split into two demographic groups\. The facet in the advantaged group has value greater than `0.5`, while the facet in the disadvantaged group has value less than or equal to `0.5`\.
+ **group\_variable** – The name or zero\-based index of the column that indicates the subgroup to be used for the bias metric [Conditional Demographic Disparity](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-data-bias-metric-cddl.html) \(CDD\) or [Conditional Demographic Disparity in Predicted Labels](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-post-training-bias-metric-cddpl.html) \(CDDPL\)\.
+ **facet\_dataset\_uri** – Only applicable when dataset\_type is `text/csv`\. The S3 URI for a dataset containing sensitive attributes for bias analysis\. You can use facets to understand the bias characteristics of your dataset and model even if your model is trained without using sensitive attributes\.
**Note**  
If the facet dataset or the main dataset is split across multiple files, an identifier column must be specified by `joinsource_name_or_index` to join the two datasets\. You must use the parameter `facet` to identify each facet in the facet dataset\.
+ **facet\_headers** – \(Only applicable when `facet_dataset_uri` is specified\) An array of strings containing column names for the facet dataset, and optionally, the identifier column header to join the facet dataset and the main dataset, see `joinsource_name_or_index`\.
+ **methods** – An object containing one or more analysis methods and their parameters\. If any method is omitted, it is neither used for analysis nor reported\.
  + **pre\_training\_bias** – Include this method if you want to compute pre\-training bias metrics\. The detailed description of the metrics can be found on [Measure Pre\-training Bias](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-measure-data-bias.html)\. The object has the following parameters:
    + **methods** – An array that contains any of the pre\-training bias metrics from the following list that you want to compute\. Set `methods` to **all** to compute all pre\-training bias metrics\. As an example, the array `["CI", "DPL"]` will compute **Class Imbalance** and **Difference in Proportions of Labels**\.
      + `CI` for [Class Imbalance](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-bias-metric-class-imbalance.html)
      + `DPL` for [Difference in Proportions of Labels](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-data-bias-metric-true-label-imbalance.html)
      + `KL` for [Kullback\-Leibler Divergence](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-data-bias-metric-kl-divergence.html)
      + `JS` for [Jensen\-Shannon Divergence](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-data-bias-metric-jensen-shannon-divergence.html)
      + `LP` for [Lp\-norm](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-data-bias-metric-lp-norm.html)
      + `TVD` for [Total Variation Distance](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-data-bias-metric-total-variation-distance.html)
      + `KS` for [Kolmogorov\-Smirnov](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-data-bias-metric-kolmogorov-smirnov.html)
      + `CDDL` for [Conditional Demographic Disparity](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-data-bias-metric-cddl.html)
  + **post\_training\_bias** – Include this method if you want to compute post\-training bias metrics\. The detailed description of the metrics can be found on [Measure Post\-training Bias](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-measure-post-training-bias.html)\. The `post_training_bias` object has the following parameters\.
    + **methods** – An array that contains any of the post\-training bias metrics from the following list that you want to compute\. Set `methods` to **all** to compute all post\-training bias metrics\. As an example, the array `["DPPL", "DI"]` computes the **Difference in Positive Proportions in Predicted Labels** and **Disparate Impact**\. The available methods are as follows\.
      + `DPPL` for [Difference in Positive Proportions in Predicted Labels](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-post-training-bias-metric-dppl.html)
      + `DI`for [Disparate Impact](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-post-training-bias-metric-di.html)
      + `DCA` for [Difference in Conditional Acceptance](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-post-training-bias-metric-dcacc.html)
      + `DCR` for [Difference in Conditional Rejection](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-post-training-bias-metric-dcr.html)
      + `SD` for [Specificity difference](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-post-training-bias-metric-sd.html)
      + `RD` for [Recall Difference](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-post-training-bias-metric-rd.html)
      + `DAR` for [Difference in Acceptance Rates](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-post-training-bias-metric-dar.html)
      + `DRR` for [Difference in Rejection Rates](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-post-training-bias-metric-drr.html)
      + `AD` for [Accuracy Difference](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-post-training-bias-metric-ad.html)
      + `TE` for [Treatment Equality](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-post-training-bias-metric-te.html)
      + `CDDPL` for [Conditional Demographic Disparity in Predicted Labels](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-post-training-bias-metric-cddpl.html)
      + `FT` for [Counterfactual Fliptest](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-post-training-bias-metric-ft.html)
      + `GE` for [Generalized entropy](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-post-training-bias-metric-ge.html)
  + **shap** – Include this method if you want to compute SHAP values\. The Clarify processing job supports the Kernel SHAP algorithm\. The `shap` object has the following parameters\.
    + **baseline** – \(Optional\) The [SHAP baseline](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-feature-attribute-shap-baselines.html) dataset, also known as the background dataset\. Additional requirements for the baseline dataset in a tabular dataset or computer vision problem are as follows\.
      + For a **tabular** dataset, `baseline` can be either the in\-place baseline data or the S3 URI of a baseline file\. If `baseline` is not provided, the Clarify processing job computes a baseline by clustering the input dataset\. The following are required of the baseline:
        + The format must be the same as the dataset format specified by `dataset_type`\.
        + The baseline can only contain features that the model can accept as input\.
        + The baseline dataset can have one or more instances\. The number of baseline instances directly affects the synthetic dataset size and job runtime\.
        + If `text_config` is specified, then the baseline value of a text column is a string used to replace the unit of text specified by `granularity`\. For example, one common placeholder is "\[MASK\]", which is used to represent a missing or unknown word or piece of text\. 

        The following examples show how to set in\-place baseline data for different `dataset_type` parameters:
        + If `dataset_type` is either `text/csv` or `application/x-parquet`, the model accepts four numeric features, and the baseline has two instances\. In addition, in this example, if one record has all zero feature values and the other record has all one feature values, then baseline should be set to `[[0,0,0,0],[1,1,1,1]]`, without any header\.
        + If `dataset_type` is `application/jsonlines`, and `features` is the key to a list of four numeric feature values\. In addition, in this example, if the baseline has one record of all zero values, then `baseline` should be `[{"features":[0,0,0,0]}]`\.
      + For **computer vision** problems, `baseline` can be the S3 URI of an image that is used to mask out features \(segments\) from the input image\. The Clarify processing job loads the mask image and resizes it to the same resolution as the input image\. If baseline is not provided, the Clarify processing job generates a mask image of [white noise](https://en.wikipedia.org/wiki/White_noise) at the same resolution as the input image\.
    + **num\_clusters** – \(Optional\) The number of clusters that the dataset is divided into to compute the baseline dataset\. Each cluster is used to compute one baseline instance\. If `baseline` is not specified, the Clarify processing job attempts to compute the baseline dataset by dividing the tabular dataset into an optimal number of clusters between `1` and `12`\. The number of baseline instances directly affects the runtime of SHAP analysis\.
    + **num\_samples** – \(Optional\) The number of samples to be used in the Kernel SHAP algorithm\. If `num_samples` is not provided, the Clarify processing job chooses the number for you\. The number of samples directly affects both the synthetic dataset size and job runtime\.
    + **seed**\(Optional\) An integer used to initialize the pseudo random number generator in the SHAP explainer to generate consistent SHAP values for the same job\. If seed is not specified, then each time that the same job runs, the model may output slightly different SHAP values\. 
    + **use\_logit** – \(Optional\) A Boolean value that indicates that you want the logit function to be applied to the model predictions\. Defaults to `false`\. If `use_logit` is `true`, then the SHAP values are calculated using the logistic regression coefficients, which can be interpreted as log\-odds ratios\.
    + **save\_local\_shap\_values** – \(Optional\) A Boolean value that indicates that you want the local SHAP values of each record in the dataset to be included in the analysis result\. Defaults to `false`\.

      If the main dataset is split across multiple files or distributed processing is activated, also specify an identifier column using the parameter j`oinsource_name_or_index`\. The identifier column and the local SHAP values are saved in the analysis result so that you can map each record to its local SHAP values\.
    + **agg\_method** – \(Optional\) The method used to aggregate the local SHAP values \(the SHAP values for each instance\) of all instances to the global SHAP values \(the SHAP values for the entire dataset\)\. Defaults to `mean_abs`\. The following methods can be used to aggregate SHAP values\.
      + **mean\_abs** – The mean of absolute local SHAP values of all instances\.
      + **mean\_sq** – The mean of squared local SHAP values of all instances\.
      + **median** – The median of local SHAP values of all instances\.
    + **text\_config** – Required for [natural language processing explainability](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-model-explainability-nlp.html)\. Include this configuration if you want to treat text columns as text and explanations should be provided for individual units of text\.
      + **granularity** – The unit of granularity for the analysis of text columns\. Valid values are "token", "sentence", or "paragraph"\. **Each unit of text is considered a feature**, and local SHAP values are computed for each unit\.
      + **language** – The language of the text columns\. Valid values are **chinese**, **danish**, **dutch**, **english**, **french**, **german**, **greek**, **italian**, **japanese**, **lithuanian**, **multi\-language**, **norwegian bokmål**, **polish**, **portuguese**, **romanian**, **russian**, **spanish**, **afrikaans**, **albanian**, **arabic**, **armenian**, **basque**, **bengali**, **bulgarian**, **catalan**, **croatian**, **czech**, **estonian**, **finnish**, **gujarati**, **hebrew**, **hindi**, **hungarian**, **icelandic**, **indonesian**, **irish**, **kannada**, **kyrgyz**, **latvian**, **ligurian**, **luxembourgish**, **macedonian**, **malayalam**, **marathi**, **nepali**, **persian**, **sanskrit**, **serbian**, **setswana**, **sinhala**, **slovak**, **slovenian**, **swedish**, **tagalog**, **tamil**, **tatar**, **telugu**, **thai**, **turkish**, **ukrainian**, **urdu**, **vietnamese**, **yoruba**\. Enter `multi-language` for a mix of multiple languages\.
      + **max\_top\_tokens** – \(Optional\) The maximum number of top tokens, based on global SHAP values\. Defaults to `50`\. It is possible for a token to appear multiple times in the dataset\. The Clarify processing job aggregates the SHAP values of each token, and then selects the top tokens based on their global SHAP values\. The global SHAP values of the selected top tokens are included in the `global_top_shap_text` section of the analysis\.json file\.
      + The local SHAP value of aggregation\.
    + **image\_config** – Required for computer vision explainability\. Include this configuration if you have an input dataset consisting of images and you want to analyze them for explainability in a computer vision problem\.
      + **model\_type** – The type of the model\. Valid values include:
        + `IMAGE_CLASSIFICATION` for an image classification model\.
        + `OBJECT_DETECTION` for an object detection model\.
      + **max\_objects** – Applicable only when model\_type is **OBJECT\_DETECTION**\.The max number of objects, ordered by confidence score, detected by the computer vision model\. Any objects ranked lower than the top max\_objects by confidence score are filtered out\. Defaults to `3`\.
      + **context** – Applicable only when model\_type is **OBJECT\_DETECTION**\. It indicates if the area around the bounding box of the detected object is masked by the baseline image or not\. Valid values are `0` to mask everything, or `1` to mask nothing\. Defaults to 1\.
      + **iou\_threshold** – Applicable only when `model_type` is **OBJECT\_DETECTION**\.The minimum intersection over union \(IOU\) metric for evaluating predictions against the original detection\. A high IOU metric corresponds to a large overlap between the predicted and ground truth detection box\. Defaults to `0.5`\.
      + **num\_segments** – \(Optional\) An integer that determines the approximate number of segments to be labeled in the input image\. Each segment of the image is considered a feature, and local SHAP values are computed for each segment\. Defaults to `20`\.
      + **segment\_compactness** – \(Optional\) An integer that determines the shape and size of the image segments generated by the [scikit\-image slic](https://scikit-image.org/docs/dev/api/skimage.segmentation.html#skimage.segmentation.slic) method\. Defaults to `5`\.
  + **pdp** – Include this method to compute [partial dependence plots](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-partial-dependence-plots.html) \(PDPs\)\.
    + **features** – Mandatory if the `shap` method is not requested\. An array of feature names or indices to compute and plot PDP plots\.
    + **top\_k\_features** – \(Optional\) Specifies the number of top features used to generate PDP plots\. If `features` is not provided, but the `shap` method is requested, then the Clarify processing job chooses the top features based on their SHAP attributions\. Defaults to `10`\.
    + **grid\_resolution** – The number of buckets to divide the range of numeric values into\. This specifies the granularity of the grid for the PDP plots\.
  + **report** – \(Optional\) Use this object to customize the analysis report\. There are three copies of the same report as part of the analysis result: Jupyter Notebook report, HTML report and PDF report\. The object has the following parameters:
    + **name** – File name of the report files\. For example, if `name` is **MyReport**, then the report files are `MyReport.ipynb`, `MyReport.html` and `MyReport.pdf`\. Defaults to `report`\.
    + **title** – \(Optional\) Title string for the report\. Defaults to **SageMaker Analysis Report**\.
+ **predictor** – Required if the analysis requires predictions from the model\. For example, when the `shap`, `pdp`, or `post_training_bias` method is requested, but predicted labels are not provided as part of the input dataset\. The following are parameters to be used in conjunction with `predictor`:
  + **model\_name** – The name of your SageMaker model created by the [CreateModel](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html) API\. If you specify `model_name` instead of endpoint\_name, the Clarify processing job creates an ephemeral endpoint with the model name, known as a **shadow endpoint**, and gets predictions from the endpoint\. The job deletes the shadow endpoint after the computations are completed\. If the model is [multi\-model](https://docs.aws.amazon.com/sagemaker/latest/dg/multi-model-endpoints.html) then the `target_model` parameter must be specified\.
  + **endpoint\_name\_prefix** – \(Optional\) A custom name prefix for the shadow endpoint\. Applicable if you provide `model_name` instead of `endpoint_name`\. For example, provide `endpoint_name_prefix` if you want to restrict access to the endpoint by endpoint name\. The prefix must match the [EndpointName](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html#sagemaker-CreateEndpoint-request-EndpointName) pattern, and its maximum length is `23`\. Defaults to `sm-clarify`\.
  + **initial\_instance\_count** – Specifies the number of instances for the shadow endpoint\. Required if you provide model\_name instead of endpoint\_name\. The value for `initial_instance_count` can be different from the [InstanceCount](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ProcessingClusterConfig.html#sagemaker-Type-ProcessingClusterConfig-InstanceCount) of the job, but we recommend a 1:1 ratio\.
  + **instance\_type** – Specifies the instance type for the shadow endpoint\. Required if you provide `model_name` instead of `endpoint_name`\. As an example, `instance_type` can be set to "ml\.m5\.large"\. In some cases, the value specified for `instance_type` can help reduce model inference time\. For example, to run efficiently, natural language processing models and computer vision models typically require a graphics processing unit \(GPU\) instance type\.
  + **accelerator\_type** – \(Optional\) Specifies [the type of Elastic Inference \(EI\) accelerator](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ProductionVariant.html#sagemaker-Type-ProductionVariant-AcceleratorType) to attach to the shadow endpoint\. Applicable if you provide `model_name` instead of `endpoint_name` for `accelerator_type`\. An example value for `accelerator_type` is **ml\.eia2\.large**\. Defaults to not use an accelerator\.
  + **endpoint\_name** – The name of your SageMaker endpoint created by the [CreateEndpoint](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html) API\. If provided, `endpoint_name` takes precedence over the `model_name` parameter\. Using an existing endpoint reduces the shadow endpoint bootstrap time, but it can also cause a significant increase in load for that endpoint\. Additionally, some analysis methods \(such as `shap` and `pdp`\) generate synthetic datasets that are sent to the endpoint\. This can cause the endpoint's metrics or captured data to be contaminated by synthetic data, which may not accurately reflect real\-world usage\. For these reasons, it's generally not recommended to use an existing production endpoint for Clarify analysis\.
  + **target\_model** – The string value that is passed on to the TargetModel parameter of the SageMaker [InvokeEndpoint](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_runtime_InvokeEndpoint.html#RequestSyntax) API\. Required if your model \(specified by the model\_name parameter\) or endpoint \(specified by the endpoint\_name parameter\) is [multi\-model](https://docs.aws.amazon.com/sagemaker/latest/dg/multi-model-endpoints.html)\.
  + **custom\_attributes** – \(Optional\) A string that allows you to provide additional information about a request for an inference that is submitted to the endpoint\. The string value is passed to the `CustomAttributes` parameter of the SageMaker [InvokeEndpoint](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_runtime_InvokeEndpoint.html#RequestSyntax) API\.
  + **content\_type** – content\_type – The model input format to be used for getting predictions from the endpoint\. If provided, it is passed to the `ContentType` parameter of the SageMaker [InvokeEndpoint](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_runtime_InvokeEndpoint.html#RequestSyntax) API\. 
    + For computer vision explainability, the valid values are **image/jpeg**, **image/png** or **application/x\-npy**\. If `content_type` is not provided, the default value is **image/jpeg**\.
    + For other types of explainability, the valid values are **text/csv** and **application/jsonlines**\. A value for `content_type` is required if the `dataset_type` is "application/x\-parquet"\. Otherwise `content_type` defaults to the value of the `dataset_type` parameter\.
  + **accept\_type** – The model output format to be used for getting predictions from the endpoint\. The value for `accept_type` is passed to the `Accept` parameter of the SageMaker [InvokeEndpoint](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_runtime_InvokeEndpoint.html#RequestSyntax) API\.
    + For computer vision explainability, if `model_type` is "OBJECT\_DETECTION" then `accept_type` defaults to **application/json**\.
    + For other types of explainability, the valid values are **text/csv**, **application/jsonlines** and **application/json**\. If a value for `accept_type` is not provided, `accept_type` defaults to the value of the `content_type` parameter\.
  + **content\_template** – A template string used to construct the model input from dataset records\. Required if the value of the `content_type` parameter is **application/jsonlines**\. The template should have only one placeholder, `$features`, which is replaced by a features list at runtime\. For example, if the template is `"{\"myfeatures\":$features}"`, and if a record has three numeric feature values: `1`, `2` and `3`, then the record will be sent to the model as JSON Line `{"myfeatures":[1,2,3]}`\.
  + **label** – A zero\-based integer index or JMESPath expression string used to extract predicted labels from the model output for bias analysis\. If the model is multi\-class and the `label` parameter extracts all of the predicted labels from the model output, then the following apply\.
    + The `probability` parameter is required to get the corresponding probabilities \(or scores\) from the model output\.
    + The predicted label of the highest score is chosen\.

    The value for `label` depends on the value of the accept\_type parameter as follows\.
    + If `accept_type` is **text/csv**, then `label` is the index of any predicted labels in the model output\.
    + If `accept_type` is **application/jsonlines** or **application/json**, then `label` is a JMESPath expression that's applied to the model output to get the predicted labels\.
  + **label\_headers** – An array of values that the label can take in the dataset\. If bias analysis is requested, then the `probability` parameter is also required to get the corresponding probability values \(scores\) from model output, and the predicted label of the highest score is chosen\. If explainability analysis is requested, the label headers are used to beautify the analysis report\. A value for `label_headers` is required for computer vision explainability\. For example, for a multi\-class classification problem, if the label has three possible values, **bird**, **cat**, and **dog**, then `label_headers` should be set to `["bird","cat","dog"]`\.
  + **probability** – \(Optional\) A zero\-based integer index or a JMESPath expression string used to extract probabilities \(scores\) for explainability analysis, or to choose the predicted label for bias analysis\. The value of `probability` depends on the value of the `accept_type` parameter as follows\.
    + If `accept_type` is **text/csv**, `probability` is the index of the probabilities \(scores\) in the model output\. If `probability` is not provided, the entire model output is taken as the probabilities \(scores\)\.
    + If `accept_type` is JSON data \(either **application/jsonlines** or **application/json**\), `probability` should be a JMESPath expression that is used to extract the probabilities \(scores\) from the model output\.

## Example analysis configuration files<a name="clarify-processing-job-configure-analysis-examples"></a>

The following sections contain example analysis configuration files for data in CSV format, JSON Lines format, and for both natural language processing \(NLP\) and computer vision explainability\.

**Topics**
+ [Analysis configuration for a CSV dataset](#clarify-analysis-configure-csv-example)
+ [Analysis configuration for a JSON Lines dataset](#clarify-analysis-configure-JSONLines-example)
+ [Analysis configuration for natural language processing explainability](#clarify-analysis-configure-nlp-example)
+ [Analysis configuration for computer vision explainability](#clarify-analysis-configure-computer-vision-example)

### Analysis configuration for a CSV dataset<a name="clarify-analysis-configure-csv-example"></a>

The following examples show how to configure bias analysis and explainability analysis for a tabular dataset in CSV format\. In these examples, the incoming dataset has four feature columns, and one binary label column, `Target`\. The contents of the dataset are as follows\. A label value of `1` indicates a positive outcome\. The dataset is provided to the SageMaker Clarify job by the `dataset` processing input\.

```
"Target","Age","Gender","Income","Occupation"
0,25,0,2850,2
1,36,0,6585,0
1,22,1,1759,1
0,48,0,3446,1
...
```

The following sections show how to compute pre\-training and post\-training bias metrics, SHAP values, and partial dependence plots \(PDPs\) showing feature importance for a dataset in CSV format\. 

#### Compute all of the pre\-training bias metrics<a name="clarify-analysis-configure-csv-example-metrics"></a>

This example configuration shows how to measure if the previous sample dataset is favorably biased towards samples with a **Gender** value of `0`\. The follow analysis configuration instructs the Clarify processing job to compute all the pre\-training bias metrics for the dataset\.

```
{
    "dataset_type": "text/csv",
    "label": "Target",
    "label_values_or_threshold": [1],
    "facet": [
        {
            "name_or_index": "Gender",
            "value_or_threshold": [0]
        }
    ],
    "methods": {
        "pre_training_bias": {
            "methods": "all"
        }
    }
}

Com
```

#### Compute all of the post\-training bias metrics<a name="clarify-analysis-configure-csv-example-postmetrics"></a>

You can compute pre\-training bias metrics prior to training\. However, you must have a trained model to compute post\-training bias metrics\. The following example output is from a binary classification model that outputs data in CSV format\. In this example output, each row contains two columns\. The first column contains the predicted label, and the second column contains the probability value for that label\. 

```
0,0.028986845165491
1,0.825382471084594
...
```

The following configuration example instructs the Clarify processing job to compute all possible bias metrics using the dataset and the predictions from the model output\. In the example, the model is deployed to a SageMaker endpoint `your_endpoint`\.

**Note**  
In the following example code, the parameter `content_type` and `accept_type` are not set\. Therefore, they automatically use the value of the parameter dataset\_type, which is "text/csv"\.

```
{
    "dataset_type": "text/csv",
    "label": "Target",
    "label_values_or_threshold": [1],
    "facet": [
        {
            "name_or_index": "Gender",
            "value_or_threshold": [0]
        }
    ],
    "methods": {
        "pre_training_bias": {
            "methods": "all"
        },
        "post_training_bias": {
            "methods": "all"
        }
    },
    "predictor": {
        "endpoint_name": "your_endpoint",
        "label": 0
    }
}
```

#### Compute the SHAP values<a name="clarify-analysis-configure-csv-example-shap"></a>

The following example analysis configuration instructs the job to compute the SHAP values designating the `Target` column as labels and all other columns as features\.

```
{
    "dataset_type": "text/csv",
    "label": "Target",
    "methods": {
        "shap": {
            "num_clusters": 1
        }
    },
    "predictor": {
        "endpoint_name": "your_endpoint",
        "probability": 1
    }
}
```

In this example, the SHAP `baseline` parameter is omitted and the value of the `num_clusters` parameter is `1`\. This instructs the Clarify processor to compute one SHAP baseline sample\. In this example, probability is set to `1`\. This instructs the Clarify processing job to extract the probability score from the second column of the model output \(using zero\-based indexing\)\.

#### Compute partial dependence plots \(PDPs\)<a name="clarify-analysis-configure-csv-example-pdp"></a>

The following example shows how to view the importance of the `Income` feature on the analysis report using PDPs\. The report parameter instructs the Clarify processing job to generate a report\. After the job completes, the generated report is saved as report\.pdf to the `analysis_result` location\. The `grid_resolution` parameter divides the range of the feature values into `10` buckets\. Together, the parameters specified in the following example instruct the Clarify processing job to generate a report containing a PDP graph for `Income` with `10` segments on the x\-axis\. The y\-axis will show the marginal impact of `Income` on the predictions\.

```
{
    "dataset_type": "text/csv",
    "label": "Target",
    "methods": {
        "pdp": {
            "features": ["Income"],
            "grid_resolution": 10
        },
        "report": {
            "name": "report"
        }
    },
    "predictor": {
        "endpoint_name": "your_endpoint",
        "probability": 1
    },
}
```

#### Compute both bias metrics and feature importance<a name="clarify-analysis-configure-csv-example-fi"></a>

 You can combine all the methods from the previous configuration examples into a single analysis configuration file and compute them all by a single job\. The following example shows an analysis configuration with all steps combined\. 

In this example, the `probability` parameter is set to `1` to indicate that probabilities are contained in the second column \(using zero\-based indexing\)\. However, because bias analysis needs a predicted label, the `probability_threshold` parameter is set to `0.5` to convert the probability score into a binary label\. In this example, the `top_k_features` parameter of the pdp method is set to `2`\. This instructs the Clarify processing job to compute PDPs for the top `2` features with the largest global SHAP values\. 

```
{
    "dataset_type": "text/csv",
    "label": "Target",
    "probability_threshold": 0.5,
    "label_values_or_threshold": [1],
    "facet": [
        {
            "name_or_index": "Gender",
            "value_or_threshold": [0]
        }
    ],
    "methods": {
        "pre_training_bias": {
            "methods": "all"
        },
        "post_training_bias": {
            "methods": "all"
        },
        "shap": {
            "num_clusters": 1
        },
        "pdp": {
            "top_k_features": 2,
            "grid_resolution": 10
        },
        "report": {
            "name": "report"
        }
    },
    "predictor": {
        "endpoint_name": "your_endpoint",
        "probability": 1
    }
}
```

Instead of deploying the model to an endpoint, you can provide the name of your SageMaker model to the Clarify processing job using the `model_name` parameter\. The following example shows how to specify a model named **your\_model**\. The Clarify processing job will create a shadow endpoint using the configuration\.

```
{
     ...
    "predictor": {
        "model_name": "your_model",
        "initial_instance_count": 1,
        "instance_type": "ml.m5.large",
        "probability": 1
    }
}
```

### Analysis configuration for a JSON Lines dataset<a name="clarify-analysis-configure-JSONLines-example"></a>

The following examples show how to configure bias analysis and explainability analysis for a tabular dataset in JSON Lines format\. In these examples, the incoming dataset has the same data as the previous section but they are in the [SageMaker JSON Lines dense format](https://docs.aws.amazon.com/sagemaker/latest/dg/cdf-inference.html#cm-jsonlines)\. Each line is a valid JSON object\. The key "Features" points to an array of feature values, and the key "Label" points to the ground truth label\. The dataset is provided to the SageMaker Clarify job by the "dataset" processing input\.

```
{"Features":[25,0,2850,2],"Label":0}
{"Features":[36,0,6585,0],"Label":1}
{"Features":[22,1,1759,1],"Label":1}
{"Features":[48,0,3446,1],"Label":0}
...
```

The following sections show how to compute pre\-training and post\-training bias metrics, SHAP values, and partial dependence plots \(PDPs\) showing feature importance for a dataset in JSON Lines format\.

#### Compute pre\-training bias metrics<a name="clarify-analysis-configure-JSONLines-pretraining"></a>

Specify the label, features, format, and methods to measure pre\-training bias metrics for a `Gender` value of `0`\. In the following example, the `headers` parameter provides the feature names first\. The label name is provided last\. By convention, the last header is the label header\. 

The `features` parameter is set to the JMESPath expression "Features" so that the Clarify processing job can extract the array of features from each record\. The `label` parameter is set to JMESPath expression "Label" so that the Clarify processing job can extract the ground truth label from each record\. Use a facet name to specify the sensitive attribute, as follows\.

```
{
    "dataset_type": "application/jsonlines",
    "headers": ["Age","Gender","Income","Occupation","Target"],
    "label": "Label",
    "features": "Features",
    "label_values_or_threshold": [1],
    "facet": [
        {
            "name_or_index": "Gender",
            "value_or_threshold": [0]
        }
    ],
    "methods": {
        "pre_training_bias": {
            "methods": "all"
        }
    }
}
```

#### Compute all the bias metrics<a name="clarify-analysis-configure-JSONLines-bias"></a>

You must have a trained model to compute post\-training bias metrics\. The following example is from a binary classification model that outputs JSON Lines data in the example's format\. Each row of the model output is a valid JSON object\. The key `predicted_label` points to the predicted label, and the key `probability` points to the probability value\.

```
{"predicted_label":0,"probability":0.028986845165491}
{"predicted_label":1,"probability":0.825382471084594}
...
```

You can deploy the model to a SageMaker endpoint named `your_endpoint`\. The following example analysis configuration instructs the Clarify processing job to compute all possible bias metrics for both the dataset and the model\. In this example, the parameter `content_type` and `accept_type` are not set\. Therefore, they are automatically set to use the value of the parameter dataset\_type, which is `application/jsonlines`\. The Clarify processing job uses the `content_template` parameter to compose the model input, by replacing the `$features` placeholder by an array of features\.

```
{
    "dataset_type": "application/jsonlines",
    "headers": ["Age","Gender","Income","Occupation","Target"],
    "label": "Label",
    "features": "Features",
    "label_values_or_threshold": [1],
    "facet": [
        {
            "name_or_index": "Gender",
            "value_or_threshold": [0]
        }
    ],
    "methods": {
        "pre_training_bias": {
            "methods": "all"
        },
        "post_training_bias": {
            "methods": "all"
        }
    },
    "predictor": {
        "endpoint_name": "your_endpoint",
        "content_template": "{\"Features\":$features}",
        "label": "predicted_label"
    }
}
```

#### Compute the SHAP values<a name="clarify-analysis-configure-JSONLines-shap"></a>

Because SHAP analysis doesn’t need a ground truth label, the `label` parameter is omitted\. In this example, the `headers` parameter is also omitted\. Therefore, the Clarify processing job must generate placeholders using generic names like `column_0` or `column_1` for feature headers, and `label0` for a label header\. You can specify values for `headers` and for a `label` to improve the readability of the analysis result\. Because the probability parameter is set to JMESPath expression "probability", the probability value will be extracted from the model output\. The following is an example to calculate SHAP values\.

```
{
    "dataset_type": "application/jsonlines",
    "features": "Features",
    "methods": {
        "shap": {
            "num_clusters": 1
        }
    },
    "predictor": {
        "endpoint_name": "your_endpoint",
        "content_template": "{\"Features\":$features}",
        "probability": "probability"
    }
}
```

#### Compute partials dependence plots \(PDPs\)<a name="clarify-analysis-configure-JSONLines-pdp"></a>

The following example shows how to view the importance of "Income" on PDP\. In this example, the feature headers are not provided\. Therefore, the `features` parameter of the `pdp` method must use zero\-based index to refer to location of the feature column\. The `grid_resolution` parameter divides the range of the feature values into `10` buckets\. Together, the parameters in the example instruct the Clarify processing job to generate a report containing a PDP graph for `Income` with `10` segments on the x\-axis\. The y\-axis will show the marginal impact of `Income` on the predictions\.

```
{
    "dataset_type": "application/jsonlines",
    "features": "Features",
    "methods": {
        "pdp": {
            "features": [2],
            "grid_resolution": 10
        },
        "report": {
            "name": "report"
        }
    },
    "predictor": {
        "endpoint_name": "your_endpoint",
        "content_template": "{\"Features\":$features}",
        "probability": "probability"
    }
}
```

#### Compute both bias metrics and feature importance<a name="clarify-analysis-configure-JSONLines-fi-metrics"></a>

You can combine all previous methods into a single analysis configuration file and compute them all by a single job\. The following example shows an analysis configuration with all steps combined\. In this example, the `probability` parameter is set\. But because bias analysis needs a predicted label, the `probability_threshold` parameter is set to `0.5` to convert the probability score into a binary label\. In this example, the `top_k_features` parameter of the `pdp` method is set to `2`\. This instructs the Clarify processing job to compute PDPs for the top `2` features with the largest global SHAP values\.

```
{
    "dataset_type": "application/jsonlines",
    "headers": ["Age","Gender","Income","Occupation","Target"],
    "label": "Label",
    "features": "Features",
    "probability_threshold": 0.5,
    "label_values_or_threshold": [1],
    "facet": [
        {
            "name_or_index": "Gender",
            "value_or_threshold": [0]
        }
    ],
    "methods": {
        "pre_training_bias": {
            "methods": "all"
        },
        "post_training_bias": {
            "methods": "all"
        },
        "shap": {
            "num_clusters": 1
        },
        "pdp": {
            "top_k_features": 2,
            "grid_resolution": 10
        },
        "report": {
            "name": "report"
        }
    },
    "predictor": {
        "endpoint_name": "your_endpoint",
        "content_template": "{\"Features\":$features}",
        "probability": "probability"
    }
}
```

### Analysis configuration for natural language processing explainability<a name="clarify-analysis-configure-nlp-example"></a>

The following example shows an analysis configuration file for computing feature importance for natural language processing \(NLP\)\. In this example, the incoming dataset is a tabular dataset in CSV format, with one binary label column and two feature columns, as follows\. The dataset is provided to the SageMaker Clarify job by the `dataset` processing input parameter\.

```
0,2,"They taste gross"
1,3,"Flavor needs work"
1,5,"Taste is awful"
0,1,"The worst"
...
```

In this example, a binary classification model was trained on the previous dataset\. The model accepts CSV data, and it outputs a single score in between `0` and `1`, as follows\.

```
0.491656005382537
0.569582343101501
...
```

The model is used to create a SageMaker model named “your\_model"\. The following analysis configuration shows how to run a token\-wise explainability analysis using the model and dataset\. The `text_config` parameter activates the NLP explainability analysis\. The `granularity` parameter indicates that the analysis should parse tokens\. 

In English, each token is a word\. The following example also shows how to provide an in\-place SHAP "baseline" instance using an average "Rating" of 4\. A special mask token "\[MASK\]" is used to replace a token \(word\) in "Comments"\. This example also uses a GPU endpoint instance type to speed up inferencing\.

```
{
    "dataset_type": "text/csv",
    "headers": ["Target","Rating","Comments"]
    "label": "Target",
    "methods": {
        "shap": {
            "text_config": {
                "granularity": "token",
                "language": "english"
            }
            "baseline": [[4,"[MASK]"]],
        }
    },
    "predictor": {
        "model_name": "your_nlp_model",
        "initial_instance_count": 1,
        "instance_type": "ml.g4dn.xlarge"
    }
}
```

### Analysis configuration for computer vision explainability<a name="clarify-analysis-configure-computer-vision-example"></a>

The following example shows an analysis configuration file computing feature importance for computer vision\. In this example, the input dataset consists of JPEG images\. The dataset is provided to the SageMaker Clarify job by the `dataset` processing input parameter\. The example shows how to configure an explainability analysis using a [SageMaker image classification model](https://docs.aws.amazon.com/agemaker/latest/dg/image-classification.html)\. In the example, a model named `your_cv_ic_model`, has been trained to classify the animals on the input JPEG images\.

```
{
    "dataset_type": "application/x-image",
    "methods": {
        "shap": {
             "image_config": {
                "model_type": "IMAGE_CLASSIFICATION",
                 "num_segments": 20,
                "segment_compactness": 10
             }
        },
        "report": {
            "name": "report"
        }
    },
    "predictor": {
        "model_name": "your_cv_ic_model",
        "initial_instance_count": 1,
        "instance_type": "ml.p2.xlarge",
        "label_headers": ["bird","cat","dog"]
    }
}
```

In this example, a [SageMaker object detection model](https://docs.aws.amazon.com/sagemaker/latest/dg/object-detection.html), `your_cv_od_model` is trained on the same JPEG images to identify the animals on them\. The following example shows how to configure an explainability analysis for the object detection model\.

```
{
    "dataset_type": "application/x-image",
    "probability_threshold": 0.5,
    "methods": {
        "shap": {
             "image_config": {
                "model_type": "OBJECT_DETECTION",
                 "max_objects": 3,
                "context": 1.0,
                "iou_threshold": 0.5,
                 "num_segments": 20,
                "segment_compactness": 10
             }
        },
        "report": {
            "name": "report"
        }
    },
    "predictor": {
        "model_name": "your_cv_od_model",
        "initial_instance_count": 1,
        "instance_type": "ml.p2.xlarge",
        "label_headers": ["bird","cat","dog"]
    }
}
```