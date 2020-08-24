# crowd\-input<a name="sms-ui-template-crowd-input"></a>

A box that accepts input data\.

**Cannot be self\-closing**  
Unlike the `input` element in the HTML standard, this element cannot be self\-closed by putting a slash before the ending bracket, e\.g\. `<crowd-input ... />`\. It must be followed with a `</crowd-input>` to close the element\.

The following is an example of a Liquid template that uses the `<crowd-input>` element\. Copy the following code and save it in a file with the extenion `.html`\. Open the file in any browser to preview and interact with this template\. 

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>

<crowd-form>
  <img style="max-width: 35vw; max-height: 50vh" src="{{ task.input.taskObject | grant_read_access }}">
  <crowd-input name="tag1" label="Word/phrase 1" required></crowd-input>
  <crowd-input name="tag2" label="Word/phrase 2" required></crowd-input>
  <crowd-input name="tag3" label="Word/phrase 3" required></crowd-input>

  <short-instructions>
    Your custom quick instructions and examples
  </short-instructions>

  <full-instructions>
    Your custom detailed instracutions and more examples
  </full-instructions>
</crowd-form>
```

### Attributes<a name="input-attributes"></a>

The following attributes are supported by this element\.

#### allowed\-pattern<a name="input-attributes-allowed-pattern"></a>

A regular expression that is used with the *auto\-validate* attribute to ignore non\-matching characters as the worker types\.

#### auto\-focus<a name="input-attributes-auto-focus"></a>

When the value is set to true, the browser places focus inside the input area after loading\. This way, the worker can start typing without having to select it first\.

#### auto\-validate<a name="input-attributes-auto-validate"></a>

A Boolean switch that, if present, turns on input validation\. The behavior of the validator can be modified by the *error\-message* and *allowed\-pattern* attributes\.

#### disabled<a name="input-attributes-disabled"></a>

A Boolean switch that, if present, displays the input area as disabled\.

#### error\-message<a name="input-attributes-error-message"></a>

The text to be displayed below the input field, on the left side, if validation fails\.

#### label<a name="input-attributes-label"></a>

A string that is displayed inside a text field\.

This text shrinks and rises up above a text field when the worker starts typing in the field or when the *value* attribute is set\.

#### max\-length<a name="input-attributes-max-length"></a>

 A maximum number of characters the input will accept\. Input beyond this limit is ignored\.

#### min\-length<a name="input-attributes-min-length"></a>

A minimum length for the input in the field

#### name<a name="input-attributes-name"></a>

 Sets the name of the input to be used in the DOM and the output of the form\.

#### placeholder<a name="input-attributes-placeholder"></a>

A string value that is used as placeholder text, displayed until the worker starts entering data into the input, It is not used as a default value\.

#### required<a name="input-attributes-required"></a>

A Boolean switch that, if present, requires the worker to provide input\.

#### type<a name="input-attributes-type"></a>

Takes a string to set the HTML5 `input-type` behavior for the input\. Examples include `file` and `date`\.

#### value<a name="input-attributes-value"></a>

A preset that becomes the default if the worker does not provide input\. The preset appears in a text field\.

### Element Hierarchy<a name="input-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Parent elements**: [crowd\-form](sms-ui-template-crowd-form.md)
+ **Child elements**: none

### Output<a name="input-element-output"></a>

Provides a `name` string as the property name, and the text that was entered in the field as its value\.

**Example : Sample JSON Output**  
The values for multiple elements are output in the same object, with their `name` attribute value as their property name\. Elements with no input do not appear in the output\. For example, let's use three inputs:  

```
<crowd-input name="tag1" label="Word/phrase 1"></crowd-input>
<crowd-input name="tag2" label="Word/phrase 2"></crowd-input>
<crowd-input name="tag3" label="Word/phrase 3"></crowd-input>
```
This is the output if only two have input:  

```
[
  {
    "tag1": "blue",
    "tag2": "red"
  }
]
```
This means any code built to parse these results should be able to handle the presence or absence of each input in the answers\.

### See Also<a name="input-see-also"></a>

For more information, see the following\.
+ [Use Amazon SageMaker Ground Truth to Label Data](sms.md)
+ [Crowd HTML Elements Reference](sms-ui-template-reference.md)