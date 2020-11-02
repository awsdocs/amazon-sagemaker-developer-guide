# crowd\-modal<a name="sms-ui-template-crowd-modal"></a>

A small window that pops up on the display when it is opened\. 

The following is an example of the syntax that you can use with the `<crowd-modal>` element\. Copy the following code and save it in a file with the extenion `.html`\. Open the file in any browser to preview and interact with this template\. 

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>

<crowd-modal
link-text = "See Examples"
link-type = "button">
Example Modal Text</crowd-modal>
```

### Attributes<a name="modal-attributes"></a>

The following attributes are supported by this element\.

#### link\-text<a name="modal-attributes-link-text"></a>

The text to display for opening the modal\. The default is "Click to open modal"\.

#### link\-type<a name="modal-attributes-link-type"></a>

A string that specifies the type of trigger for the modal\. The possible values are "link" \(default\) and "button"\.

### Element Hierarchy<a name="modal-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Parent elements**: [crowd\-form](sms-ui-template-crowd-form.md)
+ **Child elements**: none

### See Also<a name="modal-see-also"></a>

For more information, see the following\.
+ [Use Amazon SageMaker Ground Truth to Label Data](sms.md)
+ [Crowd HTML Elements Reference](sms-ui-template-reference.md)