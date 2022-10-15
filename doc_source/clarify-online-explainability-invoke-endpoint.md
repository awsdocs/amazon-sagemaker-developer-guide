# Invoke the endpoint<a name="clarify-online-explainability-invoke-endpoint"></a>

After the endpoint is running, use the SageMaker Runtime [InvokeEndpoint ](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_runtime_InvokeEndpoint.html) API in the SageMaker Runtime service to send requests to, or invoke the endpoint\. In response, the requests are handled as explainability requests by the SageMaker Clarify explainer\.

**Note**  
To invoke an endpoint, choose one of the following options:  
For instructions to use Boto3 or the AWS CLI to invoke an endpoint, see [ real\-time endpoints](https://docs.aws.amazon.com/sagemaker/latest/dg/realtime-endpoints-test-endpoints.html)\.
To use the SageMaker SDK for Python to invoke an endpoint, see the [Predictor](https://sagemaker.readthedocs.io/en/stable/api/inference/predictors.html) API\.

## Request<a name="clarify-online-explainability-request"></a>

The `InvokeEndpoint` API has an optional parameter `EnableExplanations`, which is mapped to the HTTP header `X-Amzn-SageMaker-Enable-Explanations`\. If this parameter is provided, it overrides the `EnableExplanations` parameter of the `ClarifyExplainerConfig`\.

**Note**  
The `ContentType` and `Accept` parameters of the `InvokeEndpoint` API are required\. Supported formats include MIME type `text/csv` and `application/jsonlines`\.

Use the `sagemaker_runtime_client` to send a request to the endpoint, as follows:

```
response = sagemaker_runtime_client.invoke_endpoint(
    EndpointName='name-of-your-endpoint',
    EnableExplanations='`true`',
    ContentType='text/csv',
    Accept='text/csv',
    Body='1,2,3,4',  # single record (of four numerical features)
)
```

## Response<a name="clarify-online-explainability-response"></a>

If the endpoint is created with `ExplainerConfig`, then a new response schema is used, This new schema is different from, and is not compatible with, an endpoint that lacks the `ExplainerConfig` parameter provided\.

The MIME type of the response is `"application/json"`, and the response payload can be decoded from UTF\-8 bytes to a JSON object\. The following shows the members of this JSON object are as follows:
+ `version`: The version of the response schema in string format\. For example, `"1.0"`\.
+ `predictions`: The predictions that the request makes have the following:
  + `content_type`: The MIME type of the predictions, referring to the `ContentType` of the model container response\.
  + `data`: The predictions data string delivered as the payload of the model container response for the request\.
+ `label_headers`: The label headers from the `LabelHeaders` parameter\. This is provided in either the explainer configuration or the model container output\.
+ `explanations`: The explanations provided in the request payload\. If no records are explained, then this member returns the empty object `{}`\.
+ 
  + `kernel_shap`: A key that refers to an array of Kernel SHAP explanations for each record in the request\. If a record is not explained, the corresponding explanation is `null`\.

The `kernel_shap` element has the following members:
+ `"feature_header"`: The header name of the features provided by the `FeatureHeaders` parameter in the explainer configuration `ExplainerConfig`\.
+ `feature_type`: The feature type inferred by explainer or provided in the `FeatureTypes` parameter in the `ExplainerConfig`\. This element is only available for NLP explainability problems\.
+ `attributions`: An array of attribution objects\. Text features can have multiple attribution objects, each for a unit\. The attribution object has the following members:
  + `attribution`: A list of probability values, given for each class\.
  + `description`: The description of the text units, available only for NLP explainability problems\.
    + `partial_text`: The portion of the text explained by the explainer\.
    + `start_idx`: A zero\-based index to identify the array location of the beginning of the partial text fragment\.