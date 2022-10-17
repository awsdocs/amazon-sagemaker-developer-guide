# Pre\-check the model container<a name="clarify-online-explainability-precheck"></a>

This section shows how to pre\-check the model container inputs and outputs for compatibility before configuring an endpoint\. The SageMaker Clarify explainer is **model agnostic**, but it has requirements for model container input and output\.

**Note**  
You can increase efficiency by configuring your container to support batch requests, which support two or more records in a single request\. A record is a single line of CSV data\.

## Model container input<a name="clarify-online-explainability-input"></a>

The model container supports input in either CSV \(MIME type:"text/csv"\)\. The following table shows example inputs that SageMaker Clarify supports:


| Model container input \(string representation\) | Comments | 
| --- | --- | 
|  '1,2,3,4'  |  Single record \(four numerical features\)\.  | 
|  '1,2,3,4\\n5,6,7,8'  |  Two records, separated by line break '\\n'\.  | 
|  '"This is a good product",5'  |  Single record \(a text feature and a numerical feature\)  | 
|  ‘"This is a good product",5\\n"Bad shopping experience",1'  |  Two records  | 

SageMaker also supports input in [ JSON Lines dense format](https://docs.aws.amazon.com/sagemaker/latest/dg/cdf-inference.html#cm-jsonlines) \(MIME type:"application/jsonlines"\): 


| Model container input | Comments | 
| --- | --- | 
|  '\{"data":\{"features":\[1,2,3,4\]\}\}'  |  Single record, a list of features can be extracted by JMESPath expression 'data\.features'\.  | 
|  '\{"data":\{"features":\[1,2,3,4\]\}\}\\n\{"data":\{"features":\[5,6,7,8\]\}\}'  |  Two records  | 
|  '\{"features":\["This is a good product",5\]\}'  |  Single record, a list of features can be extracted by JMESPath expression 'features'\.  | 
|  '\{"features":\["This is a good product",5\]\}\\n\{"features":\["Bad shopping experience",1\]\}'  |  Two records  | 

## Model container output<a name="clarify-online-explainability-output"></a>

Like the input, your model container output should also be in either CSV or JSON Lines dense format\. Additionally the model container should include the probabilities of the input records, which SageMaker Clarify requires to compute feature attributions\.

The following data examples are for model container outputs in **CSV format**\.

------
#### [ Probability only ]

For regression and binary classification problems, the model container outputs a single probability value \(score\) of the predicted label\. These probabilities can be extracted using column index 0\. For multi\-class problems, the model container outputs a list of probabilities \(scores\)\. For multi\-class problems, if no index is provided, all values are extracted\.


| Model container input | Model container output \(string representation\) | 
| --- | --- | 
|  Single record  |  '0\.6'  | 
|  Two records \(results in one line\)  |  '0\.6,0\.3'  | 
|  Two records \(results in two lines\)  |  '0\.6\\n0\.3'  | 
|  Single record of a multi\-class model \(three classes\)  |  '0\.1,0\.6,0\.3'  | 
|  Two records of a multi\-class model \(three classes\)  |  '0\.1,0\.6,0\.3\\n0\.2,0\.5,0\.3'  | 

------
#### [ Predicted label and probabilities ]

The model container outputs the predicted label followed by its probability in **CSV** format\. The probabilities can be extracted using index `1`\.


| Model container input | Model container output | 
| --- | --- | 
|  Single record  |  '1,0\.6'  | 
|  Two records  |  '1,0\.6\\n0,0\.3'  | 

------
#### [ Predicted labels header and probabilities ]

A multi\-class model container trained by Autopilot can be configured to output **the string representation** of the list of predicted labels and probabilities in **CSV** format\. In the following example, the probabilities can be extracted by index `1` and the label headers can be extracted by index `1` and the label headers can be extracted using index `0`\.


| Model container input | Model container output | 
| --- | --- | 
|  Single record  |  '"\[\\'cat\\',\\'dog\\',\\'fish\\'\]","\[0\.1,0\.6,0\.3\]"'  | 
|  Two records  |  '"\[\\'cat\\',\\'dog\\',\\'fish\\'\]","\[0\.1,0\.6,0\.3\]"\\n"\[\\'cat\\',\\'dog\\',\\'fish\\'\]","\[0\.2,0\.5,0\.3\]"'  | 

------

The following data examples are for model container outputs in **JSON Lines** format\.

------
#### [ Probability only ]

In this example, the model container outputs the probability that can be extracted by be extracted by [https://jmespath.org/](https://jmespath.org/) expression `'score'` in **JSON Lines** format\.


| Model container input | Model container output | 
| --- | --- | 
|  Single record  |  '\{"score":0\.6\}'  | 
|  Two records  |  '\{"score":0\.6\}\\n\{"score":0\.3\}'  | 

------
#### [ Predicted label and probabilities ]

In this example, a multi\-class model container outputs a list of label headers along with a list of probabilities in **JSON Lines** format\. The probabilities can be extracted by `JMESPath` expression `'probability'`, and the label headers can be extracted by `JMESPath` expression `'predicted labels'`\.


| Model container input | Model container output | 
| --- | --- | 
|  Single record  |  '\{"predicted\_labels":\["cat","dog","fish"\],"probabilities":\[0\.1,0\.6,0\.3\]\}'  | 
|  Two records  |  '\{"predicted\_labels":\["cat","dog","fish"\],"probabilities":\[0\.1,0\.6,0\.3\]\}\\n\{"predicted\_labels":\["cat","dog","fish"\],"probabilities":\[0\.2,0\.5,0\.3\]\}'  | 

------
#### [ Predicted labels header and probabilities ]

In this example, a multi\-class model container outputs a list of label headers and probabilities in **JSON Lines ** format\. The probabilities can be extracted by `JMESPath` expression `'probability'`, and the label headers can be extracted by `JMESPath` expression `'predicted labels'`\.


| Model container input | Model container output | 
| --- | --- | 
|  Single record  |  '\{"predicted\_labels":\["cat","dog","fish"\],"probabilities":\[0\.1,0\.6,0\.3\]\}'  | 
|  Two records  |  '\{"predicted\_labels":\["cat","dog","fish"\],"probabilities":\[0\.1,0\.6,0\.3\]\}\\n\{"predicted\_labels":\["cat","dog","fish"\],"probabilities":\[0\.2,0\.5,0\.3\]\}'  | 

------

## Model container validation<a name="clarify-online-explainability-container-validation"></a>

We recommend that you deploy your model to a SageMaker real\-time inference endpoint, and send requests to the endpoint\. Manually examine the requests \(model container inputs\) and responses \(model container outputs\) to make sure that both are compliant with the requirements in the **Model Container Input** section and **Model Container Output** section\. If your model container supports batch requests, you can start with a single record request, and then try two or more records\.

The following commands show how to request a response using the AWS CLI\. The AWS CLI is pre\-installed in SageMaker Studio, and SageMaker Notebook instances\. If you need to install the AWS CLI, follow this [installation guide](https://aws.amazon.com/cli/)\.

```
aws sagemaker-runtime invoke-endpoint \
  --endpoint-name $ENDPOINT_NAME \
  --content-type $CONTENT_TYPE \
  --accept $ACCEPT_TYPE \
  --body $REQUEST_DATA \
  $CLI_BINARY_FORMAT \
  /dev/stderr 1>/dev/null
```

The parameters are defined, as follows:
+ `$ENDPOINT NAME`: The name of the endpoint\.
+ `$CONTENT_TYPE`: The MIME type of the request \(model container input\)\.
+ `$ACCEPT_TYPE`: The MIME type of the response \(model container output\)\.
+ `$REQUEST_DATA`: The requested payload string\.
+ `$CLI_BINARY_FORMAT`: The format of the command line interface \(CLI\) parameter\. For AWS CLI v1, this parameter should remain blank\. For v2, this parameter should be set to `--cli-binary-format raw-in-base64-out`\.

**Note**  
AWS CLI v2 passes binary parameters as base64\-encoded strings [default](https://docs.aws.amazon.com/cli/latest/userguide/cliv2-migration.html#cliv2-migration-binaryparam)\.

The following examples use AWS CLI v1:

------
#### [ Request and response in CSV format ]
+ The request consists of a single record and the response is its probability value\.

  ```
  aws sagemaker-runtime invoke-endpoint \
    --endpoint-name test-endpoint-sagemaker-xgboost-model \
    --content-type text/csv \
    --accept text/csv \
    --body '1,2,3,4' \
    /dev/stderr 1>/dev/null
  ```

  Output:

  `0.6`
+ The request consists of two records, and the response includes their probabilities, and the model separates the probabilities by a comma\. The `$'content'` expression in the `--body` tells the command to interpret `'\n'` in the content as a line break\.

  ```
  aws sagemaker-runtime invoke-endpoint \
    --endpoint-name test-endpoint-sagemaker-xgboost-model \
    --content-type text/csv \
    --accept text/csv \
    --body $'1,2,3,4\n5,6,7,8' \
    /dev/stderr 1>/dev/null
  ```

  Output:

  `0.6,0.3`
+ The request consists of two records, the response includes their probabilities, and the model separates the probabilities with a line break\.

  ```
  aws sagemaker-runtime invoke-endpoint \
    --endpoint-name test-endpoint-csv-1 \
    --content-type text/csv \
    --accept text/csv \
    --body $'1,2,3,4\n5,6,7,8' \
    /dev/stderr 1>/dev/null
  ```

  Output:

  `0.6`

  `0.3`
+ The request consists of a single record, and the response is probability values \(multi\-class model, three classes\)\.

  ```
  aws sagemaker-runtime invoke-endpoint \
    --endpoint-name test-endpoint-csv-1 \
    --content-type text/csv \
    --accept text/csv \
    --body '1,2,3,4' \
    /dev/stderr 1>/dev/null
  ```

  Output:

  `0.1,0.6,0.3`
+ The request consists of two records, and the response includes their probability values \(multi\-class model, three classes\)\.

  ```
  aws sagemaker-runtime invoke-endpoint \
    --endpoint-name test-endpoint-csv-1 \
    --content-type text/csv \
    --accept text/csv \
    --body $'1,2,3,4\n5,6,7,8' \
    /dev/stderr 1>/dev/null
  ```

  Output:

  `0.1,0.6,0.3`

  `0.2,0.5,0.3`
+ The request consists of two records, and the response includes predicted label and probability\.

  ```
  aws sagemaker-runtime invoke-endpoint \
    --endpoint-name test-endpoint-csv-2 \
    --content-type text/csv \
    --accept text/csv \
    --body $'1,2,3,4\n5,6,7,8' \
    /dev/stderr 1>/dev/null
  ```

  Output:

  `1,0.6`

  `0,0.3`
+ The request consists of two records and the response includes label headers and probabilities\.

  ```
  aws sagemaker-runtime invoke-endpoint \
    --endpoint-name test-endpoint-csv-3 \
    --content-type text/csv \
    --accept text/csv \
    --body $'1,2,3,4\n5,6,7,8' \
    /dev/stderr 1>/dev/null
  ```

  Output:

  `"['cat','dog','fish']","[0.1,0.6,0.3]"`

  `"['cat','dog','fish']","[0.2,0.5,0.3]"`

------
#### [ Request and response in JSON Lines format ]
+ The request consists of a single record and the response is its probability value\.

  ```
  aws sagemaker-runtime invoke-endpoint \
    --endpoint-name test-endpoint-jsonlines \
    --content-type application/jsonlines \
    --accept application/jsonlines \
    --body '{"features":["This is a good product",5]}' \
    /dev/stderr 1>/dev/null
  ```

  Output:

  `{"score":0.6}`
+ The request contains two records, and the response includes predicted label and probability\.

  ```
  aws sagemaker-runtime invoke-endpoint \
    --endpoint-name test-endpoint-jsonlines-2 \
    --content-type application/jsonlines \
    --accept application/jsonlines \
    --body $'{"features":[1,2,3,4]}\n{"features":[5,6,7,8]}' \
    /dev/stderr 1>/dev/null
  ```

  Output:

  `{"predicted_label":1,"probability":0.6}`

  `{"predicted_label":0,"probability":0.3}`
+ The request contains two records and the response includes label headers and probabilities\.

  ```
  aws sagemaker-runtime invoke-endpoint \
    --endpoint-name test-endpoint-jsonlines-3 \
    --content-type application/jsonlines \
    --accept application/jsonlines \
    --body $'{"data":{"features":[1,2,3,4]}}\n{"data":{"features":[5,6,7,8]}}' \
    /dev/stderr 1>/dev/null
  ```

  Output:

  `{"predicted_labels":["cat","dog","fish"],"probabilities":[0.1,0.6,0.3]}`

  `{"predicted_labels":["cat","dog","fish"],"probabilities":[0.2,0.5,0.3]}`

------
#### [ Request and response in different formats ]
+ The request is in CSV format and the response is in JSON Lines format:

  ```
  aws sagemaker-runtime invoke-endpoint \
    --endpoint-name test-endpoint-csv-in-jsonlines-out \
    --content-type text/csv \
    --accept application/jsonlines \
    --body $'1,2,3,4\n5,6,7,8' \
    /dev/stderr 1>/dev/null
  ```

  Output:

  `{"probability":0.6}`

  `{"probability":0.3}`
+ The request is in JSON Lines format and the response is in CSV format:

  ```
  aws sagemaker-runtime invoke-endpoint \
    --endpoint-name test-endpoint-jsonlines-in-csv-out \
    --content-type application/jsonlines \
    --accept text/csv \
    --body $'{"features":[1,2,3,4]}\n{"features":[5,6,7,8]}' \
    /dev/stderr 1>/dev/null
  ```

  Output:

  `0.6`

  `0.3`

------

After the validations are complete, [delete](https://docs.aws.amazon.com/sagemaker/latest/dg/realtime-endpoints-delete-resources.html) the testing endpoint\.