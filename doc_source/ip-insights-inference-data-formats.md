# IP Insights Inference Data Formats<a name="ip-insights-inference-data-formats"></a>

The following are the available input and output formats for the IP Insights algorithm\. Amazon SageMaker built\-in algorithms adhere to the common input inference format described in [Common Data Formats for Inference](cdf-inference.md)\. However, the SageMaker IP Insights algorithm does not currently support RecordIO format\.

## IP Insights Input Request Formats<a name="ip-insights-input-format-requests"></a>

### INPUT: CSV Format<a name="ip-insights-input-csv"></a>

The CSV file must have two columns\. The first column is an opaque string that corresponds to an entity's unique identifier\. The second column is the IPv4 address of the entity's access event in decimal\-dot notation\. 

content\-type: text/csv

```
entity_id_1, 192.168.1.2
entity_id_2, 10.10.1.2
```

### INPUT: JSON Format<a name="ip-insights-input-json"></a>

JSON data can be provided in different formats\. IP Insights follows the common SageMaker formats\. For more information about inference formats, see [Common Data Formats for Inference](cdf-inference.md)\.

content\-type: application/json

```
{
  "instances": [
    {"data": {"features": {"values": ["entity_id_1", "192.168.1.2"]}}},
    {"features": ["entity_id_2", "10.10.1.2"]}
  ]
}
```

### INPUT: JSONLINES Format<a name="ip-insights-input-jsonlines"></a>

The JSON Lines content type is useful for running batch transform jobs\. For more information on SageMaker inference formats, see [Common Data Formats for Inference](cdf-inference.md)\. For more information on running batch transform jobs, see [Get Inferences for an Entire Dataset with Batch Transform](how-it-works-batch.md)\.

content\-type: application/jsonlines

```
{"data": {"features": {"values": ["entity_id_1", "192.168.1.2"]}}},
{"features": ["entity_id_2", "10.10.1.2"]}]
```

## IP Insights Output Response Formats<a name="ip-insights-ouput-format-response"></a>

### OUTPUT: JSON Response Format<a name="ip-insights-output-json"></a>

The default output of the SageMaker IP Insights algorithm is the `dot_product` between the input entity and IP address\. The dot\_product signifies how compatible the model considers the entity and IP address\. The `dot_product` is unbounded\. To make predictions about whether an event is anomalous, you need to set a threshold based on your defined distribution\. For information about how to use the `dot_product` for anomaly detection, see the [An Introduction to the SageMakerIP Insights Algorithm](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/ipinsights_login/ipinsights-tutorial.ipynb                  )\.

accept: application/json

```
{
  "predictions": [
    {"dot_product": 0.0},
    {"dot_product": 2.0}
  ]
}
```

Advanced users can access the model's learned entity and IP embeddings by providing the additional content\-type parameter `verbose=True` to the Accept heading\. You can use the `entity_embedding` and `ip_embedding` for debugging, visualizing, and understanding the model\. Additionally, you can use these embeddings in other machine learning techniques, such as classification or clustering\.

accept: application/json;verbose=True

```
{
  "predictions": [
    {
        "dot_product": 0.0,
        "entity_embedding": [1.0, 0.0, 0.0],
        "ip_embedding": [0.0, 1.0, 0.0]
    },
    {
        "dot_product": 2.0,
        "entity_embedding": [1.0, 0.0, 1.0],
        "ip_embedding": [1.0, 0.0, 1.0]
    }
  ]
}
```

### OUTPUT: JSONLINES Response Format<a name="ip-insights-jsonlines"></a>

accept: application/jsonlines 

```
{"dot_product": 0.0}
{"dot_product": 2.0}
```

accept: application/jsonlines; verbose=True 

```
{"dot_product": 0.0, "entity_embedding": [1.0, 0.0, 0.0], "ip_embedding": [0.0, 1.0, 0.0]}
{"dot_product": 2.0, "entity_embedding": [1.0, 0.0, 1.0], "ip_embedding": [1.0, 0.0, 1.0]}
```