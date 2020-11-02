# crowd\-icon\-button<a name="sms-ui-template-crowd-icon-button"></a>

A button with an image placed in the center\. When the user touches the button, a ripple effect emanates from the center of the button\.

The following is an example of a Liquid template designed for image classification that uses the `<crowd-icon-button>` element\. This template uses JavaScript to enable workers to report issues with the worker UI\. Copy the following code and save it in a file with the extenion `.html`\. Open the file in any browser to preview and interact with this template\. 

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>
<crowd-form>
    <crowd-image-classifier 
        src="${image_url}"
        categories="['Cat', 'Dog', 'Bird', 'None of the Above']"
        header="Choose the correct category for the image"
        name="category">


        <short-instructions>
            <p>Read the task carefully and inspect the image.</p>
            <p>Choose the appropriate label that best suits the image.</p>
            <p>If there is an issue with the image or tools, please select
              <b>None of the Above</b>, describe the issue in the text box and click the 
              button below.</p>
            <crowd-input label="Report an Issue" name="template-issues"/></crowd-input>
            <crowd-icon-button id="button1" icon="report-problem" title="Issue"/>
        </short-instructions>
        
        <full-instructions header="Classification Instructions">
            <p>Read the task carefully and inspect the image.</p>
            <p>Choose the appropriate label that best suits the image. 
            Use the <b>None of the Above</b> option if none of the other labels suit the image.</p>
        </full-instructions>

    </crowd-image-classifier>
</crowd-form>

<script>
  [
    button1,
  ].forEach(function(button) {
    button.addEventListener('click', function() {
      document.querySelector('crowd-form').submit();
    });
  });
</script>
```

### Attributes<a name="icon-button-attributes"></a>

The following attributes are supported by this element\.

#### disabled<a name="icon-button-attributes-disabled"></a>

A Boolean switch that, if present, displays the button as disabled and prevents clicks\.

#### icon<a name="icon-button-attributes-icon"></a>

A string that specifies the icon to be displayed in the center of the button\. The string must be either the name of an icon from the open\-source *[iron\-icons](https://github.com/PolymerElements/iron-icons)* set, which is pre\-loaded, or the URL to a custom icon\.

The following is an example of the syntax that you can use to add an iron\-icon to a `<crowd-icon-button>` HTML element\. Replace `icon-name` with the name of the icon you'd like to use from this [Icons set](https://www.webcomponents.org/element/@polymer/iron-icons/demo/demo/index.html)\. 

```
<crowd-icon-button id="button1" icon="icon-name" title="Issue"/>
```

### Element Hierarchy<a name="icon-button-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Parent elements**: [crowd\-form](sms-ui-template-crowd-form.md)
+ **Child elements**: none

### See Also<a name="icon-button-see-also"></a>

For more information, see the following\.
+ [Use Amazon SageMaker Ground Truth to Label Data](sms.md)
+ [Crowd HTML Elements Reference](sms-ui-template-reference.md)