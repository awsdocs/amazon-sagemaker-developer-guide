# IP Insights Algorithm<a name="ip-insights"></a>

Amazon SageMaker IP Insights is an unsupervised learning algorithm that learns the usage patterns for IPv4 addresses\. It is designed to capture associations between IPv4 addresses and various entities, such as user IDs or account numbers\. You can use it to identify a user attempting to log into a web service from an anomalous IP address, for example\. Or you can use it to identify an account that is attempting to create computing resources from an unusual IP address\. Trained IP Insight models can be hosted at an endpoint for making real\-time predictions or used for processing batch transforms\.

SageMaker IP insights ingests historical data as \(entity, IPv4 Address\) pairs and learns the IP usage patterns of each entity\. When queried with an \(entity, IPv4 Address\) event, a SageMaker IP Insights model returns a score that infers how anomalous the pattern of the event is\. For example, when a user attempts to log in from an IP address, if the IP Insights score is high enough, a web login server might decide to trigger a multi\-factor authentication system\. In more advanced solutions, you can feed the IP Insights score into another machine learning model\. For example, you can combine the IP Insight score with other features to rank the findings of another security system, such as those from [Amazon GuardDuty](https://docs.aws.amazon.com/guardduty/latest/ug/what-is-guardduty.html)\.

The SageMaker IP Insights algorithm can also learn vector representations of IP addresses, known as *embeddings*\. You can use vector\-encoded embeddings as features in downstream machine learning tasks that use the information observed in the IP addresses\. For example, you can use them in tasks such as measuring similarities between IP addresses in clustering and visualization tasks\.

**Topics**
+ [Input/Output Interface for the IP Insights Algorithm](#ip-insights-inputoutput)
+ [EC2 Instance Recommendation for the IP Insights Algorithm](#ip-insights-instances)
+ [IP Insights Sample Notebooks](#ip-insights-sample-notebooks)
+ [How IP Insights Works](ip-insights-howitworks.md)
+ [IP Insights Hyperparameters](ip-insights-hyperparameters.md)
+ [Tune an IP Insights Model](ip-insights-tuning.md)
+ [IP Insights Data Formats](ip-insights-data-formats.md)

## Input/Output Interface for the IP Insights Algorithm<a name="ip-insights-inputoutput"></a>

**Training and Validation**

The SageMaker IP Insights algorithm supports training and validation data channels\. It uses the optional validation channel to compute an area\-under\-curve \(AUC\) score on a predefined negative sampling strategy\. The AUC metric validates how well the model discriminates between positive and negative samples\. Training and validation data content types need to be in `text/csv` format\. The first column of the CSV data is an opaque string that provides a unique identifier for the entity\. The second column is an IPv4 address in decimal\-dot notation\. IP Insights currently supports only File mode\. For more information and some examples, see [IP Insights Training Data Formats](ip-insights-training-data-formats.md)\.

**Inference**

For inference, IP Insights supports `text/csv`, `application/json`, and `application/jsonlines` data content types\. For more information about the common data formats for inference provided by SageMaker, see [Common Data Formats for Inference](cdf-inference.md)\. IP Insights inference returns output formatted as either `application/json` or `application/jsonlines`\. Each record in the output data contains the corresponding `dot_product` \(or compatibility score\) for each input data point\. For more information and some examples, see [IP Insights Inference Data Formats](ip-insights-inference-data-formats.md)\.

## EC2 Instance Recommendation for the IP Insights Algorithm<a name="ip-insights-instances"></a>

The SageMaker IP Insights algorithm can run on both GPU and CPU instances\. For training jobs, we recommend using GPU instances\. However, for certain workloads with large training datasets, distributed CPU instances might reduce training costs\. For inference, we recommend using CPU instances\.

### GPU Instances for the IP Insights Algorithm<a name="ip-insights-instances-gpu"></a>

IP Insights supports all available GPUs\. If you need to speed up training, we recommend starting with a single GPU instance, such as ml\.p3\.2xlarge, and then moving to a multi\-GPU environment, such as ml\.p3\.8xlarge and ml\.p3\.16xlarge\. Multi\-GPUs automatically divide the mini batches of training data across themselves\. If you switch from a single GPU to multiple GPUs, the `mini_batch_size` is divided equally into the number of GPUs used\. You may want to increase the value of the `mini_batch_size` to compensate for this\.

### CPU Instances for the IP Insights Algorithm<a name="ip-insights-instances-cpu"></a>

The type of CPU instance that we recommend depends largely on the instance's available memory and the model size\. The model size is determined by two hyperparameters: `vector_dim` and `num_entity_vectors`\. The maximum supported model size is 8 GB\. The following table lists typical EC2 instance types that you would deploy based on these input parameters for various model sizes\. In Table 1, the value for `vector_dim` in the first column range from 32 to 2048 and the values for `num_entity_vectors` in the first row range from 10,000 to 50,000,000\.


| `vector_dim` \\ `num_entity_vectors`\. | 10,000 | 50,000 | 100,000 | 500,000 | 1,000,000 | 5,000,000 | 10,000,000 | 50,000,000 | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 32 |  ml\.m5\.large  | ml\.m5\.large | ml\.m5\.large | ml\.m5\.large | ml\.m5\.large | ml\.m5\.xlarge | ml\.m5\.2xlarge | ml\.m5\.4xlarge | 
|  `64`  |  ml\.m5\.large  | ml\.m5\.large | ml\.m5\.large | ml\.m5\.large | ml\.m5\.large | ml\.m5\.2xlarge | ml\.m5\.2xlarge |  | 
|  `128`  |  ml\.m5\.large  | ml\.m5\.large | ml\.m5\.large | ml\.m5\.large | ml\.m5\.large | ml\.m5\.2xlarge | ml\.m5\.4xlarge |  | 
|  `256`  |  ml\.m5\.large  | ml\.m5\.large | ml\.m5\.large | ml\.m5\.large | ml\.m5\.xlarge | ml\.m5\.4xlarge |  |  | 
|  `512`  |  ml\.m5\.large  | ml\.m5\.large | ml\.m5\.large | ml\.m5\.large | ml\.m5\.2xlarge |  |  |  | 
|  `1024`  |  ml\.m5\.large  | ml\.m5\.large | ml\.m5\.large | ml\.m5\.xlarge | ml\.m5\.4xlarge |  |  |  | 
|  `2048`  |  ml\.m5\.large  | ml\.m5\.large | ml\.m5\.xlarge | ml\.m5\.xlarge |  |  |  |  | 

The values for the `mini_batch_size`, `num_ip_encoder_layers`, `random_negative_sampling_rate`, and `shuffled_negative_sampling_rate` hyperparameters also affect the amount of memory required\. If these values are large, you might need to use a larger instance type than normal\.

## IP Insights Sample Notebooks<a name="ip-insights-sample-notebooks"></a>

For a sample notebook that shows how to train the SageMaker IP Insights algorithm and perform inferences with it, see [An Introduction to the SageMakerIP Insights Algorithm ](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/ipinsights_login/ipinsights-tutorial.ipynb                 )\. For instructions how to create and access Jupyter notebook instances that you can use to run the example in SageMaker, see [Use Amazon SageMaker Notebook Instances](nbi.md)\. After creating a notebook instance, choose the **SageMaker Examples** tab to see a list of all the SageMaker examples\. To open a notebook, choose its **Use** tab and choose **Create copy**\.