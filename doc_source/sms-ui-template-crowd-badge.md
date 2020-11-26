# crowd\-badge<a name="sms-ui-template-crowd-badge"></a>

An icon that floats over the top right corner of another element to which it is attached\.

The following is an example of a template that uses the `<crowd-badge>` element\. Copy the following code and save it in a file with the extenion `.html`\. Open the file in any browser to preview and interact with this template\. 

```
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>

<crowd-form>
  <crowd-image-classifier
    name="crowd-image-classifier"
    src="https://unsplash.com/photos/NLUkAA-nDdE"
    header="Choose the correct category for this image."
    categories="['Person', 'Umbrella', 'Chair', 'Dolphin']"
  >
    <full-instructions header="Classification Instructions">
      <p>Read the task carefully and inspect the image.</p>
      <p>Choose the appropriate label that best suits the image.</p>
    </full-instructions>
   
    <short-instructions id="short-instructions">
      <p>Read the task carefully and inspect the image.</p>
      <p>Choose the appropriate label that best suits the image.</p>
      <crowd-badge icon="star" for="short-instructions"/>
    </short-instructions>
  </crowd-image-classifier>
</crowd-form>
```

### Attributes<a name="badge-attributes"></a>

The following attributes are supported by this element\.

#### for<a name="badge-attributes-for"></a>

A string that specifies the ID of the element to which the badge is attached\.

#### icon<a name="badge-attributes-icon"></a>

A string that specifies the icon to be displayed in the badge\. The string must be either the name of an icon from the open\-source *[iron\-icons](https://github.com/PolymerElements/iron-icons)* set, which is pre\-loaded, or the URL to a custom icon\.

This attribute overrides the *label* attribute\. 

The following is an example of the syntax that you can use to add an iron\-icon to a `<crowd-badge>` HTML element\. Replace `icon-name` with the name of the icon you'd like to use from this [Icons set](https://www.webcomponents.org/element/@polymer/iron-icons/demo/demo/index.html)\. 

```
<crowd-badge icon="icon-name" for="short-instructions"/>
```

#### label<a name="badge-attributes-label"></a>

The text to display in the badge\. Three characters or less is recommended because text that is too large will overflow the badge area\. An icon can be displayed instead of text by setting the *icon* attribute\.

### Element Hierarchy<a name="badge-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Parent elements**: [crowd\-form](sms-ui-template-crowd-form.md)
+ **Child elements**: none

### See Also<a name="badge-see-also"></a>

For more information, see the following\.
+ [Use Amazon SageMaker Ground Truth to Label Data](sms.md)
+ [Crowd HTML Elements Reference](sms-ui-template-reference.md)