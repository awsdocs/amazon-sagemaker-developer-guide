# crowd\-radio\-button<a name="sms-ui-template-crowd-radio-button"></a>

A button that can be either checked or unchecked\. When radio buttons are inside a radio group, exactly one radio button in the group can be checked at any time\. The following is an example of how to configure a `crowd-radio-button` element inside of a `crowd-radio-group` element\.

See an interactive example of an HTML template that uses this Crowd HTML Element in [CodePen](https://codepen.io/sagemaker_crowd_html_elements/pen/yLgyWGZ)\.

The following is an example of the syntax that you can use with the `<crowd-radio-button>` element\. Copy the following code and save it in a file with the extension `.html`\. Open the file in any browser to preview and interact with this template\. 

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>
<crowd-form>
<crowd-radio-group>
    <crowd-radio-button name="tech" value="tech">Technology</crowd-radio-button>
    <crowd-radio-button name="politics" value="politics">Politics</crowd-radio-button>
</crowd-radio-group>
</crowd-form>
```

The previous example can be seen in a custom worker task template in this GitHub example: [entity recognition labeling job custom template](https://github.com/aws-samples/amazon-sagemaker-ground-truth-task-uis/blob/master/text/named-entity-recognition-with-additional-classification.liquid.html)\.

Crowd HTML Element radio buttons do not support the HTML tag, `required`\. To make a radio button selection required, use `<input type="radio">` elements to create radio buttons and add the `required` tag\. The `name` attribute for all `<input>` elements that belong to the same group of radio buttons must be the same\. For example, the following template requires the user to select a radio button in the `animal-type` group before submitting\.

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>
<crowd-form>
  <p>Select an animal type:</p>
<img src="https://images.unsplash.com/photo-1537151608828-ea2b11777ee8?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1539&q=80" style="height: 500; width: 400;"/>
<br><br>
<div>
  <input type="radio" id="cat" name="animal-type" value="cat" required>
  <label for="cat">Cat</label>
</div>
<div>
  <input type="radio" id="dog" name="animal-type" value="dog">
  <label for="dog">Dog</label>
</div>
<div>
  <input type="radio" id="unknown" name="animal-type" value="unknown">
  <label for="unknown">Unknown</label>
</div>
    <full-instructions header="Classification Instructions">
      <p>Read the task carefully and inspect the image.</p>
      <p>Choose the appropriate label that best suits the image.</p>
    </full-instructions>
    <short-instructions>
      <p>Read the task carefully and inspect the image.</p>
      <p>Choose the appropriate label that best suits the image.</p>
    </short-instructions>
</crowd-form>
```

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