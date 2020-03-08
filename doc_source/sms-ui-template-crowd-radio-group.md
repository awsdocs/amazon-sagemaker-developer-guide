# crowd\-radio\-group<a name="sms-ui-template-crowd-radio-group"></a>

A group of radio buttons\. Only one radio button within the group can be selected\. Choosing one radio button clears any previously chosen radio button within the same group\. For an example of a custom UI template that uses the `crowd-radio-group` element, see this [entity recognition labeling job custom template](https://github.com/aws-samples/amazon-sagemaker-ground-truth-task-uis/blob/master/text/named-entity-recognition-with-additional-classification.liquid.html)\.

### Attributes<a name="radio-group-attributes"></a>

No special attributes are supported by this element\.

### Element Hierarchy<a name="radio-group-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Parent elements**: [crowd\-form](sms-ui-template-crowd-form.md)
+ **Child elements**: [crowd\-radio\-button](sms-ui-template-crowd-radio-button.md)

### Output<a name="radio-group-output"></a>

Outputs an array of objects representing the [crowd\-radio\-button](sms-ui-template-crowd-radio-button.md) elements within it\.

**Example Sample of Element Output**  

```
[
  {
    "btn1": {
      "yes": true
    },
    "btn2": {
      "no": false
    }
  }
]
```

### See Also<a name="radio-group-see-also"></a>

For more information, see the following\.
+ [Use Amazon SageMaker Ground Truth for Labeling](sms.md)
+ [HTML Elements Reference](sms-ui-template-reference.md)