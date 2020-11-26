# crowd\-toggle\-button<a name="sms-ui-template-crowd-toggle-button"></a>

A button that acts as an ON/OFF switch, toggling a state\. The following example shows different ways you can use to use the `<crowd-toggle-button>` HTML element\. Copy the following code and save it in a file with the extenion `.html`\. Open the file in any browser to preview and interact with this template\. 

```
<crowd-form>
  <!--Toggle button without value-->
  <crowd-toggle-button name="toggleButtonWithoutValue"></crowd-toggle-button>

  <!--Toggle button with value-->
  <crowd-toggle-button name="toggleButtonWithValue" value="someValue"></crowd-toggle-button>

  <!--Toggle button disabled-->
  <crowd-toggle-button name="toggleButtonDisabled" disabled></crowd-toggle-button>

  <!--Toggle button marked invalid-->
  <crowd-toggle-button name="toggleButtonInvalid" invalid></crowd-toggle-button>

  <!--Toggle button marked required-->
  <crowd-toggle-button name="toggleButtonRequired" required></crowd-toggle-button>
</crowd-form>
```

### Attributes<a name="toggle-button-attributes"></a>

The following attributes are supported by this element\.

#### checked<a name="toggle-button-attributes-checked"></a>

A Boolean switch that, if present, displays the button switched to the ON position\.

#### disabled<a name="toggle-button-attributes-disabled"></a>

A Boolean switch that, if present, displays the button as disabled and prevents toggling\.

#### invalid<a name="toggle-button-attributes-invalid"></a>

When in an off position, a button using this attribute, will display in an alert color\. The standard is red, but may be changed in CSS\. When toggled on, the button will display in the same color as other buttons in the on position\.

#### name<a name="toggle-button-attributes-name"></a>

A string that is used to identify the answer submitted by the worker\. This value matches a key in the JSON object that specifies the answer\.

#### required<a name="toggle-button-attributes-required"></a>

A Boolean switch that, if present, requires the worker to provide input\.

#### value<a name="toggle-button-attributes-value"></a>

A value used in the output as the property name for the element's Boolean state\. Defaults to "on" if not provided\.

### Element Hierarchy<a name="toggle-button-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Parent elements**: [crowd\-form](sms-ui-template-crowd-form.md)
+ **Child elements**: none

### Output<a name="toggle-button-output"></a>

This element outputs the `name` as the name of an object, containing the `value` as a property name and the element's state as Boolean value for the property\. If no value for the element is specified, the property name defaults to "on\."

**Example Sample output for this element**  

```
[
  {
    "theToggler": {
      "on": true
    }
  }
]
```

### See Also<a name="toggle-button-see-also"></a>

For more information, see the following\.
+ [Use Amazon SageMaker Ground Truth to Label Data](sms.md)
+ [Crowd HTML Elements Reference](sms-ui-template-reference.md)