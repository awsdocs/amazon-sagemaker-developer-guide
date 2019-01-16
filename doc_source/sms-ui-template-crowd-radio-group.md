# crowd\-radio\-group<a name="sms-ui-template-crowd-radio-group"></a>

A group of radio buttons\. Only one radio button within the group can be selected\. Choosing one radio button clears any previously chosen radio button within the same group\.

## Attributes<a name="radio-group-attributes"></a>

The following attributes are supported by this element\.

### allow\-empty\-selection<a name="radio-group-attributes-allow-empty-selection"></a>

A Boolean switch that, if present, allows no radio button to be checked\.

### disabled<a name="radio-group-attributes-disabled"></a>

A Boolean switch that, if present, displays the radio group as disabled and prevents buttons in the group from being checked\.

## Element Hierarchy<a name="radio-group-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Parent elements**: [crowd\-form](sms-ui-template-crowd-form.md)
+ **Child elements**: [crowd\-radio\-button](sms-ui-template-crowd-radio-button.md)

## Output<a name="radio-group-output"></a>

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

## See Also<a name="radio-group-see-also"></a>

For more information, see the following\.
+ [Amazon SageMaker Ground Truth](sms.md)
+ [HTML Elements Reference](sms-ui-template-reference.md)