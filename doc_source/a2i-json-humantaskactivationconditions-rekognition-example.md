# Use Human Loop Activation Conditions JSON Schema with Amazon Rekognition<a name="a2i-json-humantaskactivationconditions-rekognition-example"></a>

When used with Amazon A2I, the Amazon Rekognition `DetectModerationLabels` operation supports the following inputs in the `ConditionType` parameters:
+ `ModerationLabelConfidenceCheck` – Use this condition type to create a human loop when inference confidence is low for one or more specified labels\.
+ `Sampling` – Use this condition to specify a percentage of all inferences to send to humans for review\. Use this condition to do the following:
  + Audit your ML model by randomly sampling all of your model's inferences and sending a specified percentage to humans for review\.
  + Using the `ModerationLabelConfidenceCheck` condition, randomly sample a percentage of the inferences that met the conditions specified in `ModerationLabelConfidenceCheck` to start a human loop and send only the specified percentage to humans for review\. 

**Note**  
If you send the same request to `DetectModerationLabels` multiple times, the result of `Sampling` will not change for the inference of that input\. For example, if you make a `DetectModerationLabels` request once, and `Sampling` does not trigger a HumanLoop, subsequent requests to `DetectModerationLabels` with the same configuration won't trigger a human loop\. 

When creating a flow definition, if you use the default worker task template that is provided in the **Human review workflows** section of the Amazon SageMaker console, inferences sent for human review by these activation conditions are included in the worker UI when a worker opens your task\. If you use a custom worker task template, you need to include the `<task.input.selectedAiServiceResponse.blocks>` custom HTML element to access these inferences\. For an example of a custom template that uses this HTML element, see [Custom Template Example for Amazon Rekognition](a2i-custom-templates.md#a2i-custom-templates-rekognition-sample)\.

## ModerationLabelConfidenceCheck Inputs<a name="a2i-rek-moderationlabelconfidencecheck"></a>

For the `ModerationLabelConfidenceCheck` `ConditionType`, the following `ConditionParameters` are supported:
+ `ModerationLabelName` – The exact \(case\-sensitive\) name of a [ModerationLabel](https://docs.aws.amazon.com/rekognition/latest/dg/API_ModerationLabel.html) detected by the Amazon Rekognition `DetectModerationLabels` operation\. You can specify the special catch\-all value \(\*\) to denote any moderation label\.
+ `ConfidenceEquals`
+ `ConfidenceLessThan`
+ `ConfidenceLessThanEquals`
+ `ConfidenceGreaterThan`
+ `ConfidenceGreaterThanEquals`

When you use the `ModerationLabelConfidenceCheck` `ConditionType`, Amazon A2I sends label inferences for the labels that you specified in `ModerationLabelName` for human review\.

## Sampling Inputs<a name="a2i-rek-randomsamplingpercentage"></a>

The `Sampling` `ConditionType` supports the `RandomSamplingPercentage` `ConditionParameters`\. The input for the `RandomSamplingPercentage` prameter should be real number between 0\.01 and 100\. This number represents the percentage of inferences that qualifies for a human review that are sent to humans for review\. If you use the `Sampling` condition without any other conditions, this number represents the percentage of all inferences that result from a single `DetectModerationLabel` request that are sent to humans for review\.

## Examples<a name="a2i-json-rek-activation-condition-examples"></a>

**Example 1: Use ModerationLabelConfidenceCheck with the And operator**

The following example of a `HumanLoopActivationConditions` condition triggers a HumanLoop when one or more of the following conditions are met:
+ Amazon Rekognition detects the `Graphic Male Nudity` moderation label with a confidence between 90 and 99\.
+ Amazon Rekognition detects the `Graphic Female Nudity` moderation label with a confidence between 80 and 99\.

Note the use of the Or and And logical operators to model this logic\.

Although only one of the two conditions under the `Or` operator need to evaluate to true for a HumanLoop to be created, Amazon Augmented AI evaluates all conditions\. Human reviewers are asked to review the moderation labels for all the conditions that evaluated to true\.

```
{
     "Conditions": [{
         "Or": [{
                 "And": [{
                         "ConditionType": "ModerationLabelConfidenceCheck",
                         "ConditionParameters": {
                             "ModerationLabelName": "Graphic Male Nudity",
                             "ConfidenceLessThanOrEqual": 99
                         }
                     },
                     {
                         "ConditionType": "ModerationLabelConfidenceCheck",
                         "ConditionParameters": {
                             "ModerationLabelName": "Graphic Male Nudity",
                             "ConfidenceGreaterThanOrEqual": 90
                         }
                     }
                 ]
             },
             {
                 "And": [{
                         "ConditionType": "ModerationLabelConfidenceCheck",
                         "ConditionParameters": {
                             "ModerationLabelName": "Graphic Female Nudity",
                             "ConfidenceLessThanOrEqual": 99
                         }
                     },
                     {
                         "ConditionType": "ModerationLabelConfidenceCheck",
                         "ConditionParameters": {
                             "ModerationLabelName": "Graphic Female Nudity",
                             "ConfidenceGreaterThanOrEqual": 80
                         }
                     }
                 ]
             }
         ]
     }]
}
```

**Example 2: Use ModerationLabelConfidenceCheck with the catch\-all value \(\*\) **

In the following example, if any moderation label with a confidence greater than or equal to 75 is detected, a HumanLoop is triggered\. Human reviewers are asked to review all moderation labels with confidence scores greater than or equal to 75\.

```
{
    "Conditions": [
        {
            "ConditionType": "ModerationLabelConfidenceCheck",
            "ConditionParameters": {
                "ModerationLabelName": "*",
                "ConfidenceGreaterThanOrEqual": 75
            }
        }
    ]
}
```

**Example 3: Use Sampling**

In the following example, 5% of Amazon Rekognition inferences from a `DetectModerationLabels` request will be sent to human workers\. When using the default worker task template provided in the SageMaker console, all moderation labels returned by Amazon Rekognition are sent to workers for review\.

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

**Example 4: Use Sampling and ModerationLabelConfidenceCheck with the And operator**

In this example, 5% of Amazon Rekognition inferences of the `Graphic Male Nudity` moderation label with a confidence greater than 50 will be sent workers for review\. When using the default worker task template provided in the SageMaker console, only the `Graphic Male Nudity` label will be sent to workers for review\. 

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
          "ConditionType": "ModerationLabelConfidenceCheck",
          "ConditionParameters": {
            "ModerationLabelName": "Graphic Male Nudity",
            "ConfidenceGreaterThan": 50
          }
        }
      ]
    }
  ]
}
```

**Example 5: Use Sampling and ModerationLabelConfidenceCheck with the And operator**

Use this example to configure your human review workflow to always send low confidence inferences of a specified label for human review and sample high confidence inference of a label at a specified rate\. 

In the following example, a human review is triggered in one of the following ways: 
+ Inferences for the `Graphic Male Nudity` moderation label the with confidence scores less than 60 are always sent for human review\. Only the `Graphic Male Nudity` label is sent to workers to review\. 
+ 5% of all inferences for the `Graphic Male Nudity` moderation label the with confidence scores greater than 90 will be sent for human review\. Only the `Graphic Male Nudity` label is sent to workers to review\. 

```
{
  "Conditions": [
    {
      "Or": [
        {
          "ConditionType": "ModerationLabelConfidenceCheck",
          "ConditionParameters": {
            "ModerationLabelName": "Graphic Male Nudity",
            "ConfidenceLessThan": 60
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
                    "ConditionType": "ModerationLabelConfidenceCheck",
                    "ConditionParameters": {
                        "ModerationLabelName": "Graphic Male Nudity",
                        "ConfidenceGreaterThan": 90
                    }
                }
            ]
        }
      ]
    }
  ]
}
```

**Example 6: Use Sampling and ModerationLabelConfidenceCheck with the Or operator**

In the following example, a human loop is created if the Amazon Rekognition inference response contains the 'Graphic Male Nudity' label with inference confidence greater than 50\. Additionally, 5% of all other inferences will trigger a human loop\. 

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
          "ConditionType": "ModerationLabelConfidenceCheck",
          "ConditionParameters": {
            "ModerationLabelName": "Graphic Male Nudity",
            "ConfidenceGreaterThan": 50
          }
        }
      ]
    }
  ]
}
```