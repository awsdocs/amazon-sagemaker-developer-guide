# crowd\-form<a name="sms-ui-template-crowd-form"></a>

The form wrapper for all custom tasks\. Sets and implements important actions for the proper submission of your form data\.

If a [crowd\-button](sms-ui-template-crowd-button.md) of type "submit" is not included inside the `<crowd-form>` element, it will automatically be appended within the `<crowd-form>` element\. 

The following is an example of an image classification template that uses the `<crowd-form>` element\. Copy the following code and save it in a file with the extenion `.html`\. Open the file in any browser to preview and interact with this template\. 

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
        </short-instructions>

 
        <full-instructions header="Classification Instructions">
            <p>Read the task carefully and inspect the image.</p>
            <p>Choose the appropriate label that best suits the image. 
            Use the <b>None of the Above</b> option if none of the other labels suit the image.</p>
        </full-instructions>

    </crowd-image-classifier>
</crowd-form>
```

### Element Hierarchy<a name="form-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Parent elements**: none
+ **Child elements**: Any of the [UI Template](sms-ui-template-reference.md) elements

### Element Events<a name="form-element-events"></a>

The `crowd-form` element extends the [standard HTML `form` element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/form) and inherits its events, such as `onclick` and `onsubmit`\.

### See Also<a name="form-see-also"></a>

For more information, see the following\.
+ [Use Amazon SageMaker Ground Truth to Label Data](sms.md)
+ [Crowd HTML Elements Reference](sms-ui-template-reference.md)