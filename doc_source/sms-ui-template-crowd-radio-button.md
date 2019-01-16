# crowd\-radio\-button<a name="sms-ui-template-crowd-radio-button"></a>

A button that can be either checked or unchecked\. When radio buttons are inside a radio group, exactly one radio button in the group can be checked at any time\.

## Attributes<a name="radio-button-attributes"></a>

The following attributes are supported by this element\.

### checked<a name="radio-button-attributes-checked"></a>

A Boolean switch that, if present, displays the radio button as checked\.

### disabled<a name="radio-button-attributes-disabled"></a>

A Boolean switch that, if present, displays the button as disabled and prevents it from being checked\.

### name<a name="radio-button-attributes-name"></a>

A string that is used to identify the answer submitted by the worker\. This value will match a key in the JSON object that specifies the answer\.

### required<a name="radio-button-attributes-required"></a>

A Boolean switch that, if present, requires the worker to provide input\.

### value<a name="radio-button-attributes-value"></a>

A property name for the element's boolean value\. If not specified, it uses "on" as the default, e\.g\. `{ "<name>": { "<value>": <true or false> } }`\.

## Element Hierarchy<a name="radio-button-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Parent elements**: [crowd\-radio\-group](sms-ui-template-crowd-radio-group.md)
+ **Child elements**: none

## Output<a name="radio-button-output"></a>

Outputs an object with the following pattern: `{ "<name>": { "<value>": <true or false> } }`\. If you use the buttons outside of a [crowd\-radio\-group](sms-ui-template-crowd-radio-group.md) element, but with the same `name` string and different `value` strings, the name object will contain a Boolean value for each `value` string\. To ensure that only one in a group of buttons is selected, make them children of a [crowd\-radio\-group](sms-ui-template-crowd-radio-group.md) element and use different name values\.

**Example Sample output of this element**  

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

## See Also<a name="radio-button-see-also"></a>

For more information, see the following\.
+ [Amazon SageMaker Ground Truth](sms.md)
+ [HTML Elements Reference](sms-ui-template-reference.md)