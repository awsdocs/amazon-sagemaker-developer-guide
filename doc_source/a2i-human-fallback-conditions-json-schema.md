# JSON Schema for Human Loop Activation Conditions in Amazon Augmented AI<a name="a2i-human-fallback-conditions-json-schema"></a>

The `HumanLoopActivationConditions` is an input parameter of the [ `CreateFlowDefinition`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateFlowDefinition.html) API\. This parameter is a JSON\-formatted string\. The JSON models the conditions under which a human loop is created, when those conditions are evaluated against the response from an integrating AI service API \(such as `Rekognition.DetectModerationLabels` or `Textract.AnalyzeDocument`\)\. This response is referred to as an *inference*\. For example, Amazon Rekognition sends an inference of a moderation label with an associated confidence score\. In this example, the inference is the model's best estimate of the appropriate label for an image\. For Amazon Textract, inference is made on the association between blocks of text \(*key\-value pairs*\), such as the association between `Name:` and `Sue` in a form as well as content within a block of text, or *word block*, such as 'Name'\.

The following is the schema for the JSON\. At the top level, the `HumanLoopActivationConditions` has a JSON array, `Conditions`\. Each member of this array is an independent condition that, if evaluated to true, will result in Amazon A2I creating a human loop\. Each such independent condition can be a primitive condition or a complex condition\. A simple condition has the following attributes:
+ `ConditionType`: This attribute identifies the type of condition\. Each AWS AI service API that integrates with Amazon A2I defines its own set of allowed `ConditionTypes`\. 
  + Rekognition `DetectModerationLabels` – This API supports the `ModerationLabelConfidenceCheck` and `Sampling` `ConditionType` values\.
  + Textract `AnalyzeDocument` – This API supports the `ImportantFormKeyConfidenceCheck`, `MissingImportantFormKey` and `Sampling` `ConditionType` values\.
+ `ConditionParameters` – This is a JSON object that parameterizes the condition\. The set of allowed attributes of this object is dependent on the value of the `ConditionType`\. Each `ConditionType` defines its own set of `ConditionParameters`\. 

A member of the `Conditions` array can model a complex condition\. This is accomplished by logically connecting primitive conditions using the `And` and `Or` logical operators and nesting the underlying primitive conditions\. Up to two levels of nesting are supported\. 

```
{
    "$schema": "http://json-schema.org/draft-07/schema#",
    "definitions": {
        "Condition": {
            "type": "object",
            "properties": {
                "ConditionType": {
                    "type": "string"
                },
                "ConditionParameters": {
                    "type": "object"
                }
            },
            "required": [
                "ConditionType"
            ]
        },
        "OrConditionArray": {
            "type": "object",
            "properties": {
                "Or": {
                    "type": "array",
                    "minItems": 2,
                    "items": {
                        "$ref": "#/definitions/ComplexCondition"
                    }
                }
            }
        },
        "AndConditionArray": {
            "type": "object",
            "properties": {
                "And": {
                    "type": "array",
                    "minItems": 2,
                    "items": {
                        "$ref": "#/definitions/ComplexCondition"
                    }
                }
            }
        },
        "ComplexCondition": {
            "anyOf": [
                {
                    "$ref": "#/definitions/Condition"
                },
                {
                    "$ref": "#/definitions/OrConditionArray"
                },
                {
                    "$ref": "#/definitions/AndConditionArray"
                }
            ]
        }
    },
    "type": "object",
    "properties": {
        "Conditions": {
            "type": "array",
            "items": {
                "$ref": "#/definitions/ComplexCondition"
            }
        }
    }
}
```

**Note**  
Human loop activation conditions aren't available for human review workflows that are integrated with custom task types\. The `HumanLoopActivationConditions` parameter is disabled for custom task types\. 

**Topics**
+ [Use Human Loop Activation Conditions JSON Schema with Amazon Textract](a2i-json-humantaskactivationconditions-textract-example.md)
+ [Use Human Loop Activation Conditions JSON Schema with Amazon Rekognition](a2i-json-humantaskactivationconditions-rekognition-example.md)