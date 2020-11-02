# crowd\-toast<a name="sms-ui-template-crowd-toast"></a>

A subtle notification that temporarily appears on the display\. Only one crowd\-toast is visible\.

The following is an example of a Liquid template that uses the `<crowd-toast>` element\. Copy the following code and save it in a file with the extenion `.html`\. Open the file in any browser to preview and interact with this template\. 

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>

<crowd-form>
  <p>Find the official website for: <strong>{{ task.input.company }}</strong></p>
  <p>Do not give Yelp pages, LinkedIn pages, etc.</p>
  <p>Include the http:// prefix from the website</p>
  <crowd-input name="website" placeholder="http://example.com"></crowd-input>

  <crowd-toast duration="10000" opened>
    This is a message that you want users to see when opening the template. This message will disappear in 10 seconds. 
   </crowd-toast>

</crowd-form>
```

### Attributes<a name="toast-attributes"></a>

The following attributes are supported by this element\.

#### duration<a name="toast-attributes-duration"></a>

A number that specifies the duration, in milliseconds, that the notification appears on the screen\.

#### text<a name="toast-attributes-text"></a>

The text to display in the notification\.

### Element Hierarchy<a name="toast-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Parent elements**: [crowd\-form](sms-ui-template-crowd-form.md)
+ **Child elements**: none

### See Also<a name="toast-see-also"></a>

For more information, see the following\.
+  [Use Amazon SageMaker Ground Truth to Label Data](sms.md) 
+ [Crowd HTML Elements Reference](sms-ui-template-reference.md)