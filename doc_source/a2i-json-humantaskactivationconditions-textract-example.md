# Use Human Loop Activation Conditions JSON Schema with Amazon Textract<a name="a2i-json-humantaskactivationconditions-textract-example"></a>

When used with Amazon A2I, the `AnalyzeDocument` operation supports the following inputs in the `ConditionType` parameter:
+ `ImportantFormKeyConfidenceCheck` – Use this condition to create a human loop when inference confidence is within a specified range for document form keys and word blocks\. A *form key* is any word in a document that is associated with an input\. The input is called a *value*\. Together, form keys and values are referred to as *key\-value pairs*\. A *word block* refers to the words that Amazon Textract recognizes inside of a detected block of text\. To learn more about Amazon Textract document blocks, see [Documents and Block Objects](https://docs.aws.amazon.com/textract/latest/dg/how-it-works-document-layout.html) in the *Amazon Textract Developer Guide*\.
+ `MissingImportantFormKey` – Use this condition to create a human loop when Textract did not identify the key or its associated aliases within the document\. 
+ `Sampling` – Use this condition to specify a percentage of forms to send to humans for review, regardless of inference confidence scores\. Use this condition to do the following:
  + Audit your ML model by randomly sampling all forms analyzed by your model and sending a specified percentage to humans for review\.
  + Using the `ImportantFormKeyConfidenceCheck` condition, randomly sample a percentage of the inferences that met the conditions specified in `ImportantFormKeyConfidenceCheck` to start a human loop and send only the specified percentage to humans for review\. 

**Note**  
If you send the same request to `AnalyzeDocument` multiple times, the result of `Sampling` will not change for the inference of that input\. For example, if you make an `AnalyzeDocument` request once, and `Sampling` doesn't trigger a HumanLoop, subsequent requests to `AnalyzeDocument` with the same configuration will not trigger a human loop\.

## ImportantFormKeyConfidenceCheck Inputs and Results<a name="a2i-textract-importantformkeycofidencecheck"></a>

The `ImportantFormKeyConfidenceCheck` `ConditionType` supports the following `ConditionParameters`:
+ `ImportantFormKey` – A string representing a key in a key\-value pair detected by Amazon Textract that needs to be reviewed by human workers\. If the value of this parameter is the special catch\-all value \(\*\), then all keys are considered to be matched to the condition\. You can use this to model the case where any key\-value pair satisfying certain confidence thresholds needs human review\.
+ `ImportantFormKeyAliases` – An array that represents alternate spellings or logical equivalents for the important form key\. 
+ `KeyValueBlockConfidenceEquals`
+ `KeyValueBlockConfidenceLessThan`
+ `KeyValueBlockConfidenceLessThanEquals`
+ `KeyValueBlockConfidenceGreaterThan`
+ `KeyValueBlockConfidenceGreaterThanEquals`
+ `WordBlockConfidenceEquals`
+ `WordBlockConfidenceLessThan`
+ `WordBlockConfidenceLessThanEquals`
+ `WordBlockConfidenceGreaterThan`
+ `WordBlockConfidenceGreaterThanEquals`

When you use the `ImportantFormKeyConfidenceCheck` `ConditionType`, Amazon A2I sends the key\-value block and word block inferences of the key\-value blocks and associated aliases that you specified in `ImportantFormKey` and `ImportantFormKeyAliases` for human review\.

When creating a flow definition, if you use the default worker task template that is provided in the **Human review workflows** section of the Amazon SageMaker console, key\-value and block inferences sent for human review by this activation condition are included in the worker UI\. If you use a custom worker task template, you need to include the `{{ task.input.selectedAiServiceResponse.blocks }}` element to include initial\-value input data \(inferences\) from Amazon Textract\. For an example of a custom template that uses this input element, see [Custom Template Example for Amazon Textract](a2i-custom-templates.md#a2i-custom-templates-textract-sample)\.

## MissingImportantFormKey Inputs and Results<a name="a2i-textract-missingimportantformkey"></a>

The `MissingImportantFormKey` `ConditionType` supports the following `ConditionParameters`:
+ `ImportantFormKey` – A string representing a key in a key\-value pair detected by Amazon Textract that needs to be reviewed by human workers\.
+ `ImportantFormKeyAliases` – An array that represents alternate spellings or logical equivalents for the important form key\. 

When you use the `MissingImportantFormKey` `ConditionType`, if the key in `ImportantFormKey` or aliases in `ImportantFormKeyAliases` are not included in Amazon Textract inference, that form will be sent to human for review and no predicted key\-value pairs will be included\. For example, if Amazon Textract only identified `Address` and `Phone` in a form, but was missing the `ImportantFormKey` `Name` \(in the `MissingImportantFormKey` condition type\) that form would be sent to humans for review without any of the form keys detected \(`Address` and `Phone`\)\.

If you use the default worker task template that is provided in the SageMaker console, a task will be created asking workers identify the key in `ImportantFormKey` and associated value\. If you use a custom worker task template, you need to include the `<task.input.humanLoopContext>` custom HTML element to configure this task\. 

## Sampling Inputs and Results<a name="a2i-textract-randomsamplingpercentage"></a>

The `Sampling` `ConditionType` supports the `RandomSamplingPercentage` `ConditionParameters`\. The input for `RandomSamplingPercentage` must be real number between 0\.01 and 100\. This number represents the percentage of data that qualifies for a human review and will be sent to humans for review\. If you use the `Sampling` condition without any other conditions, this number represents the percentage of all resulting inferences made by the `AnalyzeDocument` operation from a single request that will be sent to humans for review\.

If you specify the `Sampling` condition without any other condition type, all key\-value and block inferences are sent to workers for review\. 

When creating a flow definition, if you use the default worker task template that is provided in the **Human review workflows** section of the SageMaker console, all key\-value and block inferences sent for human review by this activation condition are included in the worker UI\. If you use a custom worker task template, you need to include the `{{ task.input.selectedAiServiceResponse.blocks }}` element to include initial\-value input data \(inferences\) from Amazon Textract\. For an example of a custom template that uses this input element, see [Custom Template Example for Amazon Textract](a2i-custom-templates.md#a2i-custom-templates-textract-sample)\.

## Examples<a name="a2i-json-activation-condition-examples"></a>

While only one condition needs to evaluate to true to trigger a human loop, Amazon A2I will evaluate all conditions for each object analyzed by Amazon Textract\. The human reviewers are asked to review the important form keys for all the conditions that evaluated to true\.

**Example 1: Detect important form keys with confidence scores in a specified range trigger HumanLoop**

Following is an example of a `HumanLoopActivationConditions` JSON that triggers a HumanLoop if any one of the following three conditions is met:
+ Textract AnalyzeDocument API returns a key\-value pair whose key is one of `Employee Name`, `Name`, or `EmployeeName`, with the confidence of the key\-value block being less than 60 and the confidences of each of the word blocks making up the key and value being less than 85\.
+ Textract AnalyzeDocument API returns a key\-value pair whose key is one of `Pay Date`, `PayDate`, `DateOfPay`, or `pay-date`, with the confidence of the key\-value block being less than 65 and the confidences of each of the Word blocks making up the key and value being less than 85\.
+ Textract AnalyzeDocument API returns a key\-value pair whose key is one of `Gross Pay`, `GrossPay`, or `GrossAmount`, with the confidence of the key\-value block being less than 60 and the confidences of each of the word blocks making up the key and value being less than 85\.

```
{
    "Conditions": [
        {
            "ConditionType": "ImportantFormKeyConfidenceCheck",
            "ConditionParameters": {
                "ImportantFormKey": "Employee Name",
                "ImportantFormKeyAliases": [
                    "Name",
                    "EmployeeName"
                ],
                "KeyValueBlockConfidenceLessThan": 60,
                "WordBlockConfidenceLessThan": 85
            }
        },
        {
            "ConditionType": "ImportantFormKeyConfidenceCheck",
            "ConditionParameters": {
                "ImportantFormKey": "Pay Date",
                "ImportantFormKeyAliases": [
                    "PayDate",
                    "DateOfPay",
                    "pay-date"
                ],
                "KeyValueBlockConfidenceLessThan": 65,
                "WordBlockConfidenceLessThan": 85
            }
        },
        {
            "ConditionType": "ImportantFormKeyConfidenceCheck",
            "ConditionParameters": {
                "ImportantFormKey": "Gross Pay",
                "ImportantFormKeyAliases": [
                    "GrossPay",
                    "GrossAmount"
                ],
                "KeyValueBlockConfidenceLessThan": 60,
                "WordBlockConfidenceLessThan": 85
            }
        }
    ]
}
```

**Example 2: Use ImportantFormKeyConfidenceCheck**

In the following example, if Amazon Textract detects a key\-value pair whose confidence for the key\-value block is less than 60 and is less than 90 for any underlying word blocks, a HumanLoop is created\. The human reviewers are asked to review all the form key\-value pairs that matched the confidence value comparisons\.

```
{
    "Conditions": [
        {
            "ConditionType": "ImportantFormKeyConfidenceCheck",
            "ConditionParameters": {
                "ImportantFormKey": "*"
                "KeyValueBlockConfidenceLessThan": 60,
                "WordBlockConfidenceLessThan": 90
            }
        }
    ]
}
```

**Example 3: Use Sampling**

In the following example, 5% of inferences resulting from an Amazon Textract `AnalyzeDocument` request will be sent to human workers for review\. All detected key\-value pairs returned by Amazon Textract are sent to workers for review\.

```
{
  "Conditions": [
    {
      "ConditionType": "Sampling",
      "ConditionParameters": {
        "RandomSamplingPercentage": 5
      }
    }
  ]
}
```

**Example 4: Use MissingImportantFormKey**

In the following example, if `Mailing Address` or its alias, `Mailing Address:` is missing from keys detected by Amazon Textract, a human review will be triggered\. When using the default worker task template, the worker UI asks workers to identify the key `Mailing Address` or `Mailing Address:` and its associated value\. 

```
{
    "ConditionType": "MissingImportantFormKey",
    "ConditionParameters": {
        "ImportantFormKey": "Mailing Address",
        "ImportantFormKeyAliases": ["Mailing Address:"]
    }
}
```

**Example 5: Use Sampling and ImportantFormKeyConfidenceCheck with the And operator**

In this example, 5% of key\-value pairs detected by Amazon Textract whose key is one of `Pay Date`, `PayDate`, `DateOfPay`, or `pay-date`, with the confidence of the key\-value block less than 65 and the confidences of each of the word blocks making up the key and value less than 85 are sent to workers for review\.

```
{
  "Conditions": [
    {
      "And": [
        {
          "ConditionType": "Sampling",
          "ConditionParameters": {
            "RandomSamplingPercentage": 5
          }
        },
        {
          "ConditionType": "ImportantFormKeyConfidenceCheck",
            "ConditionParameters": {
                "ImportantFormKey": "Pay Date",
                "ImportantFormKeyAliases": [
                    "PayDate",
                    "DateOfPay",
                    "pay-date"
                ],
                "KeyValueBlockConfidenceLessThan": 65,
                "WordBlockConfidenceLessThan": 85
            }
        }
      ]
    }
  ]
}
```

**Example 6: Use Sampling and ImportantFormKeyConfidenceCheck with the And operator**

Use this example to configure your human review workflow to always send low confidence inferences of a specified key\-value pair for human review and sample high confidence inference of a key\-value pair at a specified rate\. 

In the following example, a human review is triggered in one of the following ways: 
+ Key\-value pairs detected whose key is one of `Pay Date`, `PayDate`, `DateOfPay`, or `pay-date`, with key\-value and word block confidences less than 60 will be sent for human review\. Only the `Pay Date` form key \(and its aliases\) and associated values are sent to workers to review\. 
+ 5% of key\-value pairs detected whose key is one of `Pay Date`, `PayDate`, `DateOfPay`, or `pay-date`, with key\-value and word block confidences greater than 90 will be sent for human review\. Only the `Pay Date` form key \(and its aliases\) and associated values are sent to workers to review\. 

```
{
  "Conditions": [
    {
      "Or": [
       {
          "ConditionType": "ImportantFormKeyConfidenceCheck",
            "ConditionParameters": {
                "ImportantFormKey": "Pay Date",
                "ImportantFormKeyAliases": [
                    "PayDate",
                    "DateOfPay",
                    "pay-date"
                ],
                "KeyValueBlockConfidenceLessThan": 60,
                "WordBlockConfidenceLessThan": 60
            }
        },
        {
            "And": [
                {
                    "ConditionType": "Sampling",
                    "ConditionParameters": {
                        "RandomSamplingPercentage": 5
                    }
                },
                {
                    "ConditionType": "ImportantFormKeyConfidenceCheck",
                        "ConditionParameters": {
                            "ImportantFormKey": "Pay Date",
                            "ImportantFormKeyAliases": [
                                "PayDate",
                                "DateOfPay",
                                "pay-date"
                        ],
                        "KeyValueBlockConfidenceLessThan": 90
                        "WordBlockConfidenceGreaterThan": 90
                    }
                }
            ]
        }
      ]
    }
  ]
}
```

**Example 7: Use Sampling and ImportantFormKeyConfidenceCheck with the Or operator**

In the following example, the Amazon Textract `AnalyzeDocument` operation returns a key\-value pair whose key is one of `Pay Date`, `PayDate`, `DateOfPay`, or `pay-date`, with the confidence of the key\-value block less than 65 and the confidences of each of the word blocks making up the key and value less than 85\. Additionally, 5% of all other forms will trigger a human loop\. For each form randomly chosen, all key\-value pairs detected for that form will be sent to humans for review\.

```
{
  "Conditions": [
    {
      "Or": [
        {
          "ConditionType": "Sampling",
          "ConditionParameters": {
            "RandomSamplingPercentage": 5
          }
        },
        {
           "ConditionType": "ImportantFormKeyConfidenceCheck",
            "ConditionParameters": {
                "ImportantFormKey": "Pay Date",
                "ImportantFormKeyAliases": [
                    "PayDate",
                    "DateOfPay",
                    "pay-date"
                ],
                "KeyValueBlockConfidenceLessThan": 65,
                "WordBlockConfidenceLessThan": 85
            }
          }
        }
      ]
    }
  ]
}
```