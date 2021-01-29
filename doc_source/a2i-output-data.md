# Amazon A2I Output Data<a name="a2i-output-data"></a>

When your machine learning workflow sends Amazon A2I a data object, a *human loop* is created and human reviewers receive a *task* to review that data object\. The output data from each human review task is stored in the Amazon Simple Storage Service \(Amazon S3\) output bucket you specify in your human review workflow\. The path to the data uses the following pattern where `YYYY/MM/DD/hh/mm/ss` represents the human loop creation date with year \(`YYYY`\), month \(`MM`\) and day \(`DD`\) and the creation time with hour \(`hh`\), minute \(`mm`\) and second \(`ss`\)\. 

```
s3://customer-output-bucket-specified-in-flow-definition/flow-definition-name/YYYY/MM/DD/hh/mm/ss/human-loop-name/output.json
```

The content of your output data depends on the type of [task type](https://docs.aws.amazon.com/sagemaker/latest/dg/a2i-task-types-general.html) \(built\-in or custom\) and the type of [workforce](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-workforce-management.html) you use\. Your output data always includes the response from the human worker\. Additionally, output data may also include metadata about the human loop, the human reviewer \(worker\), and the data object\. 

Use the following sections to learn more about Amazon A2I output data format for different task types and workforces\. 

## Output Data From Built\-In Task Types<a name="sms-output-data-textract"></a>

Amazon A2I built\-in task types include Amazon Textract and Amazon Rekognition\. In addition to human responses, the output data from one of these tasks includes details about the reason the human loop was created and information about the integrated service used to create the human loop\. Use the following table to learn more about the output data schema for all built\-in task types\. The *value* for each of these parameters depends on the service you use with Amazon A2I\. Refer to the second table in this section for more information about these service\-specific values\. 


****  

| Parameter | Value Type | Example Values | Description | 
| --- | --- | --- | --- | 
| awsManagedHumanLoopRequestSource |  String  | AWS/Rekognition/DetectModerationLabels/Image/V3 or AWS/Textract/AnalyzeDocument/Forms/V1 | The API operation and associated AWS services that requested Amazon A2I to create the a human loop\. This is the API operation you use to configure your Amazon A2I human loop\. | 
| flowDefinitionArn |  String  | arn:aws:sagemaker:us\-west\-2:111122223333:flow\-definition/flow\-definition\-name |  The Amazon Resource Number \(ARN\) of the human review workflow \(flow definition\) used to create the human loop\.   | 
| humanAnswers |  List of JSON objects  | <pre>{<br />"answerContent": {<br />    "AWS/Rekognition/DetectModerationLabels/Image/V3": {<br />        "moderationLabels": [...]<br />    }<br />},</pre> or<pre>{<br />    "answerContent": {<br />        "AWS/Textract/AnalyzeDocument/Forms/V1": {<br />            "blocks": [...]<br />    }<br />},</pre> | A list of JSON objects that contain worker responses in answerContent\. This object also contains submission details and, if a private workforce was used, worker metadata\. To learn more, see [Track Worker Activity](#a2i-worker-id-private)\. For human loop output data produced from Amazon Rekognition, `DetectModerationLabel` review tasks, this parameter only contains positive responses\. For example, if workers select *No content*, this response is not included\. | 
| humanLoopName |  String  |  `'human-loop-name'`  | The name of the human loop\. | 
| inputContent |  JSON object  |  <pre>{<br />    "aiServiceRequest": {...},<br />    "aiServiceResponse": {...},<br />    "humanTaskActivationConditionResults": {...},<br />    "selectedAiServiceResponse": {...}<br />}</pre>  |  The input content the AWS service sent to Amazon A2I when it requested a human loop be created\.   | 
| aiServiceRequest |  JSON object  | <pre>{<br />    "document": {...},<br />    "featureTypes": [...],<br />    "humanLoopConfig": {...}<br />}</pre>or <pre>{<br />    "image": {...},<br />    "humanLoopConfig": {...}<br />}</pre> |  The original request sent to the AWS service integrated with Amazon A2I\. For example, if you use Amazon Rekognition with Amazon A2I, this includes the request made through the API operation `DetectModerationLabels`\. For Amazon Textract integrations, this includes the request made through `AnalyzeDocument`\.  | 
| aiServiceResponse |  JSON object  |  <pre>{<br />    "moderationLabels": [...],<br />    "moderationModelVersion": "3.0"<br />}</pre> or <pre>{<br />    "blocks": [...],<br />    "documentMetadata": {}<br />}</pre>  |  The full response from the AWS service\. This is the data that is used to determine if a human review is required\. This object may contain metadata about the data object that is not shared with human reviewers\.   | 
| selectedAiServiceResponse |  JSON object  |  <pre>{<br />    "moderationLabels": [...],<br />    "moderationModelVersion": "3.0"<br />}</pre> or <pre>{<br />    "blocks": [...],<br />    "documentMetadata": {}<br />}</pre>  |  The subset of the `aiServiceResponse` that matches the activation conditions in `ActivationConditions`\. All data objects listed in `aiServiceResponse` are listed in `selectedAiServiceResponse` when inferences are randomly sampled, or all inferences triggered activation conditions\.  | 
| humanTaskActivationConditionResults |  JSON object  |  <pre>{<br />     "Conditions": [...]<br />}</pre>  |  A JSON object in `inputContent` that contains the reason a human loop was created\. This includes a list of the activation conditions \(`Conditions`\) included in your human review workflow \(flow definition\), and the evaluation result for each condition–this result is either `true` or `false`\. To learn more about activation conditions, see [JSON Schema for Human Loop Activation Conditions in Amazon Augmented AI](a2i-human-fallback-conditions-json-schema.md)\.  | 

Select a tab on the following table to learn about the task type\-specific parameters and see an example output\-data code block for each of the built\-in task types\.

------
#### [ Amazon Textract Task Type Output Data ]

When you use the Amazon Textract built\-in integration, you see `'AWS/Textract/AnalyzeDocument/Forms/V1'` as the value for `awsManagedHumanLoopRequestSource` in your output data\.

The `answerContent` parameter contains a `Block` object that includes human responses for all blocks sent to Amazon A2I\.

The `aiServiceResponse` parameter also includes a `Block` object with Amazon Textract's response to the original request sent using to `AnalyzeDocument`\.

To learn more about the parameters you see in the block object, refer to [Block](https://docs.aws.amazon.com/textract/latest/dg/API_Block.html) in the Amazon Textract Developer Guide\. 

The following is an example of the output data from an Amazon A2I human review of Amazon Textract document analysis inferences\. 

```
{
    "awsManagedHumanLoopRequestSource": "AWS/Textract/AnalyzeDocument/Forms/V1",
    "flowDefinitionArn": "arn:aws:sagemaker:us-west-2:111122223333:flow-definition/flow-definition-name",
    "humanAnswers": [
        {
            "answerContent": {
                "AWS/Textract/AnalyzeDocument/Forms/V1": {
                    "blocks": [...]
                }
            },
            "submissionTime": "2020-09-28T19:17:59.880Z",
            "workerId": "111122223333",
            "workerMetadata": {
                "identityData": {
                    "identityProviderType": "Cognito",
                    "issuer": "https://cognito-idp.us-west-2.amazonaws.com/us-west-2_111111",
                    "sub": "c6aa8eb7-9944-42e9-a6b9-111122223333"
                }
            }
        }
    ],
    "humanLoopName": "humnan-loop-name",
    "inputContent": {
        "aiServiceRequest": {
            "document": {
                "s3Object": {
                    "bucket": "DOC-EXAMPLE-BUCKET1",
                    "name": "document-demo.jpg"
                }
            },
            "featureTypes": [
                "TABLES",
                "FORMS"
            ],
            "humanLoopConfig": {
                "dataAttributes": {
                    "contentClassifiers": [
                        "FreeOfPersonallyIdentifiableInformation"
                    ]
                },
                "flowDefinitionArn": "arn:aws:sagemaker:us-west-2:111122223333:flow-definition/flow-definition-name",
                "humanLoopName": "humnan-loop-name"
            }
        },
        "aiServiceResponse": {
            "blocks": [...],
            "documentMetadata": {
                "pages": 1
            }
        },
        "humanTaskActivationConditionResults": {
            "Conditions": [
                {
                    "EvaluationResult": true,
                    "Or": [
                        {
                            "ConditionParameters": {
                                "ImportantFormKey": "Mail address",
                                "ImportantFormKeyAliases": [
                                    "Mail Address:",
                                    "Mail address:",
                                    "Mailing Add:",
                                    "Mailing Addresses"
                                ],
                                "KeyValueBlockConfidenceLessThan": 100,
                                "WordBlockConfidenceLessThan": 100
                            },
                            "ConditionType": "ImportantFormKeyConfidenceCheck",
                            "EvaluationResult": true
                        },
                        {
                            "ConditionParameters": {
                                "ImportantFormKey": "Mail address",
                                "ImportantFormKeyAliases": [
                                    "Mail Address:",
                                    "Mail address:",
                                    "Mailing Add:",
                                    "Mailing Addresses"
                                ]
                            },
                            "ConditionType": "MissingImportantFormKey",
                            "EvaluationResult": false
                        }
                    ]
                }
            ]
        },
        "selectedAiServiceResponse": {
            "blocks": [...]
        }
    }
}
```

------
#### [ Amazon Rekognition Task Type Output Data ]

When you use the Amazon Textract built\-in integration, you see the string `'AWS/Rekognition/DetectModerationLabels/Image/V3'` as the value for `awsManagedHumanLoopRequestSource` in your output data\.

The `answerContent` parameter contains a `moderationLabels` object that contains human responses for all moderation labels sent to Amazon A2I\.

The `aiServiceResponse` parameter also includes a `moderationLabels` object with Amazon Rekognition's response to the original request sent to `DetectModerationLabels`\.

To learn more about the parameters you see in the block object, refer to [ModerationLabel](https://docs.aws.amazon.com/rekognition/latest/dg/API_ModerationLabel.html) in the Amazon Rekognition Developer Guide\. 

The following is an example of the output data from an Amazon A2I human review of Amazon Rekognition image moderation inferences\. 

```
{
    "awsManagedHumanLoopRequestSource": "AWS/Rekognition/DetectModerationLabels/Image/V3",
    "flowDefinitionArn": "arn:aws:sagemaker:us-west-2:111122223333:flow-definition/flow-definition-name",
    "humanAnswers": [
        {
            "answerContent": {
                "AWS/Rekognition/DetectModerationLabels/Image/V3": {
                    "moderationLabels": [...]
                }
            },
            "submissionTime": "2020-09-28T19:22:35.508Z",
            "workerId": "ef7294f850a3d9d1",
            "workerMetadata": {
                "identityData": {
                    "identityProviderType": "Cognito",
                    "issuer": "https://cognito-idp.us-west-2.amazonaws.com/us-west-2_111111",
                    "sub": "c6aa8eb7-9944-42e9-a6b9-111122223333"
                }
            }
        }
    ],
    "humanLoopName": "humnan-loop-name",
    "inputContent": {
        "aiServiceRequest": {
            "humanLoopConfig": {
                "flowDefinitionArn": "arn:aws:sagemaker:us-west-2:111122223333:flow-definition/flow-definition-name",
                "humanLoopName": "humnan-loop-name"
            },
            "image": {
                "s3Object": {
                    "bucket": "DOC-EXAMPLE-BUCKET1",
                    "name": "example-image.jpg"
                }
            }
        },
        "aiServiceResponse": {
            "moderationLabels": [...],
            "moderationModelVersion": "3.0"
        },
        "humanTaskActivationConditionResults": {
            "Conditions": [
                {
                    "EvaluationResult": true,
                    "Or": [
                        {
                            "ConditionParameters": {
                                "ConfidenceLessThan": 98,
                                "ModerationLabelName": "Suggestive"
                            },
                            "ConditionType": "ModerationLabelConfidenceCheck",
                            "EvaluationResult": true
                        },
                        {
                            "ConditionParameters": {
                                "ConfidenceGreaterThan": 98,
                                "ModerationLabelName": "Female Swimwear Or Underwear"
                            },
                            "ConditionType": "ModerationLabelConfidenceCheck",
                            "EvaluationResult": false
                        }
                    ]
                }
            ]
        },
        "selectedAiServiceResponse": {
            "moderationLabels": [
                {
                    "confidence": 96.7122802734375,
                    "name": "Suggestive",
                    "parentName": ""
                }
            ],
            "moderationModelVersion": "3.0"
        }
    }
}
```

------

## Output Data From Custom Task Types<a name="sms-output-data-custom"></a>

When you add Amazon A2I to a custom human review workflow, you see the following parameters in the output data returned from human review tasks\. 


****  

| Parameter | Value Type | Description | 
| --- | --- | --- | 
|  `flowDefinitionArn`  |  String  |  The Amazon Resource Number \(ARN\) of the human review workflow \(flow definition\) used to create the human loop\.   | 
|  `humanAnswers`  |  List of JSON objects  | A list of JSON objects that contain worker responses in answerContent\. The value in this parameter is determined by the output received from your [worker task template](https://docs.aws.amazon.com/sagemaker/latest/dg/a2i-instructions-overview.html)\. If a private workforce is used, worker metadata is included\. To learn more, see [Track Worker Activity](#a2i-worker-id-private)\. | 
|  `humanLoopName`  | String | The name of the human loop\. | 
|  `inputContent`  |  JSON Object  |  The input content sent to Amazon A2I in the request to [https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_StartHumanLoop.html](https://docs.aws.amazon.com/augmented-ai/2019-11-07/APIReference/API_StartHumanLoop.html)\.   | 

The following is an example of output data from a custom integration with Amazon A2I and Amazon Transcribe\. In this example, the `inputContent` consists of:
+ A path to an \.mp4 file in Amazon S3 and the video title
+ The transcription returned from Amazon Transcribe \(parsed from Amazon Transcribe output data\)
+ A start and end time used by the worker task template to clip the \.mp4 file and show workers a relevant portion of the video

```
{
    "flowDefinitionArn": "arn:aws:sagemaker:us-west-2:111122223333:flow-definition/flow-definition-name",
    "humanAnswers": [
        {
            "answerContent": {
                "transcription": "use lambda to turn your notebook"
            },
            "submissionTime": "2020-06-18T17:08:26.246Z",
            "workerId": "ef7294f850a3d9d1",
            "workerMetadata": {
                "identityData": {
                    "identityProviderType": "Cognito",
                    "issuer": "https://cognito-idp.us-west-2.amazonaws.com/us-west-2_111111",
                    "sub": "c6aa8eb7-9944-42e9-a6b9-111122223333"
                }
            }

        }
    ],
    "humanLoopName": "human-loop-name",
    "inputContent": {
        "audioPath": "s3://DOC-EXAMPLE-BUCKET1/a2i_transcribe_demo/Fully-Managed Notebook Instances with Amazon SageMaker - a Deep Dive.mp4",
        "end_time": 950.27,
        "original_words": "but definitely use Lambda to turn your ",
        "start_time": 948.51,
        "video_title": "Fully-Managed Notebook Instances with Amazon SageMaker - a Deep Dive.mp4"
    }
}
```

## Track Worker Activity<a name="a2i-worker-id-private"></a>

Amazon A2I provides information that you can use to track individual workers in task output data\. To identify the worker that worked on the human review task, use the following from the output data in Amazon S3:
+ The `acceptanceTime` is the time that the worker accepted the task\. The format of this date and time stamp is `YYYY-MM-DDTHH:MM:SS.mmmZ` for the year \(`YYYY`\), month \(`MM`\), day \(`DD`\), hour \(`HH`\), minute \(`MM`\), second \(`SS`\) and millisecond \(`mmm`\)\. The date and time are separated by a **T**\. 
+ The `submissionTime` is the time that the worker submitted their annotations using the **Submit** button\. The format of this date and time stamp is `YYYY-MM-DDTHH:MM:SS.mmmZ` for the year \(`YYYY`\), month \(`MM`\), day \(`DD`\), hour \(`HH`\), minute \(`MM`\), second \(`SS`\) and millisecond \(`mmm`\)\. The date and time are separated by a **T**\. 
+ `timeSpentInSeconds` reports the total time, in seconds, that a worker actively worked on that task\. This metric does not include time when a worker paused or took a break\.
+ The `workerId` is unique to each worker\. 
+ If you use a [private workforce](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-workforce-private.html), in `workerMetadata`, you see the following\.
  + The `identityProviderType` is the service used to manage the private workforce\. 
  + The `issuer` is the Cognito user pool or OIDC Identity Provider \(IdP\) issuer associated with the work team assigned to this human review task\.
  + A unique `sub` identifier refers to the worker\. If you create a workforce using Amazon Cognito, you can retrieve details about this worker \(such as the name or user name\) using this ID using Amazon Cognito\. To learn how, see [Managing and Searching for User Accounts](https://docs.aws.amazon.com/cognito/latest/developerguide/how-to-manage-user-accounts.html#manage-user-accounts-searching-user-attributes) in [Amazon Cognito Developer Guide](https://docs.aws.amazon.com/cognito/latest/developerguide/)\.

The following is an example of the output you may see if you use Amazon Cognito to create a private workforce\. This is identified in the `identityProviderType`\.

```
"submissionTime": "2020-12-28T18:59:58.321Z",
"acceptanceTime": "2020-12-28T18:59:15.191Z", 
"timeSpentInSeconds": 40.543,
"workerId": "a12b3cdefg4h5i67",
"workerMetadata": {
    "identityData": {
        "identityProviderType": "Cognito",
        "issuer": "https://cognito-idp.aws-region.amazonaws.com/aws-region_123456789",
        "sub": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee"
    }
}
```

 The following is an example of the output you may see if you use your own OIDC IdP to create a private workforce:

```
"workerMetadata": {
        "identityData": {
            "identityProviderType": "Oidc",
            "issuer": "https://example-oidc-ipd.com/adfs",
            "sub": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee"
        }
}
```

To learn more about using private workforces, see [Use a Private Workforce](sms-workforce-private.md)\.