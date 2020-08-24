# crowd\-text\-area<a name="sms-ui-template-crowd-text-area"></a>

A field for text input\.

The following is an example of a Liquid template designed to transcribe audio clips that uses the `<crowd-text-area>` element\. Copy the following code and save it in a file with the extenion `.html`\. Open the file in any browser to preview and interact with this template\. 

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>

<crowd-form>
  <audio controls>
      <source src="{{ task.input.taskObject | grant_read_access }}" type="audio/mpeg">
      Your browser does not support the audio element.
  </audio>
  <h3>Instructions</h3>
  <p>Transcribe the audio</p>
  <p>Ignore "umms", "hmms", "uhs" and other non-textual phrases</p>
  <crowd-text-area name="transcription" rows="4"></crowd-text-area>
</crowd-form>
```

### Attributes<a name="text-area-attributes"></a>

The following attributes are supported by this element\.

#### auto\-focus<a name="text-area-attributes-auto-focus"></a>

A Boolean switch that, if present, puts the cursor in this element on\-load so that users can immediately begin typing without having to click inside the element\.

#### auto\-validate<a name="text-area-attributes-auto-validate"></a>

A Boolean switch that, if present, turns on input validation\. The behavior of the validator can be modified by the *error\-message* and *allowed\-pattern* attributes\.

#### char\-counter<a name="text-area-attributes-char-counter"></a>

A Boolean switch that, if present, puts a small text field beneath the lower\-right corner of the element, displaying the number of characters inside the element\.

#### disabled<a name="text-area-attributes-disabled"></a>

A Boolean switch that, if present, displays the input area as disabled\.

#### error\-message<a name="text-area-attributes-error-message"></a>

The text to be displayed below the input field, on the left side, if validation fails\.

#### label<a name="text-area-attributes-label"></a>

A string that is displayed inside a text field\.

This text shrinks and rises up above a text field when the worker starts typing in the field or when the *value* attribute is set\.

#### max\-length<a name="text-area-attributes-max-length"></a>

An integer that specifies the maximum number of characters allowed by the element\. Characters typed or pasted beyond the maximum are ignored\.

#### max\-rows<a name="text-area-attributes-max-rows"></a>

An integer that specifies the maximum number of rows of text that are allowed within a crowd\-text\-area\. Normally the element expands to accommodate new rows\. If this is set, after the number of rows exceeds it, content scrolls upward out of view and a scrollbar control appears\.

#### name<a name="text-area-attributes-name"></a>

A string used to represent the element's data in the output\.

#### placeholder<a name="text-area-attributes-placeholder"></a>

A string presented to the user as placeholder text\. It disappears after the user puts something in the input area\.

#### rows<a name="text-area-attributes-rows"></a>

An integer that specifies the height of the element in rows of text\.

#### value<a name="text-area-attributes-value"></a>

A preset that becomes the default if the worker does not provide input\. The preset appears in a text field\.

### Element Hierarchy<a name="text-area-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Parent elements**: [crowd\-form](sms-ui-template-crowd-form.md)
+ **Child elements**: none

### Output<a name="text-area-output"></a>

This element outputs the `name` as a property name and the element's text contents as the value\. Carriage returns in the text are represented as `\n`\.

**Example Sample output for this element**  

```
[
  {
    "textInput1": "This is the text; the text that\nmakes the crowd go wild."
  }
]
```

### See Also<a name="text-area-see-also"></a>

For more information, see the following\.
+ [Use Amazon SageMaker Ground Truth to Label Data](sms.md)
+ [Crowd HTML Elements Reference](sms-ui-template-reference.md)