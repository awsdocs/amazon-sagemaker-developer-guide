# crowd\-radio\-button<a name="sms-ui-template-crowd-radio-button"></a>

A button that can be either checked or unchecked\. When radio buttons are inside a radio group, exactly one radio button in the group can be checked at any time\. The following is an example of how to configure a `crowd-radio-button` element inside of a `crowd-radio-group` element\.

The following is an example of the syntax that you can use with the `<crowd-radio-button>` element\. Copy the following code and save it in a file with the extenion `.html`\. Open the file in any browser to preview and interact with this template\. 

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>
<crowd-form>
<crowd-radio-group>
    <crowd-radio-button name="tech" value="tech">Technology</crowd-radio-button>
    <crowd-radio-button name="politics" value="politics">Politics</crowd-radio-button>
</crowd-form>
</crowd-radio-group>
```

The previous example can be seen in a custom worker task template in this GitHub example: [entity recognition labeling job custom template](https://github.com/aws-samples/amazon-sagemaker-ground-truth-task-uis/blob/master/text/named-entity-recognition-with-additional-classification.liquid.html)\.

### Attributes<a name="radio-button-attributes"></a>

The following attributes are supported by this element\.

#### checked<a name="radio-button-attributes-checked"></a>

A Boolean switch that, if present, displays the radio button as checked\.

#### disabled<a name="radio-button-attributes-disabled"></a>

A Boolean switch that, if present, displays the button as disabled and prevents it from being checked\.

#### name<a name="radio-button-attributes-name"></a>

A string that is used to identify the answer submitted by the worker\. This value will match a key in the JSON object that specifies the answer\.

**Note**  
If you use the buttons outside of a [crowd\-radio\-group](sms-ui-template-crowd-radio-group.md) element, but with the same `name` string and different `value` strings, the `name` object in the output will contain a Boolean value for each `value` string\. To ensure that only one button in a group is selected, make them children of a [crowd\-radio\-group](sms-ui-template-crowd-radio-group.md) element and use different name values\.

#### value<a name="radio-button-attributes-value"></a>

A property name for the element's boolean value\. If not specified, it uses "on" as the default, e\.g\. `{ "<name>": { "<value>": <true or false> } }`\.

### Element Hierarchy<a name="radio-button-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Parent elements**: [crowd\-radio\-group](sms-ui-template-crowd-radio-group.md)
+ **Child elements**: none

### Output<a name="radio-button-output"></a>

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

### See Also<a name="radio-button-see-also"></a>

For more information, see the following\.
+ [Use Amazon SageMaker Ground Truth to Label Data](sms.md)
+ [Crowd HTML Elements Reference](sms-ui-template-reference.md)