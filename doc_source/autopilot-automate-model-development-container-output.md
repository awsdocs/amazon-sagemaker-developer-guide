# Configure inference output in generated containers<a name="autopilot-automate-model-development-container-output"></a>

Amazon SageMaker Autopilot generates an ordered [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ContainerDefinition.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ContainerDefinition.html) list\. This can be used to build a model to deploy in a machine learning pipeline\. This model can be used for online hosting and inference\. 

Customers can list inference container definitions with the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ListCandidateForAutoMLJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ListCandidateForAutoMLJob.html) API\. The list of inference container definitions that represent the best candidate is also available in the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeAutoMLJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DescribeAutoMLJob.html) response\.

## Inference container definitions for regression and classification problem types<a name="autopilot-problem-type-container-output"></a>

Autopilot generates inference containers specific to the [training mode](sagemaker/latest/dg/autopilot-model-support-validation.html#autopilot-training-mode) and the [problem type](sagemaker/latest/dg/autopilot-datasets-problem-types.html#autopilot-problem-types) of the job\.

### Container definitions for hyperparameter optimization \(HPO\) mode<a name="autopilot-problem-type-container-output-hpo"></a>
+ **Regression**: HPO generates two containers:

  1. A feature engineering container that transforms the original features into features that the regression algorithms can train on\.

  1. An algorithm container that transforms features and generates a regression score for the dataset\.
+ **Classification**: HPO generates three containers:

  1. A feature engineering container that transforms the original features into features that the classification algorithms can train on\.

  1. An algorithm container that generates the `predicted_label` with the highest probability\. This container can also produce the various probabilities associated with the classification outcomes in the inference response\.

  1. A feature engineering container that performs post\-processing of the algorithm prediction\. For example, it can perform an inverse transform on the predicted label and change it to the original label\. 

### Container definitions for ensembling mode<a name="autopilot-problem-type-container-output-ensemble"></a>

In ensembling mode, both regression and classification problem types have only one inference container\. This inference container transforms the features and generates the predictions based on problem type\. 

## Inference responses per problem type<a name="autopilot-problem-type-inference-response"></a>

### Inference responses for classification models<a name="autopilot-problem-type-inference-response-classification"></a>

For classification inference containers, you can select the content of the inference response by using four predefined keys:
+ `predicted_label`: The label with the highest probability of predicting the correct label, as determined by Autopilot\.
+ `probability`: 
  + **HPO models:** The probability of the `True` class for binary classification\. The probability of the `predicted_label` for multiclass classification\.
  + **Ensemble models:** The probability of the `predicted_label` for binary and multiclass classification\.
+ `probabilities`: The list of probabilities for all corresponding classes\.
+ `labels`: The list of all labels\.

By default, inference containers are configured to generate only the `predicted_label`\. To select additional inference content, you can update the `inference_response_keys` parameter to include up to these three environment variables:
+ `SAGEMAKER_INFERENCE_SUPPORTED`: This is set to provide hints to you about what content each container supports\.
+ `SAGEMAKER_INFERENCE_INPUT`: This should be set to the keys that the container expects in input payload\.
+ `SAGEMAKER_INFERENCE_OUTPUT`: This should be populated with the set of keys that the container outputs\.

### Inference responses for classification models in HPO mode<a name="autopilot-problem-type-inference-response-classification-hpo"></a>

This section shows how to configure the inference response from classification models using hyperparameter optimization \(HPO\) mode\.

To choose the inference response content in HPO mode: Add the `SAGEMAKER_INFERENCE_INPUT` and `SAGEMAKER_INFERENCE_OUTPUT` variables to the second and third containers that are generated in HPO mode for classification problems\.

The keys supported by the second container \(algorithm\) are predicted\_label, probability, and probabilities\. Note that `labels` is deliberately not added to `SAGEMAKER_INFERENCE_SUPPORTED`\.

The keys supported by the third classification model container are `predicted_label`, `labels`, `probability`, and `probabilities`\. Therefore, the `SAGEMAKER_INFERENCE_SUPPORTED` environment includes the names of these keys\.

To update the definition of the inference containers to receive `predicted_label` and `probability`, use the following code example\.

```
containers[1]['Environment'].update({'SAGEMAKER_INFERENCE_OUTPUT': 'predicted_label, probability'})
containers[2]['Environment'].update({'SAGEMAKER_INFERENCE_INPUT': 'predicted_label, probability'})
containers[2]['Environment'].update({'SAGEMAKER_INFERENCE_OUTPUT': 'predicted_label, probability'})
```

The following code example updates the definition of the inference containers to receive `predicted_label`, `probabilities`, and `labels`\. Do not pass the `labels` to the second container \(the algorithm container\), because it is generated by the third container independently\. 

```
containers[1]['Environment'].update({'SAGEMAKER_INFERENCE_OUTPUT': 'predicted_label,probabilities'})
containers[2]['Environment'].update({'SAGEMAKER_INFERENCE_INPUT': 'predicted_label,probabilities'})
containers[2]['Environment'].update({'SAGEMAKER_INFERENCE_OUTPUT': 'predicted_label, probabilities,labels'})
```

The following collapsible sections provide code examples for AWS SDK for Python \(Boto3\) and for SageMaker SDK for Python\. Each section shows how to select the content of the inference responses in HPO mode for the respective code example\.

#### AWS SDK for Python \(Boto3\)<a name="autopilot-problem-type-inference-response-classification-hpo-boto3"></a>

```
import boto3

sm_client = boto3.client('sagemaker', region_name='<Region>')

role = '<IAM role>'
input_data = '<S3 input uri>'
output_path = '<S3 output uri>'

best_candidate = sm_client.describe_auto_ml_job(AutoMLJobName='<AutoML Job Name>')['BestCandidate']
best_candidate_containers = best_candidate['InferenceContainers']
best_candidate_name = best_candidate['CandidateName']

best_candidate_containers[1]['Environment'].update({'SAGEMAKER_INFERENCE_OUTPUT': 'predicted_label, probability'})
best_candidate_containers[2]['Environment'].update({'SAGEMAKER_INFERENCE_INPUT': 'predicted_label, probability'})
best_candidate_containers[2]['Environment'].update({'SAGEMAKER_INFERENCE_OUTPUT': 'predicted_label, probability'})

# create model
reponse = sm_client.create_model(
    ModelName = '<Model Name>',
    ExecutionRoleArn = role,
    Containers = best_candidate_containers
)

# Lauch Transform Job
response = sm_client.create_transform_job(
    TransformJobName='<Transform Job Name>',
    ModelName='<Model Name>',
    TransformInput={
        'DataSource': {
            'S3DataSource': {
                'S3DataType': 'S3Prefix',
                'S3Uri': input_data
            }
        },
        'ContentType': "text/CSV",
        'SplitType': 'Line'
    },
    TransformOutput={
        'S3OutputPath': output_path,
        'AssembleWith': 'Line',
    },
    TransformResources={
        'InstanceType': 'ml.m4.xlarge',
        'InstanceCount': 1,
    },
)
```

#### SageMaker SDK for Python<a name="autopilot-problem-type-inference-response-classification-hpo-sdk"></a>

```
from sagemaker import AutoML

aml = AutoML.attach(auto_ml_job_name='<AutoML Job Name>')
aml_best_model = aml.create_model(name='<Model Name>',
                                  candidate=None,
                                  inference_response_keys**=['probabilities', 'labels'])

aml_transformer = aml_best_model.transformer(accept='text/csv',
                                            assemble_with='Line',
                                            instance_type='ml.m5.xlarge',
                                            instance_count=1,)

aml_transformer.transform('<S3 input uri>',
                          content_type='text/csv',
                          split_type='Line',
                          job_name='<Transform Job Name>',
                          wait=True)
```

### Inference responses for classification models in ensembling mode<a name="autopilot-problem-type-inference-response-classification-ensemble"></a>

This section shows how to configure the inference response from classification models using ensembling mode\. 

In **ensembling mode**, to choose the content of the inference response, update the `SAGEMAKER_INFERENCE_OUTPUT` environment variable\.

The keys supported by the classification model container are `predicted_label`, `labels`, `probability`, and `probabilities`\. These keys are included in the `SAGEMAKER_INFERENCE_SUPPORTED` environment\.

To update the inference container definition to receive `predicted_label` and `probability`, refer to the following code example\.

```
containers[0]['Environment'].update({'SAGEMAKER_INFERENCE_OUTPUT': 'predicted_label, probability'})
```

The following collapsible section provides a code example for selecting the content of the inference responses in ensembling mode\. The example uses AWS SDK for Python \(Boto3\)\.

#### AWS SDK for Python \(Boto3\)<a name="autopilot-problem-type-inference-response-classification-ensembling-boto3"></a>

```
import boto3
sm_client = boto3.client('sagemaker', region_name='<Region>')

role = '<IAM role>'
input_data = '<S3 input uri>'
output_path = '<S3 output uri>' 

best_candidate = sm_client.describe_auto_ml_job(AutoMLJobName='<AutoML Job Name>')['BestCandidate']
best_candidate_containers = best_candidate['InferenceContainers']
best_candidate_name = best_candidate['CandidateName']

*best_candidate_containers[0]['Environment'].update({'SAGEMAKER_INFERENCE_OUTPUT': 'predicted_label, probability'})
*
# create model
reponse = sm_client.create_model(
    ModelName = '<Model Name>',
    ExecutionRoleArn = role,
    Containers = best_candidate_containers
)

# Lauch Transform Job
response = sm_client.create_transform_job(
    TransformJobName='<Transform Job Name>',
    ModelName='<Model Name>',
    TransformInput={
        'DataSource': {
            'S3DataSource': {
                'S3DataType': 'S3Prefix',
                'S3Uri': input_data
            }
        },
        'ContentType': "text/CSV",
        'SplitType': 'Line'
    },
    TransformOutput={
        'S3OutputPath': output_path,
        'AssembleWith': 'Line',
    },
    TransformResources={
        'InstanceType': 'ml.m4.xlarge',
        'InstanceCount': 1,
    },
)
```

The following collapsible section provides a code example that is identical to the SageMaker SDK for Python example for HPO\. It is included for your convenience\.

#### SageMaker SDK for Python<a name="autopilot-problem-type-inference-response-classification-ensembling-sdk"></a>

The following HPO code example uses SageMaker SDK for Python\.

```
from sagemaker import AutoML

aml = AutoML.attach(auto_ml_job_name='<AutoML Job Name>')
aml_best_model = aml.create_model(name='<Model Name>',
                                  candidate=None,
                                  *inference_response_keys**=['probabilities', 'labels'])*

aml_transformer = aml_best_model.transformer(accept='text/csv',
                                            assemble_with='Line',
                                            instance_type='ml.m5.xlarge',
                                            instance_count=1,)

aml_transformer.transform('<S3 input uri>',
                          content_type='text/csv',
                          split_type='Line',
                          job_name='<Transform Job Name>',
                          wait=True)
```