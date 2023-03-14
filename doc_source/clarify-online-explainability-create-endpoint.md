# Configure and create an endpoint<a name="clarify-online-explainability-create-endpoint"></a>

Create a new endpoint configuration to fit your model, and use this configuration to create the endpoint\. You can use the model container validated in the [pre\-check step ](https://docs.aws.amazon.com/sagemaker/latest/dg/clarify-online-explainability-precheck.html) to create an endpoint and enable the SageMaker Clarify online explainability feature\.

Use the `sagemaker_client` object to create an endpoint using the [CreateEndpointConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpointConfig.html) API\. Set the member `ClarifyExplainerConfig` inside the `ExplainerConfig` parameter as follows:

```
sagemaker_client.create_endpoint_config(
    EndpointConfigName='name-of-your-endpoint-config',
    ExplainerConfig={
        'ClarifyExplainerConfig': {
            'EnableExplanations': '`true`',
            'InferenceConfig': {
                ...
            },
            'ShapConfig': {
                ...
            }
        },
    },
    ProductionVariants=[{
        'VariantName': 'AllTraffic',
        'ModelName': 'name-of-your-model',
        'InitialInstanceCount': 1,
        'InstanceType': 'ml.m5.xlarge',
    }]
     ...
)
sagemaker_client.create_endpoint(
    EndpointName='name-of-your-endpoint',
    EndpointConfigName='name-of-your-endpoint-config'
)
```

The first call to the `sagemaker_client` object creates a new endpoint configuration with the explainability feature enabled\. The second call uses the endpoint configuration to launch the endpoint\.

## The `EnableExplanations` expression<a name="clarify-online-explainability-create-endpoint-enable"></a>

The `EnableExplanations` parameter is a [https://jmespath.org/](https://jmespath.org/) Boolean expression string\. It is evaluated for **each record** in the explainability request\. If this parameter is evaluated to be **true**, then the record will be explained\. If this parameter is evaluated to be **false**, then explanations are not be generated\.

SageMaker Clarify deserializes the model container output for each record into a JSON compatible data structure, and then uses the `EnableExplanations` parameter to evaluate the data\.

**Notes**  
There are two options for records depending on the format of the model container output\.  
If the model container output is in CSV format, then a record is loaded as a JSON array\.
If the model container output is in JSON Lines format, then a record is loaded as a JSON object\.

The `EnableExplanations` parameter is a JMESPath expression that can be passed either during the `InvokeEndpoint` or `CreateEndpointConfig` operations\. If the JMESPath expression that you supplied is not valid, the endpoint creation will fail\. If the expression is valid, but the expression evaluation result is unexpected, then the endpoint will be created successfully, but an error will be generated when the endpoint is invoked\. Test your `EnableExplanations` expression by using the `InvokeEndpoint` API, and then apply it to the endpoint configuration\.

The following are some examples of valid `EnableExplanations` expression\. In the examples, a JMESPath expression encloses a literal using backtick characters\. For example, ``true`` means true\.


| Expression \(string representation\) | Model container output \(string representation\) | Evaluation result \(Boolean\) | Meaning | 
| --- | --- | --- | --- | 
|  '`true`'  |  \(N/A\)  |  True  |  Activate online explainability unconditionally\.  | 
|  '`false`'  |  \(N/A\)  |  False  |  Deactivate online explainability unconditionally\.  | 
|  '\[1\]>`0\.5`'  |  '1,0\.6'  |  True  |  For each record, the model container outputs its predicted label and probability\. Explains a record if its probability \(at index 1\) is greater than 0\.5\.  | 
|  'probability>`0\.5`'  |  '\{"predicted\_label":1,"probability":0\.6\}'  |  True  |  For each record, the model container outputs JSON data\. Explain a record if its probability is greater than 0\.5\.  | 
|  '\!contains\(probabilities\[:\-1\], max\(probabilities\)\)'  |  '\{"probabilities": \[0\.4, 0\.1, 0\.4\], "labels":\["cat","dog","fish"\]\}'  |  False  |  For a multi\-class model: Explains a record if its predicted label \(the class that has the max probability value\) is the last class\. Literally, the expression means that the max probability value is not in the list of probabilities excluding the last one\.  | 

## Synthetic dataset<a name="clarify-online-explainability-create-endpoint-synthetic"></a>

SageMaker Clarify uses the Kernel SHAP algorithm\. Given a record \(also called a sample or an instance\) and the SHAP configuration, the explainer first generates a synthetic dataset\. SageMaker Clarify then queries the model container for the predictions of the dataset, and then computes and returns the feature attributions\. The size of the synthetic dataset affects the runtime for the Clarify explainer\. Larger synthetic datasets take more time to obtain model predictions than smaller ones\.

 The synthetic dataset size is determined by the following formula:

```
Synthetic dataset size = SHAP baseline size * n_samples
```

The SHAP baseline size is the number of records in the SHAP baseline data\. This information is taken from the `ShapBaselineConfig`\.

The size of `n_samples` is set by the parameter `NumberOfSamples` in the explainer configuration and the number of features\. If the number of features is `n_features`, then `n_samples` is the following: 

```
n_samples = MIN(NumberOfSamples, 2^n_features - 2)
```

The following shows `n_samples` if `NumberOfSamples` is not provided\.

```
n_samples = MIN(2*n_features + 2^11, 2^n_features - 2)
```

For example, a tabular record with 10 features has a SHAP baseline size of 1\. If `NumberOfSamples` is not provided, the synthetic dataset contains 1022 records\. If the record has 20 features, the synthetic dataset contains 2088 records\.

For NLP problems, `n_features` is equal to the number of non\-text features plus the number of text units\.

**Note**  
The `InvokeEndpoint` API has a request timeout limit\. If the synthetic dataset is too large, the explainer may not be able to complete the computation within this limit\. If necessary, use the previous information to understand and reduce the SHAP baseline size and `NumberOfSamples`\. If your model container is set up to handle batch requests, then you can also adjust the value of `MaxRecordCount`\.