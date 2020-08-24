# crowd\-radio\-group<a name="sms-ui-template-crowd-radio-group"></a>

A group of radio buttons\. Only one radio button within the group can be selected\. Choosing one radio button clears any previously chosen radio button within the same group\. For an example of a custom UI template that uses the `crowd-radio-group` element, see this [entity recognition labeling job custom template](https://github.com/aws-samples/amazon-sagemaker-ground-truth-task-uis/blob/master/text/named-entity-recognition-with-additional-classification.liquid.html)\.

The following is an example of the syntax that you can use with the `<crowd-radio-group>` element\. Copy the following code and save it in a file with the extenion `.html`\. Open the file in any browser to preview and interact with this template\. 

```
  
<script src="https://assets.crowd.aws/crowd-html-elements.js"></script>

<style>
	body {
		padding-left: 20px;
		margin-bottom: 20px;
	}
	#outer-container {
	    display: flex;
	    justify-content: space-around;
	    max-width: 900px;
	    margin-left: 100px;
	}
	.left-container {
    	margin-right: auto;
    	padding-right: 50px;
	}
	.right-container {
    	margin-left: auto;
    	padding-left: 50px;
	}
	#vertical-separator {
	    border: solid 1px #d5dbdb;
	}
</style>

<crowd-form>
    <div>
        <h1>Instructions</h1>
	Lorem ipsum...
    </div>
    <div>
        <h2>Background</h2>
    	<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.</p>
    </div>
    <div id="outer-container">
	<span class="left-container">
	    <h2>Option 1</h2>
	    <p>Nulla facilisi morbi tempus iaculis urna. Orci dapibus ultrices in iaculis nunc sed augue lacus.</p>
	</span>
	<span id="vertical-separator"></span>
	<span class="right-container">
	    <h2>Option 2</h2>
	    <p>Ultrices vitae auctor eu augue ut. Pellentesque massa placerat duis ultricies lacus sed turpis tincidunt id.</p>
	</span>
    </div>
    <div>
        <h2>Question</h2>
    	<p>Which do you agree with?</p>
	<crowd-radio-group>
	    <crowd-radio-button name="option1" value="Option 1">Option 1</crowd-radio-button>
	    <crowd-radio-button name="option2" value="Option 2">Option 2</crowd-radio-button>
	</crowd-radio-group>

    	<p>Why did you choose this answer?</p>
	<crowd-text-area name="explanation" placeholder="Explain how you reached your conclusion..."></crowd-text-area>
    </div>
</crowd-form>
```

### Attributes<a name="radio-group-attributes"></a>

No special attributes are supported by this element\.

### Element Hierarchy<a name="radio-group-element-hierarchy"></a>

This element has the following parent and child elements\.
+ **Parent elements**: [crowd\-form](sms-ui-template-crowd-form.md)
+ **Child elements**: [crowd\-radio\-button](sms-ui-template-crowd-radio-button.md)

### Output<a name="radio-group-output"></a>

Outputs an array of objects representing the [crowd\-radio\-button](sms-ui-template-crowd-radio-button.md) elements within it\.

**Example Sample of Element Output**  

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

### See Also<a name="radio-group-see-also"></a>

For more information, see the following\.
+ [Use Amazon SageMaker Ground Truth to Label Data](sms.md)
+ [Crowd HTML Elements Reference](sms-ui-template-reference.md)