# Code examples: SDK for Python<a name="clarify-online-explainability-examples"></a>

This section provides sample code to create and invoke an endpoint that uses SageMaker Clarify online explainability\. These code examples use the [AWS SDK for Python\.](https://aws.amazon.com/sdk-for-python/)

## Tabular data<a name="clarigy-online-explainability-examples-tabular"></a>

The following example uses tabular data and a SageMaker model called `model_name`\. In this example, the model container accepts data in CSV format, and each record has four numerical features\. In this minimal configuration, **for demonstration purposes only**, the SHAP baseline data is set to zero\. Refer to the [ SHAP Baselines for Explainability](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-feature-attribute-shap-baselines.html) to choose more appropriate values for `ShapBaseline`\.

Configure the endpoint, as follows:

```
endpoint_config_name = 'tabular_explainer_endpoint_config'
response = sagemaker_client.create_endpoint_config(
    EndpointConfigName=endpoint_config_name,
    ProductionVariants=[{
        'VariantName': 'AllTraffic',
        'ModelName': model_name,
        'InitialInstanceCount': 1,
        'InstanceType': 'ml.m5.xlarge',
    }],
    ExplainerConfig={
        'ClarifyExplainerConfig': {
            'ShapConfig': {
                'ShapBaselineConfig': {
                    'ShapBaseline': '0,0,0,0',
                },
            },
        },
    },
)
```

Use the endpoint configuration to create an endpoint, as follows:

```
endpoint_name = 'tabular_explainer_endpoint'
response = sagemaker_client.create_endpoint(
    EndpointName=endpoint_name,
    EndpointConfigName=endpoint_config_name,
)
```

Use the `DescribeEndpoint` API to inspect the progress of creating an endpoint, as follows:

```
response = sagemaker_client.describe_endpoint(
    EndpointName=endpoint_name,
)
response['EndpointStatus']
```

After the endpoint status is "InService", invoke the endpoint with a test record, as follows:

```
response = sagemaker_runtime_client.invoke_endpoint(
    EndpointName=endpoint_name,
    ContentType='text/csv',
    Accept='text/csv',
    Body='1,2,3,4',
)
```

Assume that the response has a status code of 200 \(no error\), and load the response body, as follows:

```
import codecs
import json
json.load(codecs.getreader('utf-8')(response['Body']))
```

The default action for the endpoint is to explain the record\. The following shows example output in the returned JSON object\.

```
{
    "version": "1.0",
    "predictions": {
        "content_type": "text/csv; charset=utf-8",
        "data": "0.0006380207487381"
    },
    "explanations": {
        "kernel_shap": [
            [
                {
                    "attributions": [
                        {
                            "attribution": [-0.00433456]
                        }
                    ]
                },
                {
                    "attributions": [
                        {
                            "attribution": [-0.005369821]
                        }
                    ]
                },
                {
                    "attributions": [
                        {
                            "attribution": [0.007917749]
                        }
                    ]
                },
                {
                    "attributions": [
                        {
                            "attribution": [-0.00261214]
                        }
                    ]
                }
            ]
        ]
    }
}
```

Use the `EnableExplanations` parameter to enable on\-demand explanations, as follows:

```
response = sagemaker_runtime_client.invoke_endpoint(
    EndpointName=endpoint_name,
    ContentType='text/csv',
    Accept='text/csv',
    Body='1,2,3,4',
    EnableExplanations='[0]>`0.8`',
)
```

In this example, the prediction value is less than the threshold value of 0\.8, so the record is not explained:

```
{
    "version": "1.0",
    "predictions": {
        "content_type": "text/csv; charset=utf-8",
        "data": "0.6380207487381995"
    },
    "explanations": {}
}
```

Use visualization tools to help interpret the returned explanations\. The following image shows how SHAP plots can be used to understand how each feature contributes to the prediction\. The base value on the diagram, also called the expected value, is the mean predictions of the training dataset\. Features that push the expected value higher are red, and features that push the expected value lower are blue\. See [SHAP additive force layout](https://shap.readthedocs.io/en/latest/generated/shap.plots.force.html) for additional information\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/clarify/force-plot.png)

See the [full example notebook for tabular data](https://github.com/aws/amazon-sagemaker-examples/blob/main/sagemaker-clarify/online_explainability/tabular/tabular_online_explainability_with_sagemaker_clarify.ipynb)\. 

## Text data<a name="clarigy-online-explainability-examples-text"></a>

This section provides a code example to create and invoke an online explainability endpoint for text data\. The code example uses SDK for Python\.

The following example uses text data and a SageMaker model called `model_name`\. In this example, the model container accepts data in CSV format, and each record is a single string\.

```
endpoint_config_name = 'text_explainer_endpoint_config'
response = sagemaker_client.create_endpoint_config(
    EndpointConfigName=endpoint_config_name,
    ProductionVariants=[{
        'VariantName': 'AllTraffic',
        'ModelName': model_name,
        'InitialInstanceCount': 1,
        'InstanceType': 'ml.m5.xlarge',
    }],
    ExplainerConfig={
        'ClarifyExplainerConfig': {
            'InferenceConfig': {
                'FeatureTypes': ['text'],
                'MaxRecordCount': 100,
            },
            'ShapConfig': {
                'ShapBaselineConfig': {
                    'ShapBaseline': '"<MASK>"',
                },
                'TextConfig': {
                    'Granularity': 'token',
                    'Language': 'en',
                },
                'NumberOfSamples': 100,
            },
        },
    },
)
```
+ `ShapBaseline`: A special token reserved for natural language processing \(NLP\) processing\.
+ `FeatureTypes`: Identifies the feature as text\. If this parameter is not provided, the explainer will attempt to infer the feature type\.
+ `TextConfig`: Specifies the unit of granularity and language for the analysis of text features\. In this example, the language is English, and granularity `'token'` means a word in English text\.
+ `NumberOfSamples`: A limit to set the upper bounds of the size of the synthetic dataset\.
+ `MaxRecordCount`: The maximum number of records in a request that the model container can handle\. This parameter is set to stabilize performance\.

Use the endpoint configuration to create the endpoint, as follows:

```
endpoint_name = 'text_explainer_endpoint'
response = sagemaker_client.create_endpoint(
    EndpointName=endpoint_name,
    EndpointConfigName=endpoint_config_name,
)
```

After the status of the endpoint becomes `InService`, invoke the endpoint\. The following code sample uses a test record as follows:

```
response = sagemaker_runtime_client.invoke_endpoint(
    EndpointName=endpoint_name,
    ContentType='text/csv',
    Accept='text/csv',
    Body='"This is a good product"',
)
```

If the request completes successfully, the response body will return a valid JSON object that's similar to the following:

```
{
    "version": "1.0",
    "predictions": {
        "content_type": "text/csv",
        "data": "0.9766594\n"
    },
    "explanations": {
        "kernel_shap": [
            [
                {
                    "attributions": [
                        {
                            "attribution": [
                                -0.007270948666666712
                            ],
                            "description": {
                                "partial_text": "This",
                                "start_idx": 0
                            }
                        },
                        {
                            "attribution": [
                                -0.018199033666666628
                            ],
                            "description": {
                                "partial_text": "is",
                                "start_idx": 5
                            }
                        },
                        {
                            "attribution": [
                                0.01970993241666666
                            ],
                            "description": {
                                "partial_text": "a",
                                "start_idx": 8
                            }
                        },
                        {
                            "attribution": [
                                0.1253469515833334
                            ],
                            "description": {
                                "partial_text": "good",
                                "start_idx": 10
                            }
                        },
                        {
                            "attribution": [
                                0.03291143366666657
                            ],
                            "description": {
                                "partial_text": "product",
                                "start_idx": 15
                            }
                        }
                    ],
                    "feature_type": "text"
                }
            ]
        ]
    }
}
```

Use visualization tools to help interpret the returned text attributions\. The following image shows how the captum visualization utility can be used to understand how each word contributes to the prediction\. The higher the color saturation, the higher the importance given to the word\. In this example, a highly saturated bright red color indicates a strong negative contribution\. A highly saturated green color indicates a strong positive contribution\. The color white indicates that the word has a neutral contribution\. See the [captum](https://github.com/pytorch/captum) library for additional information on parsing and rendering the attributions\.

![\[Captum visualization utility used to understand how each word contributes to the prediction.\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/clarify/word-importance.png)

See the [full example notebook for text](https://github.com/aws/amazon-sagemaker-examples/blob/main/sagemaker-clarify/online_explainability/natural_language_processing/nlp_online_explainability_with_sagemaker_clarify.ipynb) data\. 