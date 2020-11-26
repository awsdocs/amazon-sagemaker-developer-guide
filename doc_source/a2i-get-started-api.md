# Demo: Get Started Using the Amazon A2I API<a name="a2i-get-started-api"></a>

This walkthrough explains the API operations you can use to get started using Amazon A2I\. 

To use a Jupyter Notebook to execute these operations, select a Jupyter notebook from [Use Cases and Examples using Amazon A2I](a2i-task-types-general.md) and use [Use SageMaker Notebook Instance with Amazon A2I Jupyter Notebook](a2i-task-types-general.md#a2i-task-types-notebook-demo) to learn how to use it in SageMaker notebook instance\.

To learn more about the API operations you can use with Amazon A2I, see [Use APIs in Amazon Augmented AIAPI References](a2i-api-references.md)\.

## Create a Private Work Team<a name="a2i-get-started-api-create-work-team"></a>

You can create a private work team and add yourself as a worker so that you can preview Amazon A2I\. 

If you are not familiar with Amazon Cognito, we recommend that you use the SageMaker console to create a private workforce and add yourself as a private worker\. For instructions, see [Step 1: Create a Work Team](a2i-get-started-console.md#a2i-get-started-console-step-1)\.

If you are familiar with Amazon Cognito you can use the following instructions to create a private work team using the SageMaker API\. After you create a work team, note down the work team ARN \(`WorkteamArn`\)\.

To learn more about the private workforce, and other available configurations, see [Use a Private Workforce](sms-workforce-private.md)\.

**Create a private workforce**  
If you have not created a private workforce, you can do so using an [Amazon Cognito user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools.html)\. Make sure that you have added yourself to this user pool\. You can create a private work team using the AWS Python SDK \(Boto3\) function, `[create\_workforce](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_workforce)`\. For other language\-specific SDKs, refer to the list in [CreateWorkforce](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateWorkforce.html#API_CreateWorkforce_SeeAlso)\.

```
    
    response = client.create_workforce(
        CognitoConfig={
            "UserPool": "Pool_ID",
            "ClientId": "app-client-id"
        },
        WorkforceName="workforce-name"
    (
```

**Create a private work team**  
After you have created a private workforce in the AWS Region to configure and start your human loop, you can create a private work team using the AWS Python SDK \(Boto3\) function, `[create\_workteam](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_workteam)`\. For other language\-specific SDKs, refer to the list in `[CreateWorkteam](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateWorkteam.html#API_CreateWorkteam_SeeAlso)`\.

```
    response = client.create_workteam(
        WorkteamName="work-team-name",
        WorkforceName= "workforce-name",
        MemberDefinitions=[
            {
                "CognitoMemberDefinition": {
                    "UserPool": "<aws-region>_ID",
                    "UserGroup": "user-group",
                    "ClientId": "app-client-id"
                },
            }
        ]
    )
```

Access your work team ARN as follows:

```
    workteamArn = response["WorkteamArn"]
```

**List private work teams in your account**  
If you have already created a private work team, you can list all work teams in a given AWS Region in your account using the AWS Python SDK \(Boto3\) function, `[list\_workteams](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.list_workteams)`\. For other language\-specific SDKs, refer to the list in `[ListWorkteams](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ListWorkteams.html#API_ListWorkteams_SeeAlso)`\. 

```
    response = client.list_workteams()
```

If you have numerous work teams in your account, you may want to use `MaxResults`, `SortBy`, and `NameContains` to filter your results\.

## Create a Human Review Workflow<a name="a2i-get-started-api-create-human-review-workflow"></a>

You can create a human review workflow using the Amazon A2I operation `[CreateFlowDefinition](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateFlowDefinition.html)`\. Before you create your human review workflow, you need to create a human task UI\. You can do this with the `[CreateHumanTaskUi](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateHumanTaskUi.html)` operation\.

If you are using Amazon A2I with the Amazon Textract or Amazon Rekognition integrations, you can specify activation conditions using a JSON\. 

### Create a Human Task UI<a name="a2i-get-started-api-worker-task-template"></a>

If you are creating a human review workflow to be used with Amazon Textract or Amazon Rekognition integrations, you need to use and modify pre\-made worker task template\. For all custom integrations, you can use your own, custom worker task template\. Use the following table to see how to create a human task UI using a worker task template for the two built\-in integrations\. Replace the template with your own to customize this request\. 

------
#### [ Amazon Textract\- Key\-value pair extraction ]

To learn more about this template, see [Custom Template Example for Amazon Textract](a2i-custom-templates.md#a2i-custom-templates-textract-sample)\.

```
template = r"""
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>
{% capture s3_uri %}http://s3.amazonaws.com/{{ task.input.aiServiceRequest.document.s3Object.bucket }}/{{ task.input.aiServiceRequest.document.s3Object.name }}{% endcapture %}
<crowd-form>
  <crowd-textract-analyze-document 
      src="{{ s3_uri | grant_read_access }}" 
      initial-value="{{ task.input.selectedAiServiceResponse.blocks }}" 
      header="Review the key-value pairs listed on the right and correct them if they don"t match the following document." 
      no-key-edit="" 
      no-geometry-edit="" 
      keys="{{ task.input.humanLoopContext.importantFormKeys }}" 
      block-types='["KEY_VALUE_SET"]'>
    <short-instructions header="Instructions">
        <p>Click on a key-value block to highlight the corresponding key-value pair in the document.
        </p><p><br></p>
        <p>If it is a valid key-value pair, review the content for the value. If the content is incorrect, correct it.
        </p><p><br></p>
        <p>The text of the value is incorrect, correct it.</p>
        <p><img src="https://assets.crowd.aws/images/a2i-console/correct-value-text.png">
        </p><p><br></p>
        <p>A wrong value is identified, correct it.</p>
        <p><img src="https://assets.crowd.aws/images/a2i-console/correct-value.png">
        </p><p><br></p>
        <p>If it is not a valid key-value relationship, choose No.</p>
        <p><img src="https://assets.crowd.aws/images/a2i-console/not-a-key-value-pair.png">
        </p><p><br></p>
        <p>If you can’t find the key in the document, choose Key not found.</p>
        <p><img src="https://assets.crowd.aws/images/a2i-console/key-is-not-found.png">
        </p><p><br></p>
        <p>If the content of a field is empty, choose Value is blank.</p>
        <p><img src="https://assets.crowd.aws/images/a2i-console/value-is-blank.png">
        </p><p><br></p>
        <p><strong>Examples</strong></p>
        <p>Key and value are often displayed next or below to each other.
        </p><p><br></p>
        <p>Key and value displayed in one line.</p>
        <p><img src="https://assets.crowd.aws/images/a2i-console/sample-key-value-pair-1.png">
        </p><p><br></p>
        <p>Key and value displayed in two lines.</p>
        <p><img src="https://assets.crowd.aws/images/a2i-console/sample-key-value-pair-2.png">
        </p><p><br></p>
        <p>If the content of the value has multiple lines, enter all the text without line break. 
        Include all value text even if it extends beyond the highlight box.</p>
        <p><img src="https://assets.crowd.aws/images/a2i-console/multiple-lines.png"></p>
    </short-instructions>
    <full-instructions header="Instructions"></full-instructions>
  </crowd-textract-analyze-document>
</crowd-form>
"""
```

------
#### [ Amazon Rekognition \- Image moderation ]

To learn more about this template, see [Custom Template Example for Amazon Rekognition](a2i-custom-templates.md#a2i-custom-templates-rekognition-sample)\.

```
template = r"""
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>
{% capture s3_arn %}http://s3.amazonaws.com/{{ task.input.aiServiceRequest.image.s3Object.bucket }}/{{ task.input.aiServiceRequest.image.s3Object.name }}{% endcapture %}

<crowd-form>
  <crowd-rekognition-detect-moderation-labels
    categories='[
      {% for label in task.input.selectedAiServiceResponse.moderationLabels %}
        {
          name: "{{ label.name }}",
          parentName: "{{ label.parentName }}",
        },
      {% endfor %}
    ]'
    src="{{ s3_arn | grant_read_access }}"
    header="Review the image and choose all applicable categories."
  >
    <short-instructions header="Instructions">
      <style>
        .instructions {
          white-space: pre-wrap;
        }
      </style>
      <p class="instructions">Review the image and choose all applicable categories.
If no categories apply, choose None.

<b>Nudity</b>
Visuals depicting nude male or female person or persons

<b>Partial Nudity</b>
Visuals depicting covered up nudity, for example using hands or pose

<b>Revealing Clothes</b>
Visuals depicting revealing clothes and poses

<b>Physical Violence</b>
Visuals depicting violent physical assault, such as kicking or punching

<b>Weapon Violence</b>
Visuals depicting violence using weapons like firearms or blades, such as shooting

<b>Weapons</b>
Visuals depicting weapons like firearms and blades
    </short-instructions>

    <full-instructions header="Instructions"></full-instructions>
  </crowd-rekognition-detect-moderation-labels>
</crowd-form>"""
```

------
#### [ Custom Integration ]

The following is an example template that can be used in a custom integration\. This template is used in this [notebook](https://github.com/aws-samples/amazon-a2i-sample-jupyter-notebooks/blob/master/Amazon%20Augmented%20AI%20(A2I)%20and%20Comprehend%20DetectSentiment.ipynb), demonstrating a custom integration with Amazon Comprehend\.

```
template = r"""
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>

<crowd-form>
    <crowd-classifier
      name="sentiment"
      categories='["Positive", "Negative", "Neutral", "Mixed"]'
      initial-value="{{ task.input.initialValue }}"
      header="What sentiment does this text convey?"
    >
      <classification-target>
        {{ task.input.taskObject }}
      </classification-target>
      
      <full-instructions header="Sentiment Analysis Instructions">
        <p><strong>Positive</strong> sentiment include: joy, excitement, delight</p>
        <p><strong>Negative</strong> sentiment include: anger, sarcasm, anxiety</p>
        <p><strong>Neutral</strong>: neither positive or negative, such as stating a fact</p>
        <p><strong>Mixed</strong>: when the sentiment is mixed</p>
      </full-instructions>

      <short-instructions>
       Choose the primary sentiment that is expressed by the text. 
      </short-instructions>
    </crowd-classifier>
</crowd-form>
"""
```

------

Using the template specified above, you can create a template using the AWS Python SDK \(Boto3\) function, `[create\_human\_task\_ui](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_human_task_ui)`\. For other language\-specific SDKs, refer to the list in `[CreateHumanTaskUi](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateHumanTaskUi.html#API_CreateHumanTaskUi_SeeAlso)`\. 

```
    
    response = client.create_human_task_ui(
        HumanTaskUiName="human-task-ui-name",
        UiTemplate={
            "Content": template
        }
    )
```

This response element contains the human task UI ARN\. Save this as follows:

```
    humanTaskUiArn = response["HumanTaskUiArn"]
```

### Create JSON to specify activation conditions<a name="a2i-get-started-api-activation-conditions"></a>

For Amazon Textract and Amazon Rekognition built\-in integrations, you can save activation conditions in a JSON object and use this in your `CreateFlowDefinition` request\. 

Next, select a tab to see example activation conditions you can use for these built\-in integrations\. For additional information about activation condition options, see [JSON Schema for Human Loop Activation Conditions in Amazon Augmented AI](a2i-human-fallback-conditions-json-schema.md)\.

------
#### [ Amazon Textract\- Key\-value pair extraction ]

This example specifies conditions for specific keys \(such as `Mail address`\) in the document\. If Amazon Textract's confidence falls outside of the thresholds set here, the document is sent to a human for review, with the specific keys that triggered the human loop prompted to the worker\.

```
      import json  

      humanLoopActivationConditions = json.dumps(
        {
            "Conditions": [
                {
                  "Or": [
                    
                    {
                        "ConditionType": "ImportantFormKeyConfidenceCheck",
                        "ConditionParameters": {
                            "ImportantFormKey": "Mail address",
                            "ImportantFormKeyAliases": ["Mail Address:","Mail address:", "Mailing Add:","Mailing Addresses"],
                            "KeyValueBlockConfidenceLessThan": 100,
                            "WordBlockConfidenceLessThan": 100
                        }
                    },
                    {
                        "ConditionType": "MissingImportantFormKey",
                        "ConditionParameters": {
                            "ImportantFormKey": "Mail address",
                            "ImportantFormKeyAliases": ["Mail Address:","Mail address:","Mailing Add:","Mailing Addresses"]
                        }
                    },
                    {
                        "ConditionType": "ImportantFormKeyConfidenceCheck",
                        "ConditionParameters": {
                            "ImportantFormKey": "Phone Number",
                            "ImportantFormKeyAliases": ["Phone number:", "Phone No.:", "Number:"],
                            "KeyValueBlockConfidenceLessThan": 100,
                            "WordBlockConfidenceLessThan": 100
                        }
                    },
                    {
                      "ConditionType": "ImportantFormKeyConfidenceCheck",
                      "ConditionParameters": {
                        "ImportantFormKey": "*",
                        "KeyValueBlockConfidenceLessThan": 100,
                        "WordBlockConfidenceLessThan": 100
                      }
                    },
                    {
                      "ConditionType": "ImportantFormKeyConfidenceCheck",
                      "ConditionParameters": {
                        "ImportantFormKey": "*",
                        "KeyValueBlockConfidenceGreaterThan": 0,
                        "WordBlockConfidenceGreaterThan": 0
                      }
                    }
            ]
        }
            ]
        }
    )
```

------
#### [ Amazon Rekognition \- Image moderation ]

The human loop activation conditions used here are tailored towards Amazon Rekognition content moderation; they are based on the confidence thresholds for particular moderation labels, `Suggestive` and `Female Swimwear Or Underwear`\.

```
        import json  

        humanLoopActivationConditions = json.dumps(
        {
            "Conditions": [
                {
                  "Or": [
                    {
                        "ConditionType": "ModerationLabelConfidenceCheck",
                        "ConditionParameters": {
                            "ModerationLabelName": "Suggestive",
                            "ConfidenceLessThan": 98
                        }
                    },
                    {
                        "ConditionType": "ModerationLabelConfidenceCheck",
                        "ConditionParameters": {
                            "ModerationLabelName": "Female Swimwear Or Underwear",
                            "ConfidenceGreaterThan": 98
                        }
                    }
                  ]
               }
            ]
        }
    )
```

------

### Create a human review workflow<a name="a2i-get-started-api-flow-definition"></a>

This section gives an example of the `CreateFlowDefinition` AWS Python SDK \(Boto3\) request using the resources created in the previous sections\. For other language\-specific SDKs, refer to the list in [CreateFlowDefinition](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateFlowDefinition.html#API_CreateFlowDefinition_SeeAlso)\. Use the tabs in the following table to see the requests to create a human review workflow for Amazon Textract and Amazon Rekognition built\-in integrations\.

------
#### [ Amazon Textract\- Key\-value pair extraction ]

If you use the built\-in integration with Amazon Textract, you must specify `"AWS/Textract/AnalyzeDocument/Forms/V1"` for `"AwsManagedHumanLoopRequestSource"` in `HumanLoopRequestSource`\. 

```
    response = client.create_flow_definition(
        FlowDefinitionName="human-review-workflow-name",
        HumanLoopRequestSource={
            "AwsManagedHumanLoopRequestSource": "AWS/Textract/AnalyzeDocument/Forms/V1"
        }, 
        HumanLoopActivationConfig={
            "HumanLoopActivationConditionsConfig": {
                "HumanLoopActivationConditions": humanLoopActivationConditions
            }
        },
        HumanLoopConfig={
            "WorkteamArn": workteamArn,
            "HumanTaskUiArn": humanTaskUiArn,
            "TaskTitle": "Document entry review",
            "TaskDescription": "Review the document and instructions. Complete the task",
            "TaskCount": 1,
            "TaskAvailabilityLifetimeInSeconds": 43200,
            "TaskTimeLimitInSeconds": 3600,
            "TaskKeywords": [
                "document review",
            ],
        },
        OutputConfig={
            "S3OutputPath": "s3://DOC-EXAMPLE-BUCKET/prefix/",
        },
        RoleArn="arn:aws:iam::<account-number>:role/<role-name>",
        Tags=[
            {
                "Key": "string",
                "Value": "string"
            },
        ]
    )
```

------
#### [ Amazon Rekognition \- Image moderation ]

If you use the built\-in integration with Amazon Rekognition, you must specify `"AWS/Rekognition/DetectModerationLabels/Image/V3"` for `"AwsManagedHumanLoopRequestSource"` in `HumanLoopRequestSource`\.

```
    response = client.create_flow_definition(
        FlowDefinitionName="human-review-workflow-name",
        HumanLoopRequestSource={
            "AwsManagedHumanLoopRequestSource": "AWS/Rekognition/DetectModerationLabels/Image/V3"
        }, 
        HumanLoopActivationConfig={
            "HumanLoopActivationConditionsConfig": {
                "HumanLoopActivationConditions": humanLoopActivationConditions
            }
        },
        HumanLoopConfig={
            "WorkteamArn": workteamArn,
            "HumanTaskUiArn": humanTaskUiArn,
            "TaskTitle": "Image content moderation",
            "TaskDescription": "Review the image and instructions. Complete the task",
            "TaskCount": 1,
            "TaskAvailabilityLifetimeInSeconds": 43200,
            "TaskTimeLimitInSeconds": 3600,
            "TaskKeywords": [
                "content moderation",
            ],
        },
        OutputConfig={
            "S3OutputPath": "s3://DOC-EXAMPLE-BUCKET/prefix/",
        },
        RoleArn="arn:aws:iam::<account-number>:role/<role-name>",
        Tags=[
            {
                "Key": "string",
                "Value": "string"
            },
        ]
    )
```

------
#### [ Custom Integration ]

If you use a custom integration, exclude the following parameters: `HumanLoopRequestSource`, `HumanLoopActivationConfig`\.

```
    response = client.create_flow_definition(
        FlowDefinitionName="human-review-workflow-name",
        HumanLoopConfig={
            "WorkteamArn": workteamArn,
            "HumanTaskUiArn": humanTaskUiArn,
            "TaskTitle": "Image content moderation",
            "TaskDescription": "Review the image and instructions. Complete the task",
            "TaskCount": 1,
            "TaskAvailabilityLifetimeInSeconds": 43200,
            "TaskTimeLimitInSeconds": 3600,
            "TaskKeywords": [
                "content moderation",
            ],
        },
        OutputConfig={
            "S3OutputPath": "s3://DOC-EXAMPLE-BUCKET/prefix/",
        },
        RoleArn="arn:aws:iam::<account-number>:role/<role-name>",
        Tags=[
            {
                "Key": "string",
                "Value": "string"
            },
        ]
    )
```

------

After you create a human review workflow, you can retrieve the flow definition ARN from the response:

```
    humanReviewWorkflowArn = response["FlowDefinitionArn"]    
```

## Create a Human Loop<a name="a2i-get-started-api-create-human-loop"></a>

The API operation you use to start a human loop depends on the Amazon A2I integration you use\. 
+ If you use the Amazon Textract built\-in integration, you use the [AnalyzeDocument](https://docs.aws.amazon.com/textract/latest/dg/API_AnalyzeDocument.html) operation\.
+ If you use the Amazon Rekognition built\-in integration, you use the [DetectModerationLabels](https://docs.aws.amazon.com/rekognition/latest/dg/API_DetectModerationLabels.html) operation\.
+ If you use a custom integration, you use the [StartHumanLoop](https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_StartHumanLoop.html) operation\. 

Select your task type in the following table to see example requests for Amazon Textract and Amazon Rekognition using the AWS SDK for Python \(Boto3\)\. 

------
#### [ Amazon Textract\- Key\-value pair extraction ]

The following example uses the AWS SDK for Python \(Boto3\) to call `analyze_document` in us\-west\-2\. Replace the red, italicized text with your resources\. Include the [https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_HumanLoopDataAttributes.html](https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_HumanLoopDataAttributes.html) parameter if you are using the Amazon Mechanical Turk workforce\. For more information, see [analyze\_document](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/textract.html#Textract.Client.analyze_document) documention in the AWS SDK for Python \(Boto\) API Reference\.

```
   response = client.analyze_document(
         Document={"S3Object": {"Bucket": "AWSDOC-EXAMPLE-BUCKET", "Name": "document-name.pdf"},
         HumanLoopConfig={
            "FlowDefinitionArn":"arn:aws:sagemaker:us-west-2:111122223333:flow-definition/flow-definition-name",
            "HumanLoopName":"human-loop-name",
            "DataAttributes" : {ContentClassifiers=["FreeOfPersonallyIdentifiableInformation"|"FreeOfAdultContent"]}
         }
         FeatureTypes=["FORMS"]
    )
```

Human loops are only created if Amazon Textract's confidence for document analysis task meets the activation conditions you specified in your human review workflow\. You can check the `response` element to determine if a human loop has been created\. To see everything included in this response, see [https://docs.aws.amazon.com/textract/latest/dg/API_HumanLoopActivationOutput.html](https://docs.aws.amazon.com/textract/latest/dg/API_HumanLoopActivationOutput.html)\.

```
    if "HumanLoopArn" in analyzeDocumentResponse["HumanLoopActivationOutput"]:
        # A human loop has been started!
        print(f"A human loop has been started with ARN: {analyzeDocumentResponse["HumanLoopActivationOutput"]["HumanLoopArn"]}"
```

------
#### [ Amazon Rekognition \- Image moderation ]

The following example uses the AWS SDK for Python \(Boto3\) to call `detect_moderation_labels` in us\-west\-2\. Replace the red, italicized text with your resources\. Include the [https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_HumanLoopDataAttributes.html](https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_HumanLoopDataAttributes.html) parameter if you are using the Amazon Mechanical Turk workforce\. For more information, see [detect\_moderation\_labels](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rekognition.html#Rekognition.Client.detect_moderation_labels) documention in the AWS SDK for Python \(Boto\) API Reference\.

```
   response = client.detect_moderation_labels(
            Image={"S3Object":{"Bucket": "AWSDOC-EXAMPLE-BUCKET", "Name": "image-name.png"}},
            HumanLoopConfig={
               "FlowDefinitionArn":"arn:aws:sagemaker:us-west-2:111122223333:flow-definition/flow-definition-name",
               "HumanLoopName":"human-loop-name",
               "DataAttributes" : {ContentClassifiers=["FreeOfPersonallyIdentifiableInformation"|"FreeOfAdultContent"]}
             }
    )
```

Human loops are only created if Amazon Rekognition's confidence for an image moderation task meets the activation conditions you specified in your human review workflow\. You can check the `response` element to determine if a human loop has been created\. To see everything included in this response, see [https://docs.aws.amazon.com/rekognition/latest/dg/API_HumanLoopActivationOutput.html](https://docs.aws.amazon.com/rekognition/latest/dg/API_HumanLoopActivationOutput.html)\.

```
    if "HumanLoopArn" in response["HumanLoopActivationOutput"]:
        # A human loop has been started!
        print(f"A human loop has been started with ARN: {response["HumanLoopActivationOutput"]["HumanLoopArn"]}")
```

------
#### [ Custom Integration ]

The following example uses the AWS SDK for Python \(Boto3\) to call `start_human_loop` in us\-west\-2\. Replace the red, italicized text with your resources\. Include the [https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_HumanLoopDataAttributes.html](https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_HumanLoopDataAttributes.html) parameter if you are using the Amazon Mechanical Turk workforce\. For more information, see [start\_human\_loop](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker-a2i-runtime.html#AugmentedAIRuntime.Client.start_human_loop) documention in the AWS SDK for Python \(Boto\) API Reference\.

```
   response = client.start_human_loop(
        HumanLoopName= "human-loop-name",
        FlowDefinitionArn= "arn:aws:sagemaker:us-west-2:111122223333:flow-definition/flow-definition-name",
        HumanLoopInput={"InputContent": inputContentJson},
        DataAttributes={"ContentClassifiers": ["FreeOfPersonallyIdentifiableInformation"|"FreeOfAdultContent"]}
   )
```

This example stores intput content in the variable *`inputContentJson`*\. Assume that the input content contains two elements: a text blurb and sentiment \(such as Positive, Negative, or Neutral\), and it is formatted as follows:

```
    inputContent = {
        "initialValue": sentiment,
         "taskObject": blurb
     }
```

The keys `initialValue` and `taskObject` must correspond to the keys used in the liquid elements of our worker task template\. Refer to the custom template in [Create a Human Task UI](#a2i-get-started-api-worker-task-template) to see an example\. 

To create `inputContentJson`, do the following\. 

```
    import json
    
    inputContentJson = json.dumps(inputContent)
```

A human loop starts each time you call `start_human_loop`\. To check the status of your human loop, use [describe\_human\_loop](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker-a2i-runtime.html#AugmentedAIRuntime.Client.describe_human_loop): 

```
    human_loop_info = a2i.describe_human_loop(HumanLoopName="human_loop_name")
    print(f"HumanLoop Status: {resp["HumanLoopStatus"]}")
    print(f"HumanLoop Output Destination: {resp["HumanLoopOutput"]}")
```

------